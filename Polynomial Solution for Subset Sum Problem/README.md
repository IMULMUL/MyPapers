This is a polynomial-time algorithm to solve the Subset Sum Problem.

To test it, add `print(find_solution(18, [1, 6, 6, 4, 5, 12, 15, 21, 53]))` at the end of algorithm.py file and then run it. If you want to benchmark the time-taken for very large input values, you can do something like:
```python
import random, time
required_sum = 117653409467538
array = random.sample(range(1, 500000000), 50000000) # generate 50000000 random numbers between 1 and 499999999
start = time.time()
solution = find_solution(required_sum, array)
print(solution)
print("seconds taken:", time.time() - start)
```

## Overview

Below is a rough description of how the algorithm works. It is not comprehensive (or final) and has only been added to help with initial peer-review and as a proof of invention.

```
Given set of positive integers: S
Target sum: T
Problem: Find a subset of S (if it exists) such that its sum is equal to T.

1. Sort the set S in increasing order.
2. Remove any numbers that are greater than T. Adding anything to them will only result in number larger than T.
3. Iterate over S in a decreasing order while adding the iterated numbers until we get a sum greater than T. The length of subset formed until this point is N i.e. the minimum possible length of the solution subset.
   3.1 If we iterate through S without the sum becoming greater than T, a solution doesn't exist i.e. even the sum of the entire set is less than T.
4. Iterate over S in an increasing order while adding the iterated numbers until we get a sum greater than T. The length of subset formed until this point is M i.e. the maximum possible length of the solution subset.
5. Consider the last N digits of the set. It acts like a buffer (dynamic subset) and its position is from reverse(S) to reverse(S)-N.
   5.1 If the sum of the buffer is greater than T, move to left i.e. to smaller numbers in set.
   5.2 If its sum is equal to T, we have found our solution.
   5.3 If its sum is less than T, proceed to 6.
6. Start iterating over S, add the first (smallest) number of S to the sum of buffer.
   6.1 If this sum is equal to T, we have found our solution.
   6.2 If this sum is greater than T, stop the iteration and go back to 6, and move the buffer one number to the left. This is done because if this number is causing the buffer to be greater than T, the rest of the numbers will do too as they are in increasing order.
   6.2 If this sum is less than then T, reject this number and move to the next number in S.
   6.3 If all the numbers in set resulted in a sum less than T, increase N by 1 and go back to 6. This is done because if all sums for the N-1 largest numbers isn't big enough, the rest of S is only smaller because we are iterating bigges->smallest.
   6.4 If the end of this accquired set is reached, increase N by 1 and go back to 6.
8. Keep repeating steps 5 and 8 until a solution is found or length of buffer reaches M.
```

## Proof (WIP)

Below is WIP mathematical proof that its time-complexity is polynomial:

1. Sorting the array: Sorting an array of length 𝑛 generally takes 𝑂(𝑛 log 𝑛) time.

2. Finding the upper bound of the subset: In the worst case, this step involves iterating over entire the sorted array once, which takes 𝑂(𝑛) time.

3. Finding the lower bound of the subset: Similar to step 2, this step also involves iterating over the sorted array once until a condition is met, taking 𝑂(𝑛) time in worst case.

4. Searching for the subset: Let's denote the length of the array as 𝑛. After sorting, we have upper and lower bounds within which the subset must lie. Let 𝑚 denote the length of the subarray (buffer) we consider in each iteration of the while loop. This 𝑚 is bounded by the difference between the upper and lower bounds, and thus 𝑚 ≤ 𝑛.

Now, within the while loop:

- Operations such as summing elements of sub-arrays and checking conditions are done in constant time 𝑂(1).
- The size of the subarray (buffer) over which we iterate within the while loop can vary, but it's bounded by 𝑛.

Therefore, the time complexity of the while loop is 𝑂(𝑚), where 𝑚 is bounded by 𝑛.

Combining all the steps, the overall time complexity of the algorithm is the sum of the time complexities of the individual steps:

𝑂(𝑛 log 𝑛) + 𝑂(𝑛) + 𝑂(𝑛) + 𝑂(𝑚)

Since 𝑚 is bounded by 𝑛, we can simplify this to:

𝑂(𝑛 log 𝑛) + 𝑂(𝑛) + 𝑂(𝑛) + 𝑂(𝑛) = 𝑂(𝑛 log 𝑛)

Thus, the time complexity of the algorithm is 𝑂(𝑛 log 𝑛).
