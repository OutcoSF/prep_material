A number of problems involving arrays, lists, and strings require us to iterate with more than one pointer. Sometimes pointers may travel at different speeds and may start from a different initial position.

For example, an algorithm that requires one to swap a bunch of values may require multiple pointers.

### Language Specific

*   Languages like Java, C, C++, and Javascript, have `for` loops that are more flexible and can be easily customized to handle two or more pointers. But universally, a `while` loop will give the same flexibility for all languages.
*   Languages like Ruby or Python can use `while` loop to handle looping with multiple pointers.
*   When there are multiple pointers to keep track of, avoid using built in iterators that simply loop from left to right.
*   Lets demonstrate the use of while loops with a few examples.

### Example 1 : Sort a Bit Array

Given a bit array, return it sorted in-place (a bit array is simply an array that contains only bits, either a 1 or a 0).

See if you can solve this in O(N) time and O(1) auxiliary space.

#### Hint 1

Since we want to sort it in-place we should be modifying the values change changing its position rather than creating a new array. Additionally because there are only two possible values, and we know that the 0’s will have to be on the left, and the ones have to be on the right. How can we identify elements to swap?

#### Hint 2

1.  If we have two pointers: one that starts on the very **left**, and one on the very **right**, then we can iterate inward.
2.  We need to iterate the **left** pointer until it hits a 1
3.  Then decrement the **right** pointer to the left until it reaches a 0
4.  Once we find them we will do a swap.

#### Solution

<div>

<pre class="line-numbers"><code class="language-javascript">function bitArraySort(arr) {
    let left = 0;
    let right = arr.length - 1;
    while(left < right) {
      while(arr[left] === 0) { left++; }
      while(arr[right] === 1) { right--; }
      if(left < right) {
         // swap the left and right values
        [arr[left], arr[right]] = [arr[right], arr[left]];
      }
    }
    return arr;
}</code>
	</pre>

</div>

### Challenge 1 : Sorted Two Sum

Given a sorted array of integers and a target value, determine if there exists two integers in the array that sum up to the target value.

See if you can solve this in O(N) time and O(1) auxiliary space.

#### Brute Force

The brute force approach would be, to try out every single possible pair combination and see if the sum matches the target. This would lead to a O(N^2) time complexity because the number of pairs is proportional to N^2.

#### Frequency Hash

We could also try to create a hashtable or map to speed up the search or lookup time to find a match. But this would lead to a O(N) amount of auxiliary space.

#### Hint 1

Given that the array is sorted, we know every element right is equal or larger and every element to the left is lower. How could we approach this in a more efficiently with multiple pointers?

#### Hint 2

1.  If we have two pointers: one that starts on the very **left**, and one on the very **right**, we can find the sum of them.
2.  What happens if this sum is greater? What happens if this sum is less than? And what happens if this sum is equal?
3.  What happens if the two pointers eventually meet and we have not found a pair that matches the target?

### Challenge 2 : Merge Two Sorted Arrays

Given two sorted arrays of integers, combine the values into one sorted array?

**Input:**`[1,3,5], [2,4,6,8,10]`

**Output:** `[1,2,3,4,5,6,8,10]`

See if you can solve this in O(N+M) time and O(N+M) auxiliary space.

#### Brute Force

The brute force approach would be, to concatenate the two arrays into a larger array and perform a sort on the combined array. However the time complexity would be quasilinear O((N+M)log(N+M)) if we used a efficient sorting method such as mergesort.

#### Hint 1

Given that the arrays are sorted, we know the lowest element must either be the first item of array one or the first item of array two.

#### Hint 2

1.  If we have two pointers: one that starts at the beginning of **array1**, and one pointer at the beginning of **array2**, we can place the smaller value into a **results** array.
2.  What happens if the value in **array1** is smaller? What happens if the value in **array2** is smaller? What happens if the values are equal?
3.  What happens if one of the pointers reaches the very end of its array?
