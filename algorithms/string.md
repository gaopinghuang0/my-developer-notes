## String searching/matching
* Rabin-Karp algorithm, which uses hashing to find an exact match of a pattern string in a text. The key is to use a rolling hash, which can compute new hash value based on previous one. A good rolling hash function is Rabin fingerprint. [Wikipedia](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm) uses a polynomial approach, which is simpler and equally good. A practical application of the algorithm is detecting plagiarism. Another example is [Leetcode 1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/).
* Boyer-Moore
* KMP algorithm for pattern searching. [A video about the longest prefix suffix array](https://www.youtube.com/watch?v=tWDUjkMv6Lc&feature=youtu.be).
* Real use case: [Adblock Plus: investigating filter matching algorithms](https://adblockplus.org/blog/investigating-filter-matching-algorithms)
