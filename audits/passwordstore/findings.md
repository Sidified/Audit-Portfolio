### [H-1] Storing the Password On-Chain Makes It Publicly Accessible

**Description:**

All data stored on-chain is publicly accessible and can be read by anyone. The `PasswordStore::s_password` variable is intended to remain private and only be accessible to the owner through the `PasswordStore::getPassword` function.

**Impact:**

Any user can read the password directly from blockchain storage, completely compromising the protocol's intended privacy guarantees.

**Proof of Concept (PoC):**

The following steps demonstrate how any user can read the password directly from storage.

1. Start a local Anvil chain.

```bash
make anvil
```

2. Deploy the contract.

```bash
make deploy
```

3. Read storage slot `1`.

We use slot `1` because `s_password` is stored at that location.

```bash
cast storage <ADDRESS_HERE> 1 --rpc-url http://127.0.0.1:8545
```

Example output:

```text
0x6d7950617373776f726400000000000000000000000000000000000000000014
```

This value represents the raw bytes stored at slot `1`. It can be decoded using Foundry's `cast parse-bytes32-string` command.

```bash
cast parse-bytes32-string 0x6d7950617373776f726400000000000000000000000000000000000000000014
```

Output:

```text
myPassword
```

**Recommended Mitigation:**

The current architecture should be reconsidered, as secrets cannot be securely stored on-chain.

One possible approach is to encrypt the password off-chain and store only the encrypted value on-chain. However, this would require users to securely manage the decryption key off-chain. Additionally, the `getPassword` function should likely be removed, as exposing encrypted data through a retrieval function provides limited value and may encourage unsafe usage patterns.


### [H-2] Missing Access Control in `PasswordStore::setPassword` Allows Unauthorized Password Changes

**Description:**

The `PasswordStore::setPassword` function is declared as `external` and contains no access-control checks. However, both the contract's intended functionality and its NatSpec documentation indicate that only the owner should be able to update the password.

```javascript
function setPassword(string memory newPassword) external {
    // @Audit - Missing access control
    s_password = newPassword;
    emit SetNewPassword();
}
```

**Impact:**

Any user can modify the stored password, breaking the contract's intended access restrictions and functionality.

**Proof of Concept (PoC):**

Add the following test to `PasswordStore.t.sol`:

<details>
<summary>Code</summary>

```javascript
function test_anyone_can_set_password(address randomAddress) public {
    vm.assume(randomAddress != owner);

    vm.startPrank(randomAddress);

    string memory expectedPassword = "myNewPassword";
    passwordStore.setPassword(expectedPassword);

    vm.startPrank(owner);

    string memory actualPassword = passwordStore.getPassword();
    assertEq(actualPassword, expectedPassword);
}
```

</details>

The test demonstrates that a non-owner can successfully update the password.

**Recommended Mitigation:**

Restrict access to the contract owner.

```javascript
if (msg.sender != s_owner) {
    revert PasswordStore__NotOwner();
}
```

Alternatively, consider using OpenZeppelin's `Ownable` implementation and the `onlyOwner` modifier.


### [I-1] Incorrect NatSpec Documentation for `getPassword`

**Description:**

The NatSpec documentation for `getPassword` includes a parameter description for a parameter that does not exist.

```javascript
/*
 * @notice This allows only the owner to retrieve the password.
 * @param newPassword The new password to set.
 */
function getPassword() external view returns (string memory) {
```

The function signature is `getPassword()` and does not accept any parameters. As a result, the NatSpec documentation is inaccurate.

**Impact:**

Incorrect documentation may confuse developers, auditors, integrators, and users interacting with the codebase.

**Recommended Mitigation:**

Remove the incorrect NatSpec parameter entry.

```diff
- * @param newPassword The new password to set.
```
