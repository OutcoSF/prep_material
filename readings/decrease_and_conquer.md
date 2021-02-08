You may have heard of a foundational algorithm technique called [Divide and Conquer](https://en.wikipedia.org/wiki/Divide_and_conquer_algorithm). In this section, we will focus on its lesser known cousin Decrease and Conquer.

Lets clarify the difference now:

**Divide and Conquer**:

*   divides a problem into **two or more** subproblems.
*   the subproblems are solved independently
*   (optionally) the results are combined back together.
*   implemented with multiple recursion or a `stack` and `while` loop
*   e.g., Quicksort, Mergesort, [Strassen matrix multiplication](https://en.wikipedia.org/wiki/Strassen_algorithm)

**Decrease and Conquer**:

*   reduces a problem to a **single** smaller subproblem.
*   the reduction can be by a constant value or a constant factor.
*   implemented with a `while` loop (stack often is not necessary with a single subproblem) or single recursion
*   e.g., Binary Search, Quickselect, [Russian Peasant Multiplication](http://www.cut-the-knot.org/Curriculum/Algebra/PeasantMultiplication.shtml)

We will focus on **decrease and conquer** for now and introduce divide and conquer in a later section. Lets cover few algorithms using decrease and conquer to reduce the problem by a constant or variable factor.

Note: Decrease and conquer includes reduction by a constant value as well (subtract by a value), however we will focus on reduction by a factor (division by a factor).

### <a id="Example_1_Binary_Search_23"></a>Example 1: Binary Search

Given a sorted array of unique integers, and a target value determine the index of a matching value within the array. If there is not match, return `-1`.

Input: `[1,3,4,5,6,7,8,10,11,13,15,17,20,22]`, `17`

Output: `11`

Binary search is a foundational search algorithm used to find the index of a value within a sorted array. It converges at a solution in logarithmic time O(log(N)) which is quicker than performing a linear search. Binary search uses a process of elimination to discard of the potential locations where a value could be.

#### <a id="Dictionary_Analogy_33"></a>Dictionary Analogy

![Dictionary](https://s24.postimg.org/kq5fju645/Untitled_11.png)  
Think about how one searches for the definition of a word within an English dictionary. We can begin a picking a page in the middle of the book. Then check whether the word is on the page. If it isn’t there, the word has to be either before or after this page. We deduce from alphabetical order which half to continue the search.

Next we split the appropriate section in half again and look for the word in the new page. We repeat until either the current page has the target word, or we find that the word does not exist in the dictionary.

#### <a id="Pseudocode_39"></a>Pseudocode

<pre><code>1\. Start with the full range of the array (0 to array length - 1).
2\. Check the value at the middle of that range.
2\. If mid matches target we return the mid index.
3\. If the mid is larger than target we can eliminate the right half.
4\. If the mid is less than target, we can eliminate the left half.
5\. Adjust the range depending on which half if still active.
6\. Repeat steps 2-5 until a match is found or the range is empty
7\. If the range is empty, return -1.
</code>
</pre>

#### <a id="Hint_1_52"></a>Hint 1

Consider using a while loop (instead of recursion) to simplify the logic. What step in the pseudo code above would be the while condition?

#### <a id="Hint_2_55"></a>Hint 2

When reducing the active range, remember to also eliminate the mid index each time as well.

#### <a id="Solution_58"></a>Solution

<pre><code class="language-javascript"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">binarySearch</span>(<span class="hljs-params">arr, target</span>) </span>{
    <span class="hljs-keyword">let</span> start = <span class="hljs-number">0</span>;
    <span class="hljs-keyword">let</span> end = arr.length - <span class="hljs-number">1</span>;
    <span class="hljs-keyword">let</span> mid;
    <span class="hljs-keyword">while</span>(start <= end) {
      mid = <span class="hljs-built_in">Math</span>.floor((start + end) / <span class="hljs-number">2</span>);
      <span class="hljs-keyword">if</span>(arr[mid] === target) { <span class="hljs-keyword">return</span> mid; }
      <span class="hljs-keyword">if</span>(target < arr[mid]) {
        end = mid - <span class="hljs-number">1</span>;
      } <span class="hljs-keyword">else</span> {
        start = mid + <span class="hljs-number">1</span>;
      }
    }
    <span class="hljs-keyword">return</span> -<span class="hljs-number">1</span>;
}
</code>
</pre>

### <a id="Example_2_Greatest_Common_Divisor_GCD_78"></a>Example 2: Greatest Common Divisor (GCD)

Given two integers, find the greatest common divisor (GCD).

Input: `52, 78`

Output: `26`

In mathematics, the GCD of two integers is the largest positive integer that is a factor of both integers. In the case both `52` and `78` are divisible by `26`. Which also happens to be the largest common factor as well.

#### <a id="Prime_Factorization_88"></a>Prime Factorization

One potential approach is to find all the prime factors of each number. In the example case above, we get:

`52` has prime factors `[2, 2, 13]`  
`78` has prime factors `[2, 3, 13]`

Multiply all the prime factors common to both integers. In this case, `2` and `13` are common therefore `2 x 13 = 26` is the GCD.

Finding all the prime factors for both input integers and then computing the product of all the common factors can take more time than necessary. Lets explore a faster approach.

#### <a id="Euclids_Algorithm_Decrease_and_Conquer_99"></a>Euclid’s Algorithm (Decrease and Conquer)

[Euclids’s algorithm](https://en.wikipedia.org/wiki/Greatest_common_divisor#Using_Euclid.27s_algorithm) uses decrease an conquer to converge at the GCD faster than the prime factorization approach. The basis for Euclid’s algorithm is that the GCD of two numbers must be a factor of its _difference_ as well.

In the example above: `78 - 52 = 26`, the GCD must be a factor of `26` as well. Another way to put it is the `GCD(78, 52)` must also equal to the `GCD(52, 26)` and/or the `GCD(78, 26)`.

Solving the smaller subproblem `GCD(52, 26)` we can apply the difference again `52 - 26 = 26` and reduce the problem further to `GCD(26,26)`. The GCD of two equal numbers that the number itself so we get `GCD(26,26) = 26` (since the GCD must be positive, if the equal pair is negative, take the absolute value).

So instead of finding all the divisors or prime factors of two really large values, lets find the _difference_ between them to reduce the problem to smaller inputs. If we keep finding the difference the problem reduces to the point where the inputs are much smaller.

#### <a id="Use_Modulo_instead_of_Subtract_109"></a>Use Modulo instead of Subtract

Finding the difference is fine, but finding the modulo which gives the remainder is even faster. This is because remainder is the result performing multiple subtractions in one shot. Lets see how to approach the algorithm.

#### <a id="Pseudocode_113"></a>Pseudocode

Given integers A and B, where A > B, return its GCD.

<pre><code>1\. Save B as a temp variable.
2\. Set B equal to A % B.
3\. Set A equal to the temp.
4\. Repeat steps 1-3 until B equals 0.
5\. Return A.
</code>
</pre>

#### <a id="Resources_122"></a>Resources

To get a deeper understanding, check out a proof on Euclid’s Algorithm [here](https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/the-euclidean-algorithm) by Khan Academy.

#### <a id="Hint_1_125"></a>Hint 1

Try using a while loop. Which step would be the while condition?

#### <a id="Hint_2_128"></a>Hint 2

To make the pseudocode work for the general case where the first integer can be smaller _or_ larger than the second integer by swapping them if A < B. What if one or more of the values are negative?

#### <a id="Solution_131"></a>Solution

<pre><code class="language-javascript"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">gcd</span>(<span class="hljs-params">a, b</span>)</span>{
  <span class="hljs-comment">// if inputs are negative integers, convert to positive</span>
  <span class="hljs-keyword">if</span>(a < <span class="hljs-number">0</span>) { a *= -<span class="hljs-number">1</span>; }
  <span class="hljs-keyword">if</span>(b < <span class="hljs-number">0</span>) { b *= -<span class="hljs-number">1</span>; }
  <span class="hljs-keyword">let</span> temp;
  <span class="hljs-comment">// swap inputs if a < b</span>
  <span class="hljs-keyword">if</span>(a < b) {
    temp = a;
    a = b;
    b = temp;
  }
  <span class="hljs-comment">// use modulo to reduce the larger value until one number is zero</span>
  <span class="hljs-keyword">while</span>(b > <span class="hljs-number">0</span>) {
    temp = b;
    b = a % b;
    a = temp;
  }
  <span class="hljs-comment">// return the non zero value</span>
  <span class="hljs-keyword">return</span> a;
}
</code>
</pre>

#### <a id="Refactoring_159"></a>Refactoring

In some languages (Python, Ruby, Javascript ES6) swapping can be done a bit more clean, lets refactor using an ES6 feature of JavaScript called [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).

<pre><code class="language-javascript"><span class="hljs-comment">//Given two integers, find the greatest common divisor.</span>
<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">gcd</span>(<span class="hljs-params">a, b</span>)</span>{
  <span class="hljs-comment">// if inputs are negative convert to positive</span>
  <span class="hljs-keyword">if</span>(a < <span class="hljs-number">0</span>) { a *= -<span class="hljs-number">1</span>; }
  <span class="hljs-keyword">if</span>(b < <span class="hljs-number">0</span>) { b *= -<span class="hljs-number">1</span>; }
  <span class="hljs-comment">// swap inputs if a < b</span>
  <span class="hljs-keyword">if</span>(a < b) {
    [a, b] = [b, a];
  }
  <span class="hljs-comment">// use modulo to reduce the larger value until one number is zero</span>
  <span class="hljs-keyword">while</span>(b > <span class="hljs-number">0</span>) {
     <span class="hljs-comment">// set a to b and b to the modulo of a and b</span>
    [a, b] = [b, a % b];
  }
  <span class="hljs-comment">// return the non zero value</span>
  <span class="hljs-keyword">return</span> a;
}
</code>
</pre>

#### <a id="More_Refactoring_185"></a>More Refactoring

Notice if we take the modulo A % B where A < B, we actually just get A back. For example `5 % 10 = 5`. This is because `10` goes into `5` zero times and we get a remainder of `5`.

A further optimization is that we can just remove the initial swap out completely. Because the logic inside the `while` loop takes care of the swap for us if A < B initially. Lets remove all the comments out for clarity.

#### <a id="Final_Solution_191"></a>Final Solution

<pre><code class="language-javascript"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">gcd</span>(<span class="hljs-params">a, b</span>)</span>{
  <span class="hljs-keyword">if</span>(a < <span class="hljs-number">0</span>) { a *= -<span class="hljs-number">1</span>; }
  <span class="hljs-keyword">if</span>(b < <span class="hljs-number">0</span>) { b *= -<span class="hljs-number">1</span>; }
  <span class="hljs-keyword">while</span>(b > <span class="hljs-number">0</span>) { [a, b] = [b, a % b]; }
  <span class="hljs-keyword">return</span> a;
}
</code>
</pre>

### <a id="Challenge_1__Number_of_Ones_in_a_Sorted_Bit_Array_201"></a>Challenge 1 : Number of Ones in a Sorted Bit Array

Given a sorted bit array (values of either 0 or 1), determine the number of 1’s in the array.

Perform this in `O(log(N))` time complexity.

Input: `[0,0,0,1,1,1,1,1,1,1,1]`

Output: `8`

#### <a id="Hint_1_211"></a>Hint 1

If we know the index of the first 1 in the array we can compute the the number of 1’s using the length of the array.

#### <a id="Hint_2_214"></a>Hint 2

Try using a binary search. But what target value would we even be searching for?
