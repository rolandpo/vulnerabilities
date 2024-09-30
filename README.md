# Insufficient gas griefing

In a function with a sub-call to a different contract, the someone could use enough gas for the transaction to execute but the sub-call to fail. Change in state could cause unexpected issue for subsequent calls.