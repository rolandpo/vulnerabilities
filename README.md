# Insufficient gas griefing

In a function with a sub-call to a different contract, the someone could use enough gas for the transaction to execute but the sub-call to fail. Change in state could cause unexpected issue for subsequent calls.

# Reentrancy

Function with an external call to an arbitrary contract can allow reentrancy into the same or related function with outdated state by implementing such a call in the fallback function.

The _checkOnERC721Received call in ERC721._safeMint can be vulnerable to reentrancy.

Single-function reentrancy - a vulnerable function is repeatedly called.
Cross-function reentrancy - a vulnerable function shares state with another function
Read-only reentrancy - a different contract which reads state from the original contract is reentered

# Overflows and underflows

Overflows and underflow revert from Solidity 0.8 onwards, but can still allow DOS attacks. Reverting can be bypassed with a unchecked block. Older version of Solidity can use the SafeMath library to prevent unexpected overflows/underflows.

Typecasting a larger value to a smaller uint data type can cause overflows without reverting. Overflows/underflows are also possible with shift operations as well as inline assembly.

# Authorisation through tx.origin

tx.origin should never be used for access control since users can be manipulated into calling the fallback function of a malicious contract. msg.sender should be used instead.

# Outdated compiler version

Bugs can exist due to an outdated Solidity compiler version.

# Floating pragma

A specific Solidity version should be chosen, to minimise the risks of deploying through a problematic version introducing unexpected bugs.

# Unsafe low-level call

Low-level calls return false instead of throwing an exception, while failed contract calls throw. Therefore, the return value of low-level calls should be checked to prevent unexpected behaviour.

Since a call to a non-existant contract is always considered successful by the EVM, Solidity uses the extcodesize opcode to check if the contract exists. Since low-levels calls operate on addresses rather than contract istances, this check is skipped, so verifying the existence of the contract is important to prevent unexpected behaviour.

# Unchecked return value

External calls can be made using .send(), .call() or .transfer(). The .send() and .call() function return a boolean and do not revert on a failure, while .transfer() does.

# DoS with revert

DoS attacks can occur when a function tries sending funds to an address but a fallback function reverts all payments. A function could implement iterating over multiple addresses to be paid, but one malicious address could cause all payments to revert. Other DoS attacks can be caused by overflows/underflows, funds directly send to the contract without being recorded by internal accounting or division by 0.

# Unsupported opcodes

Opcode support can vary across EVM-compatible chains, which can lead to bugs if these differences are not accounted for.

# Assert violation

A failing assert() function steals gas, as opposed to require() and revert(), and should only be used for checking invariants.

# Deprecated functions

Deprecated functions can lead to unexpected behaviour and compilation errors.

# DelegateCall to untrusted callee

Delegatecall is a low-level function that executes code of target contract with the storage, msg.sender and msg.value of the calling contract. Checks should be in place to make sure the target contract is not user-defined.

# Signature malleability

Due to the eliptic curve being symmetric about the x-axis, each valid signature has a complementary signature, possible to be calculated without knowing the private key from which it is derived. This means that checking for whether a signature has been used is not sufficient in protecting against malicious activity. If the signature is part of a signed message hash, previously signed messages could be replayed.

# Shadowing state variables

Variables of the same name can have diffrent values across multiple contracts, which can lead to unintended behaviour if one contract inherits from another with the inherited variable value overriden.

# Weak randomness

Pseudorandom variables, such as block.timestamp, blockhash and block.prevrandao are not reliable for generating randomness since the blockchain is entirely deterministic. Randomness should be generated using oracle solutions, such as Chainlink VRF.

# Signature replay attacks

To protect against signature replay attacks, only new hashes should be processed. Every processed message hash should be stored and compared to new one. The address of the contract should be included in the message hash to avoid the message being used in other contracts. The message hash should never be generated using the signature because of signature malleability.

# Writing to arbitrary storage locations

Authorization checks should be in place to prevent overwriting sensitive data.

# Hash collision with abi.encodePacked()

When multiple variable-length arguments are used in abi.encodePacked(), two different calls can produce the same output since the function doesn't differentiate between multiple arguments and multiple elements of an array or string. This can lead to potential vulnerabilities, such as exploiting signature verification. Avoiding using variable-length arguments in abi.encodePacked() or using abi.encode() can mitigate the issue.

# Incorrect inheritance order

When a contracts inherits from multiple contracts, the compiler linearises the inheritance from left to right. If multiple inherited contracts implement or override the same function signature, the right-most contract is prioritised for calling that function.

# Unencrypted private data

Contract code and storage is always public and can be read. Variables marked as private are only inaccessible to other contracts. Commit-reveal schemes and zero-knowledge proofs can be used to protect sensitive data.



# Other

ERC3156 flash loan
USDC blacklist


# Checklist

- Vault assets/shares conflation
- no recipient address validation on public claim functions
- checks at start of function that should be valid at the end
