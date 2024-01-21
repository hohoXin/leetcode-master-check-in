# leetcode 28. Implement strStr()
## Thinking Reocrd:
Idea:\
Brute Forece:\
We can search for all of the beginging letter of needle in the haystack and check the whole needle.

KMP:\
Pre-process lps:\
Assuming we are trying to calculate the lps[i]. The string pointer is point to i. And we use prev to store the value at lps[i-1].\
Which means at least at (i-1)th, we can achieve prev as the longest_border. This prev number of elements are the same as the prev number of element at the begining.\
When string[i]== string[prev], which means both from the begining and the end, we have (prev+1) same element. So we can let the prev += 1, let the lps[i] = prev, let pointer++.\
When string[i] != string[prev], we then try prev = lps[prev-1].\
Which means even though we can make prev+1 equal to the begining. We can try the longest border within the prev length string, and use it as the new prev.\
That's a lps[prev-1] length number of element. So we change the prev = this new length and try the matching again.\
If the string[i] == new string[prev], we can use the same logic above, we have (prev+1) same element. So we can let the prev += 1, let the lps[i] = prev, let pointer++.\
If not, let the prev = lps[prev-1], try the maximum posible border. Again and again. Try the prev+1 first element with the last prev+1 element.\
But after the last comparing failure, when the prev = 0, we cannot find any smaller border before the string[i]. We have already counter the smallest prev. We can only say lps[i] = 0, prev = 0.\
After deciding the lps[i], we can move our pointer in the string to the next. In this way, we will get the lps array.\
The initiation is lps[0] = 0, we start the i from 1 and prev from 0.

Searching:\
After we find the lps of our little string, we can do the KMP searching on the large string.\
Let the L_pointer walk through the large string, and let the S_pointer check the small string.\
If L_pointer == S_pointer, we can move forward both pointer for letter by letter checking. L_pointer++, S_pointer++.\
If L_pointer != S_pointer, we can go back to all of the element before the S_pointer to find the lps. Which means for the begining S_pointer number of elements, at least the last lps[S_pointer-1] are equal to the begining lps[S_pointer-1] element, we can shrink the S_pointer = lps[S_pointer-1], and start the L_pointer, S_pointer again.\
During this shrink, we gurantee we didn't ignore any possible overlap situation. And whenver there is a match we can continue our pointers checking agian. L_pointer++, S_pointer++.\
Since the lps[S_pointer-1] will eventually shrink to 0 if we always get an unmatch in our problem. When the S_pointer = 0, means we restart a comparing from the begining of the small string.\
The initation state is L_pointer = S_pointer = 0. The L_pointer will never roll back.\
We travel through the whole large string. When we find our S_pointer meet its end, we find out the first match in the large string.

TBD:\
What is the worst case? How to calculate the time complexity?


## Submissions:
![Alt text](../__assets__/pic/lc_28_1.png?raw=true "lc_28_1")

