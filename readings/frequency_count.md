Frequency counting is a technique that can be used to solve problems what requires tracking, counting, or looking up values quickly. These type of problems often involve a collection of some sort (i.e., array, hashtable, or string). Often the problem will involve matching, counting, or confirming values in the collection.

#### <a id="Implementation_2"></a>Implementation

To implement a frequency count, we typically uses a hashtable, However for specific cases, we may opt to use a set, or an array.

**Hashtable**: General all purpose use

**Array**: Values in the collection that are of a small range of integer values.

**Set**: If only needed to track if something exists.

To populate our count we will have to loop through our input collection. This leads to a O(N) time and potentially O(N) auxiliary space (if all values are unique). However future lookups can be performed in O(1) time as a result.

Lets look at examples to see its implementation.

### <a id="Example_1_Two_Sum_15"></a>Example 1: Two Sum

Given an array of integers, and a target value determine if there are two integers that add to the sum.

Input: `[4,2,6,5,7,9,10]`, `13`

Output: `true`

#### <a id="Brute_Force_22"></a>Brute Force

This problem looks quite similar to the [_sorted two sum_](http://class.outco.io/courses/technical/lectures/1950839) problem, but the input array is not sorted. If we use a brute force approach we could try every single unique pair in the array. This would solve the problem in O(N^2) time.

Try to solve this problem without using hints first. If you get stuck then use the minimum hints to get yourself unstuck. Goodluck!

#### <a id="Hint_1_27"></a>Hint 1

Try using a hash table (or a set) to store values we have come across to speed up lookup time.

#### <a id="Hint_2_31"></a>Hint 2

1.  As we look through each value in the array, what do we need to check in the hash to know whether there is a sum that matches the target?
2.  If the hash does not contain a matching value, then what should we add to the hash table?
3.  If the hash _does_ contains a matching value, what should we do?
4.  If we finish the loop but have not found a match, what should we return?

#### <a id="Solution_38"></a>Solution

<pre><code class="language-javascript"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">twoSum</span>(<span class="hljs-params">numbers, target</span>)</span>{
  <span class="hljs-keyword">let</span> hash = {};
  <span class="hljs-keyword">let</span> current;
  <span class="hljs-keyword">for</span>(<span class="hljs-keyword">let</span> i = <span class="hljs-number">0</span>; i < numbers.length; i++) {
    current = numbers[i];
    <span class="hljs-keyword">if</span>(hash[current]) { <span class="hljs-keyword">return</span> <span class="hljs-literal">true</span>; }
    hash[target - current] = <span class="hljs-literal">true</span>;
  }
  <span class="hljs-keyword">return</span> <span class="hljs-literal">false</span>;
}
</code>
</pre>

### <a id="Challenge_1_Sort_a_Bit_Array_52"></a>Challenge 1: Sort a Bit Array

Given a bit array, return it sorted in-place (a bit array is simply an array that contains only bits, either 0 or 1).

See if you can solve this in O(N) time and O(1) auxiliary space.

Try to solve this using a frequency count rather than using multiple pointers, or using a comparison sort function.

Input : `[0, 1, 1, 0, 1, 1, 1, 0]`

Output : `[0, 0, 0, 1, 1, 1, 1, 1]`

### <a id="Hint_1_64"></a>Hint 1:

Since there are only two values we could use a two item `array` to keep a count of zeros and ones.

### <a id="Hint_2_68"></a>Hint 2:

After creating and populating a frequency count, how do we use the number of zeros and number of ones to populate the original input `array`.
