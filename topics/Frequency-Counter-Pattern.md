# Frequency Counter Pattern

JavaScript solution

```Javascript
function same(arr1, arr2) {
  if (arr1.length !== arr2.length) {
    return false;
  }
  
  let frequencyCounter1 = {};
  let frequencyCounter2 = {};

  for (let val of arr1) {
    frequencyCounter1[val] = (frequencyCounter1[val] || 0) + 1;
  }
	
  for (let val of arr2) {
    frequencyCounter2[val] = (frequencyCounter2[val] || 0) + 1;
  }

  for (let key in frequencyCounter1) {
    if (!(key ** 2 in frequencyCounter2)) {
      return false;
    }

    if (frequencyCounter2[key ** 2] !== frequencyCounter1[key]) {
      return false;
    }
  }

  return true;
}

console.log(same([1, 1, 2, 3], [1, 4, 9, 7]));

```

This code consists of a function named 'same', which accepts two arrays (arr1 and arr2) as parameters. Its purpose is to check if the two input arrays are the 'same'.

Here is a simplified explanation of this function:

1. Initially, the function checks if the length of two input arrays is the same. If not, it returns false, as this means that the two arrays cannot be 'same'.

2. Two objects named 'frequencyCounter1' and 'frequencyCounter2' are set up to keep track of the frequency of each element in two arrays respectively.

3. Then the function iterates over each array using a 'for...of' loop. For each element in the array, the function increments the corresponding frequency counter of that element by one in the respective counter object. If the element does not exist in the object, it is added with a frequency of 1.

4. After that, the function enters another loop using 'for...in', this time over the properties of 'frequencyCounter1'. In this loop, the function checks two conditions:
   - If the square of the current key doesn't exist in 'frequencyCounter2', it returns false, signifying the two input arrays are not 'same'.
   - If the frequency of the square of the current key in 'frequencyCounter2' is not the same as the frequency of the current key in 'frequencyCounter1', it also returns false.

5. If the function hasn't returned false by the end of these checks, that means the two arrays passed all the checks, and it returns true - the arrays are 'same'.

In the final line of the code, it tests this 'same' function with the arrays [1, 1, 2, 3] and [1, 4, 9, 7]. But the function will most certainly return false, because the elements and their frequencies in the first array and their squares and frequencies in the second array don't match.