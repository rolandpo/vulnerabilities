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

