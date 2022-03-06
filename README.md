# algorithms

Let's practice coding algorithms together!

## Caesar Cipher Encryptor

### Prompt

Given a non-empty string of lowercase letters and a non-negative integer
  representing a key, write a function that returns a new string obtained by
  shifting every letter in the input string by k positions in the alphabet,
  where k is the key.
  
Make sure you wrap around the alphabet
  
### Sample input

```c#
string = "xyz"
key = 2
```

### Sample output

```c#
"zab"
```

### Initial planning

1. way to implement this function is to use ASCII values. For example

```c#
int asciCode = (int)'a';
string asciStr = asciCode.ToString();
//output is 97
// z would be 122
```

So to shift the letter by your key, we can do

```c#
int shiftedLetter = ((int)'a') + 2;
// this will return 99
```

However, the crux is the wrapping that has to happen to return to the start of the alphabet. The solution would look like this.

```c#
int asciCode = ((int)'z') + 2;
if(asciCode <= 122)
	return asciCode.ToString();
else
	return ((char)(96 + asciCode % 122)).ToString();
```

`asciCode % 122` means that we're dividing something like 124/122 and getting a remainder of 2. Wrapping 2 means that our letter is going to be `b`. Hence, we
need to add the 2 to 96 to get to `b`.

#### What happens if the `key` is a really large number like 54?

54 = 26 + 26 + 2. So this means that the answer should be the same as above. But, `((int)'z') + 54 = 176`. 

`176 % 122 = 54` which is still greater than the 2 that we want to get to `b`.

Hence, our first step needs to be to mod the key, like this

```c#
// now the key will return the remainder, which is what we want
int key = key % 26;
int asciCode = ((int)'z') + key;
if(asciCode <= 122)
	return asciCode.ToString();
else
	return ((char)(96 + asciCode % 122)).ToString();
```

### Solution

```c#
public static string getShiftedValue(char character, int key)
{
	int asciCode = ((int)character) + key;
	if(asciCode <= 122)
		return ((char)asciCode).ToString();
	else
		return ((char)(96 + asciCode % 122)).ToString();
}
public static string CaesarCypherEncryptor(string str, int key) {
	// Write your code here.
	string newStr="";
	key = key % 26;
	var charArray = str.ToCharArray();
	foreach(var character in charArray){
		newStr += getShiftedValue(character, key);
	}
	return newStr;
}
```

### Solution B

What if we didn't want to use the `char` cast? Well we could create an array of values ourselves like this

```c#
var letterDictionary = new Dictionary<char letter, int index>();
letterDictionary.Add('a', 0);
letterDictionary.Add('b', 1);
...
letterDictionary.Add('z', 25);
```

Hence, we'd need to update our algorithm a bit to look like this

ℹ️ Let's revisit this implementation in the future as it's not fully finished

```c#
public static string getShiftedValue(char character, int key)
{
	int incrementedChar = letterDictionary[character] + key;
	if(incrementedChar <= 25)
		return letterDictionary[character].ToString();
	else
		return letterDictionary[character % 26].ToString();
}

public static string CaesarCypherEncryptor(string str, int key) {
	// Write your code here.
	string newStr="";
	key = key % 26;
	var charArray = str.ToCharArray();
	foreach(var character in charArray){
		newStr += getShiftedValue(character, key);
	}
	return newStr;
}
```

Time= O(n) where n is the length of the string

Space= O(n) 

With an alphabet, our length is only 26. If our alphabet size changed, then we would have an O(m).

## Subarray Sort

### Prompt

  Write a function that takes in an array of at least two integers and that
  returns an array of the starting and ending indices of the smallest subarray
  in the input array that needs to be sorted in place in order for the entire
  input array to be sorted (in ascending order).

### Sample input

```c#
[1,2,4,7,10,11,7,12,6,7,16,18,19]
```

### Sample output

```c#
[3,9]
```

If we sort the input array above starting at index 3 aka 7 to index 9 aka 7, then the entire array will be sorted.

### Input/output table

