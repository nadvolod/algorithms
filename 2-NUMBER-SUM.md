# 2 Number Sum

## Prompt

Write a function that takes in a non-empty array of **distinct** integers and an
  integer representing a target sum. If any two numbers in the input array sum
  up to the target sum, the function should return them in an array, in any
  order. If no two numbers sum up to the target sum, the function should return
  an empty array.

| Input  | Expected output  |
|---|---|
| [3,5,11,-1,6], targetSum=10  | [-1, 11]  |
| [1, 2], targetSum=10  | []  |
| [1,2,3], targetSum=2  | [] |

## Approach

1. Traverse the array from left to right and store the value
2. Inside of that, traverse the array from right to left and store that value
3. If leftStart + rightStart = targetSum then return new int[] {leftStartIdx, rightStartIdx};
4. `distinct` integers are great for a `HashSet`

## Solution A

Easiest but not the most time efficient solution

```c#
public class Program {
	public static int[] TwoNumberSum(int[] array, int targetSum) {
		for(int i=0; i < array.Length; i++){
			int leftVal = array[i];
			// this is the better solution, starting at i+1 instead
			// this ensures that a number will never be added to itself
			// while still making sure that all the combinations are tested
			for(int j=i+1; j < array.Length; j++){
				int rightVal = array[j];
				if(leftVal + rightVal == targetSum)
					return new int[] {leftVal, rightVal};
			}
		}
		return new int[0];
	}
}
```

Time: O(n^2), 1 loop inside of another. So this isn't the most optimal

Space: O(1), constant space because we aren't using any extra objects

## Solution B, better optimized for time

Time: O(n), since it's just a single loop

Space: O(n) since we're storing values in a hash table

```c#
using System.Collections.Generic;

public class Program {
	public static int[] TwoNumberSum(int[] array, int targetSum) {
		HashSet<int> nums = new HashSet<int>();
    // runtime of O(n) for this loop
		foreach (int num in array){
			int potentialMatch = targetSum - num;
			if(nums.Contains(potentialMatch)){
				return new int[] {potentialMatch, num};
			} else {
				nums.Add(num);
			}
		}
		return new int[0];
	}
}
```
