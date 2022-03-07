# Validate Subsequence

## Prompt

Given two non-empty arrays of integers, write a function that determines
  whether the second array is a subsequence of the first one.

A subsequence of an array is a set of numbers that aren't necessarily adjacent
  in the array but that are in the same order as they appear in the array. For
  instance, the numbers
  
**subsequnce** a sequence that can be derived from another sequence by deleting
some or no elements without changing the order of the remaining elements.

All of the integers have to appear and in the correct order. 
But not necessarily in sequence

## Input/output table

| Input array  | Input sequence  | Output |
|---|---|---|
| [5, 1, 22, 25, 6, -1, 8, 10]  | [1,6,-1,10] | true |
| [5, 1, 22, 25, 6, -1, 8, 10]  | [1] | true |
| [5, 1, 22, 25, 6, -1, 8, 10]  | [5, 1, 22, 25, 6, -1, 8, 10] | true |
| [5, 1, 22, 25, 6, -1, 8, 10] | [5, 1, 22, 25, 6, -1, 8, 10, 12] | false |

## Potential approach

* Definitely have to traverse both of the arrays. It's possible that the last element in our array will be the last element in our sequence. Like in our input above.

Let's create a pointer on the subsequence array by looking at one element.

[1,6,-1,10]

Start with `1`

Okay, we found the first element `1` in our main sequence at index 2.

Next, we move our pointer of the subsequence to `6` and start searching from the current location in the main array.
We don't need to restart the search in the main array because the subsequence has to be in order.
Hence, starting from the beginning is unecessary.
Once we finish traversing the subsequence, we can stop.
In the subsequence, only move forward when we find a match

## Solution

```c#
using System;
using System.Collections.Generic;

public class Program {
	// O(n) time, because we depend on the size of the main array
	// O(1) space since we don't store anything or create any objects
	public static bool IsValidSubsequence(List<int> array, List<int> sequence) {
		int arrIdx =0;
		int seqIdx=0;
		while(arrIdx < array.Count && seqIdx < sequence.Count){
			// if the value in the array == value in the sequence
			if (array[arrIdx] == sequence[seqIdx]){
				seqIdx++;
			}
			// otherwise, increment only the main array index
			arrIdx++;
		}
		// if the seqIdx has been incremented to traverse entire sequence, then 
		// the sequence is part of the array
		// otherwise, this will be false
		return seqIdx == sequence.Count;
	}
}
```

## Time and space complexity

Time = O(N) because the time depends on the size of our main array. Even though sometimes we don't need to traverse the entire array.

Space = O(1) we're not storing anything extra, any big pieces of data, nothing that increases in size.