| Input  | Expected output  |
|---|---|
| [1, 2, 4, 7, 10, 11, 7, 12, 6, 7, 16, 18, 19]  | [3, 9]  |
| [1, 2]  | [-1, -1]  |
| [2, 1]  | [0, 1]  |
| [1, 2, 4, 7, 10, 11, 7, 12, 13, 14, 16, 18, 19]  | [4, 6]  |

### Notes

* There are always at least 2 numbers that are not sorted if 1 number is out of place. For example, [1,2,3,10,5,7], the 10 and the 5 need to move even though only the 10 is out of place.
* Just having a single number out of place could force a huge sub-array to need sorting. For example [1,2,3,4,-1]. The -1 is out of place here. If we wanted to move it to the correct location of -1, then we would need to sort the entire array.

[1,2,3,4,-1,7,10] - what part of this array would need to be sorted? Still the same as before [1,2,3,4,-1]. It's the [size of the smallest unsorted value and it's correct position, the value of the largest unsorted number]

So:

1. We need to find all of the unsorted values
2. Find the smallest unsorted value and it's sorted position in the array
3. Find the largest unsorted value and it's final position in the array
4. This will help to create the start and end of the array that needs to be sorted

### An algorithm attempt

```
[0,1,2,3,4,  5,6,7, 8,9,10,11,12]
[1,2,4,7,10,11,7,12,6,7,16,18,19]
```
 ✅✅✅ ✅ ✅ ❌ ❌❌ ❌ ✅ ✅ ✅ ✅

Sorted (✅) means that the number before is smaller and the number after is bigger.

Unsorted (❌)

Now, we need to get the lowest and highest values that are unsorted. So, starting from the left 6 should come between [4,7]. Next, starting from the right, 12 should come between [7,16]. Hence, this gives us indices 3 to 9 that need to be sorted.

Space= O(1), it's constant space
Time= O(n), where n is the length of the input array. We just need a single pass through the array

### Solution A

```c#
using System;

public class Program {
	public static int[] SubarraySort(int[] array) {
		// Write your code here.
		int minOutOfOrder = Int32.MaxValue;
		int maxOutOfOrder = Int32.MinValue;
		
		// this for loop is O(n) operation, n is the length of the array
		for(int i=0; i < array.Length; i++){
			int num = array[i];
			if(isOutOfOrder(i, num, array)){
				// we wanted to have the max value here initially so that any number evaluates to less than that
				// Min() returns the min from 2 ints
				minOutOfOrder = Math.Min(minOutOfOrder, num);
				//we wanted the min value so that on the first pass, any value will be larger 
				//than initial maxOutOfOrder
				maxOutOfOrder = Math.Max(maxOutOfOrder, num);
			}
		}
		// our array is already sorted if our default values haven't changed
		if (minOutOfOrder == Int32.MaxValue){
			return new int[] {-1, -1};
		}
		int subarrayLeftIdx = 0;
		// 6 is our minOutOfOrder
		// starting at index 0 we move right while the condition is true
		// at one point, we reach 7 at index 3, so subarrayLeftIdx=3
		// because it's incremented at 1,2,4 and exits while loop at 7
		// O(n) operation as it's only dependent on array length
		while (minOutOfOrder >= array[subarrayLeftIdx]){
			subarrayLeftIdx++;
		}
		// same logic as for the minOutOfOrder but we start on the right and work our way left
		int subarrayRightIdx = array.Length - 1;
		// O(n) operation as it's only dependent on array length
		while (maxOutOfOrder <= array[subarrayRightIdx]){
			subarrayRightIdx--;
		}
		
		return new int[] {subarrayLeftIdx, subarrayRightIdx};
	}
	
	public static bool isOutOfOrder(int i, int num, int[] array){
		// assert that the number is greater than the next one in the array
		// if so, then it's outOfOrder
		if(i==0){
			return num > array[i+1];
		}
		// if i is the last one in the array
		if(i==array.Length -1){
			return num < array[i-1];
		}
		// return true if our num is greater than the num on the right, or less than the num on the left
		return num > array[i+1] || num < array[i-1];
	}
}

```

### Test cases using above algorithm

Input
```
{
  "array": [1, 2, 4, 7, 10, 11, 7, 12, 6, 7, 16, 18, 19]
}
```
Output
```
[3, 9]
```
