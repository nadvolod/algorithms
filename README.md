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
