# Greedy Methods

Usually used to solve optimization problems, where we are asked to optimize some value and calculate it, or see if the optimal path is possible. Refer to [Optimization Patter](./Patterns/optimization.md) for examples.

Some key words: `min/max, longest/shortest, largest/smallest` etc.

Some example problems:
1. Return the minimum number of swaps required to make s1 and s2 equal, or return -1 if it is impossible to do so.
2. Return the minimum number of steps to make s and t anagrams of each other.
3. Given three integers a, b, and c, return the longest possible happy string, if there are multiple happy strings, return any of them, if there is no such string return empty string.

## Advantages
- Simple to code
- Doesn't use complex algorithms (BFS or DFS for example)
- runtime is fast, usually `O(n)` or `O(nlogn)`

## Template to solve using greedy method

- Greedy algorithms follow this format:
  - identify what choices you have at every step
    - find the best choice and perform it
    - just do what looks best until done
  - assume best choice at every step leads to final best answer

While you can use a formal math proof to check if greedy approach works, you can use intuition in practical settings (like an interview).

Steps to build the intuition of whether greedy would work:

1. Identify the different steps and choices of a problem: a) choices have to be comparable, and b) must determine if the choice is better than another
2. Check if greedy works: a) try finding small cases where greedy isn't optimal b) if cases pass, try to figure out why greedy is optimal 

## Maximum Number of Non-Overlapping Intervals on an Axis

## Fractional Knapsack