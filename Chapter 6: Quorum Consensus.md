# Chapter 6: Quorom Consensus
Quorum Consensus can guarantee consistency for both read and write operations.

N = The number of replicas
W = A write quorum of size W. For a write operation to be considered as successful, write operation must be acknowledged from W replicas.
R = A read quorum of size R. For a read opearation to be considered as successful, read operation must wait for responses from at least R replicas.

W + R > N: Strong Consistency

If R = 1 and W = N, fast read. 

If W = 1 and R = N, fast write.

If W + R > N, strong consistency.

If W + R <= N, weak consistency.