## Summary from the Lecture:
solution source: [leetcode-editorial-solution](https://leetcode.com/problems/implement-strstr/solution/)

### Approach 2: Rabin Karp Algorithm (Single Hash)
They play a lot of trick on using the MOD to avoid overflow.
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        m = len(needle)
        n = len(haystack)
        if n < m:
            return -1

        # CONSTANTS
        RADIX = 26
        MOD = 1_000_000_033
        MAX_WEIGHT = 1

        for _ in range(m):
            MAX_WEIGHT = (MAX_WEIGHT * RADIX) % MOD

        # Function to compute the hash of m-String
        def hash_value(string):
            ans = 0
            factor = 1

            for i in range(m - 1, -1, -1):
                ans += ((ord(string[i]) - 97) * (factor)) % MOD
                factor = (factor * RADIX) % MOD

            return ans % MOD

        # Compute the hash of needle
        hash_needle = hash_value(needle)

        # Check for each m-substring of haystack, starting at window_start
        for window_start in range(n - m + 1):
            if window_start == 0:
                # Compute hash of the First Substring
                hash_hay = hash_value(haystack)
            else:
                # Update Hash using Previous Hash Value in O(1)
                hash_hay = ((hash_hay * RADIX) % MOD
                            - ((ord(haystack[window_start - 1]) - 97)
                            * MAX_WEIGHT) % MOD
                            + (ord(haystack[window_start + m - 1]) - 97)
                            + MOD) % MOD

            # If hash matches, Check Character by Character. 
            # Because of Mod, spurious hits can be there.
            if hash_needle == hash_hay:
                for i in range(m):
                    if needle[i] != haystack[i + window_start]:
                        break
                if i == m - 1:
                    return window_start

        return -1
```

### Approach 2: Rabin Karp Algorithm (Double Hash)
We can produce different hash values by changing

MOD value

RADIX value

MAPPING value

a to 0, b to 1, c to 2, and so on.\
a to 1, b to 2, c to 3, and so on.\
a to 25, b to 24, c to 23, and so on.\
WEIGHTAGE associated with characters of the string. There can be two or more versions

Rightmost character has weightage 1, then the second rightmost character has weightage RADIX, and so on. The leftmost character will have weightage RADIX^(m−1) \
Leftmost character will have weightage 1, then the second leftmost character will have weightage RADIX, and so on. The rightmost character will have weightage RADIX^(m−1) \
We can make one or more changes (Change MOD, RADIX as well as MAPPING) for producing a second value for Pair. We can even use three techniques for hash value, generating triplet for string.

Mathematically, we can prove that by using different MOD and RADIX, we can reduce the CHANCE (i. e. probability) of spurious hits to 10^(−10)

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        m = len(needle)
        n = len(haystack)

        if n < m:
            return -1

        # CONSTANTS
        RADIX_1 = 26
        MOD_1 = 10**9+33
        MAX_WEIGHT_1 = 1
        RADIX_2 = 27
        MOD_2 = 2**31-1
        MAX_WEIGHT_2 = 1

        for _ in range(m):
            MAX_WEIGHT_1 = (MAX_WEIGHT_1 * RADIX_1) % MOD_1
            MAX_WEIGHT_2 = (MAX_WEIGHT_2 * RADIX_2) % MOD_2

        # Function to compute hash_pair of m-String
        def hash_pair(string):
            hash_1 = hash_2 = 0
            factor_1 = factor_2 = 1
            for i in range(m - 1, -1, -1):
                hash_1 += ((ord(string[i]) - 97) * (factor_1)) % MOD_1
                factor_1 = (factor_1 * RADIX_1) % MOD_1
                hash_2 += ((ord(string[i]) - 97) * (factor_2)) % MOD_2
                factor_2 = (factor_2 * RADIX_2) % MOD_2

            return [hash_1 % MOD_1, hash_2 % MOD_2]

        # Compute hash pairs of needle
        hash_needle = hash_pair(needle)

        # Check for each m-substring of haystack, starting at window_start
        for window_start in range(n - m + 1):
            if window_start == 0:
                # Compute hash pairs of the First Substring
                hash_hay = hash_pair(haystack)
            else:
                # Update Hash pairs using Previous using O(1) Value
                hash_hay[0] = (((hash_hay[0] * RADIX_1) % MOD_1
                               - ((ord(haystack[window_start - 1]) - 97)
                                  * (MAX_WEIGHT_1)) % MOD_1
                               + (ord(haystack[window_start + m - 1]) - 97))
                               % MOD_1)
                hash_hay[1] = (((hash_hay[1] * RADIX_2) % MOD_2
                               - ((ord(haystack[window_start - 1]) - 97)
                                  * (MAX_WEIGHT_2)) % MOD_2
                               + (ord(haystack[window_start + m - 1]) - 97))
                               % MOD_2)

            # If the hash matches, return immediately.
            # Probability of Spurious Hit tends to zero
            if hash_needle == hash_hay:
                return window_start
        return -1
```


## Complexity Analysis:
### Approach 1: Sliding Window
- Time complexity: O(nm).\
 For every window_start, we may have to iterate at most m times. There are n-m+1 such window_start. Thus, it is O((n-m+1)*m) = O(nm).\
 One example where the worst case time complexity occurs is when you have a string of the form "aaa...aab" and a pattern of the form "aaa...aac". Here, we will have to match all but the last character of the string with the pattern.\
- Space complexity: O(1).\
There are a handful of variables in code (m, n, i, window_start), and all of them use constant space, hence, the space complexity is constant.

### Approach 2: Rabin Karp Algorithm (Single Hash)
- Time complexity: O(nm).\
In the worst case, hashNeedle might match with the hash value of all haystack substrings. Hencen, we then have to iterate character by character in each window. There are n-m+1 such windows of length m. Hence, the time complexity is O((n-m+1)*m) = O(nm).\
But in the best case, if no hash value of haystack substring matches with hashNeedle, then we don't have to iterate character by character in each window. In that case, it will be O(n+m). Computing the hash value of haystack and needle will be O(m) and for taversing all windows, we will have O(n-m) time complexity. And during each traversal, we are doing constant number of operations, hence, in that case, it will be O(n-m+2m)=O(n+m).\
- Space complexity: O(1). We are not using any extra space.

### Approach 3: Rabin Karp Algorithm (Double Hash)
- Time complexity: O(n).\
For computing hash pairs of needle, we have to do O(m) operations.\
For checking for a match, we have to iterate over n−m+1 times. Out of these n−m+1 operations, we have to do O(1) work for n−m times and O(m) work for 1 time.\ 
Hence, the total time complexity is O(n−m+1)⋅O(1)+O(m)=O(n−m+1)+O(m)=O(n+m).\
Moreover, we are proceeding only when n >= m, thus final time complexity is O(n) only. In this case, O(m+n) has an upper bound of O(2*n), that's why we can ignore the m term. When n < m, we are simply returning -1. Thus, only n is dominating the time complexity, and not m.\
- Space complexity: O(1). We are not using any extra space.

### Approach 4: Knuth-Morris-Pratt Algorithm
- Time complexity: O(n).\
If n < m, then we immediately return -1. Hence, it is O(1) in this case.\
Otherwise,\
Pre-processing takes O(m) time.\
In the case of "Matching", or "Mismatch (Empty Previous Border)", we simply increment i, which is O(1).\
In the case of "Mismatch (Non-Empty Previous Border)", we reduce prev to longest_border[prev-1]. In other words, we try to reduce at most as many times as the while loop has executed. There will be at most m−1 such reductions.\
Thus, it will be O(m).\
Searching takes O(n) time.\
We never backtrack/reset the haystack_pointer. We increment it by 1 in Matching or Zero-Matching.\
In Partial-Matching, we don't immediately increment and try to reduce to a condition of Matching, or Zero-Matching. For this, we set needle_pointer to longest_border[needle_pointer-1], which always reduces to 0 or matches. The maximum number of rollbacks of needle_pointer is bounded by needle_pointer. For any mismatch, we can only roll back as much as we have advanced up to the mismatch.\
Thus, for searching it is O(2n), which is O(n).\
Hence, it is O(m+n) and since n ≥ mn, we can ignore mmm term. The final upper bound is O(2⋅n), which is O(n).\
Therefore, overall it is O(n)+O(1), which is O(n).\
No worst-case or accidental inputs exist here.
- Space complexity: O(m). To store the longest_border array, we need O(m) extra space. 

## Second Try Submissions:
### Approach : KMP Algorithm
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        # KMP solution
        
        h_len = len(haystack)
        n_len = len(needle)

        if n_len > h_len:
            return -1
        
        h_p = 0
        n_p = 0
        n_lps = self.calLPS(needle)

        while h_p < h_len:
            if haystack[h_p] == needle[n_p]:
                n_p += 1
                h_p += 1
            elif n_p > 0:
                # I forget the - 1 in my second try
                n_p = n_lps[n_p-1]
            elif n_p == 0:
                h_p += 1
            else:
                print("ERROR IN SEARCHING!")
                break
            
            if n_p == n_len:
                return (h_p - n_len)
        
        return -1



    def calLPS(self, s: str) -> List[int]:
        pointer = 1
        prev = 0
        lps = [0] * len(s)

        while pointer < len(s):
            if s[pointer] == s[prev]:
                prev += 1
                lps[pointer] = prev
                pointer += 1
            elif prev > 0:
                # I forget the - 1 in my first try
                prev = lps[prev-1]
            elif prev == 0:
                lps[pointer] = prev
                pointer += 1
            else:
                print("ERROR IN LPS CALCULATION")
                break
        
        return lps
```
![Alt text](../__assets__/pic/lc_28_2.png?raw=true "lc_28_2")



# leetcode 459. Repeated Substring Pattern

## Thinking Reocrd:
Idea:\
Change the matching window. Start from 0 to 1 to 0 to len(s)//2.\
First we can check if the len(s) % len(window) == 0.\
Then we can slice the window to check character by character.

## Submissions:
![Alt text](../__assets__/pic/lc_459_1.png?raw=true "lc_459_1")

## Summary from the Lecture:
video source: [代码随想录-重复的子字符串](https://www.bilibili.com/video/BV1cg41127fw)

Some elegant code:

### Approach 1: Using Divisors
```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        n = len(s)
        for i in range(1, n // 2 + 1):
            if n % i == 0:
                # python way to make a duplicate of a string
                pattern = s[:i] * (n // i)
                if s == pattern:
                    return True
        return False
```

### Approach 2: String Concatenation
```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        t = s + s
        if s in t[1:-1]:
            return True
        return False
```

### Approach 3: KMP Algorithm
```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        n = len(s)
        lps = [0] * n
        i = 1
        length = 0
        while i < n:
            if s[i] == s[length]:
                length += 1
                lps[i] = length
                i += 1
            else:
                if length != 0:
                    length = lps[length - 1]
                else:
                    lps[i] = 0
                    i += 1
        length = lps[n - 1]
        return length != 0 and n % (n - length) == 0
```

## Complexity Analysis:
### Approach 1: Using Divisors
- Time complexity: O(n**(3/2)).\
A number n can have a maximum of 2*n**(1/2) number of divisors. As a result, we would execute the inner loop that concatenates the substring O(n**(1/2)) times. In the inner loop, we concatenate a substring of length i for n / i times to generate a string of length n, which would require O(n) time for each iteration. As a result, it would take O(n**(3/2)) in total.
- Space complexity: O(n).\
We used another string variable, pattern, which is initialized to an empty string before the inner loop iteration and grows up to a length of n after the inner loop iteration.
### Approach 2: String Concatenation
- Time complexity: O(n).\
It takes O(n) time to form string t followed by its substring by eliminating the first and final character.\
Due to implementation differences of the underlying methods, finding s in t takes O(n^2) in Java and Python3 but O(n) in C++. However, one can use algorithms like KMP to solve it in linear time.
- Space complexity: O(n).\
We used a string variable t that takes O(n) space.
### Approach 3: KMP Algorithm
- Time complexity: O(n).
- Space complexity: O(1).

TBD: deeper understanding of KMP algorithm here.