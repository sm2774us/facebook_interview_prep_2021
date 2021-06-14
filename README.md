# facebook_interview_prep_2021
* [Facebook Phone Interview Questions](https://leetcode.com/discuss/interview-question/790428/Facebook-Phone-Interview-Questions)
* [Facebook interview experiences - All Combined from LC - Till Date 07-Jun-2020](https://leetcode.com/discuss/general-discussion/675445/facebook-interview-experiences-all-combined-from-lc-till-date-07-jun-2020)
* [Facebook Interviews](https://leetcode.com/list/xyvbjku7/)
* [FB-Phone-Interview-List](https://leetcode.com/list/5h1lvmem/)
* [Cracking the top 40 Facebook coding interview questions](https://www.educative.io/blog/cracking-top-facebook-coding-interview-questions)
* [Facebook Interview Questions and Answers](https://hackr.io/blog/facebook-interview-questions)
* [Import and Useful Links from all over the LeetCode](https://leetcode.com/discuss/general-discussion/665604/important-and-useful-links-from-all-over-the-leetcode)

## 1. Phone Screen:
| No. | LC-#     | Title	                                                                                                 | url                                                                                        | Time                                                       | Space                 | Difficulty | Data_Structure | Algorithm                    | Premium    |
| --- | -------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ | ---------------------------------------------------------- | --------------------- | ---------- | -------------- | ---------------------------- | ---------- |
| 1   | 238      | [Product of Array Except Self](#lc-238product-of-array-except-self)                                       | https://leetcode.com/problems/product-of-array-except-self/                                | _O(n)_                                                     | _O(1)_                | Medium     | Array          |                              |            |
| 2   | 1428     | [Leftmost Column with at Least a One](#lc-1428leftmost-column-with-at-least-a-one/)                       | https://leetcode.com/problems/leftmost-column-with-at-least-a-one/                         | _O(R+C)_ or _O(N+M)_ [ RxC matrix or N*M matrix ]          | _O(1)_                | Medium     | Array/Matrix   | Binary Search                | 🔒          |
| 3   |          | [Leftmost Column Index of 1](#leftmost-column-index-of-1) { Similar to LC-1428 }                          | https://leetcode.com/discuss/interview-question/341247/facebook-leftmost-column-index-of-1 | _O(R*log(C)_ or _O(N*log(M))_ [ RxC matrix or N*M matrix ] | _O(1)_                | Medium     | Array/Matrix   | Binary Search                |            |
| 4   | 240      | [Search a 2D Matrix II](#lc-240search-a-2d-matrix-ii)                                                     | https://leetcode.com/problems/search-a-2d-matrix-ii                                        | _O(M+N)_                                                   | _O(1)_                | Medium     | Array          |                              |            |

#### [LC-238:Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
##### Solution Explanation
```
=================================================================================================================================================================
Approach 1: Left and Right Arrays that capture the multiplication product scanning from left-to-right or right-to-left.
=================================================================================================================================================================
- We maintain a left and right array that captures the multiplication product scanning from left-to-right or right-to-left.
- The time complexity is two linear traversals, thus it's linear time.

- The tricky part is to keep a multiplicative counter with result till its previous element (not its self),
  and assign this value on to its left/right_array.

=================================================================================================================================================================
Approach 2: Optimized Space Solution. Without using extra memory of left and right product list.
=================================================================================================================================================================
Step 1. Create a list that contains the product of all left side elements except the current index of nums element.
Step 2. Create a variable of the right product and multiply with what we have in Step 1 (List that contains all the
        left side produts except the current index itself) through the loop --> this will calculate the product of
        array except for self.
Step 3. Keep updating the right product and loop.
Step 4. Return the answer.
```
##### Complexity Analysis:
```
=================================================================================================================================================================
Approach 1: Left and Right Arrays that capture the multiplication product scanning from left-to-right or right-to-left.
=================================================================================================================================================================
Time complexity : O(N) [ Technically O(2N) ]
========================
We traverse the list containing N elements twice. Each look up in the list costs only O(1) time.

Space complexity : O(1) [ As per problem, the output array does not count as extra space for space complexity analysis. ]
========================
Constant space since we only create a single output array to store the results.

=================================================================================================================================================================
Approach 2: Optimized Space Solution. Without using extra memory of left and right product list.
=================================================================================================================================================================
Time complexity : O(N) [ Technically O(2N) ]
========================
We traverse the list containing N elements twice. Each look up in the list costs only O(1) time.

Space complexity : O(1) [ As per problem, the output array does not count as extra space for space complexity analysis. ]
========================
Constant space since we only create a single output array to store the results.
```
```python
from typing import List
import unittest

class Solution(object):
    #
    # -------------------------------------------------------------------------------------------------------------------------
    # Approach 1: Left and Right Arrays that capture the multiplication product scanning from left-to-right or right-to-left.
    # -------------------------------------------------------------------------------------------------------------------------
    #
    # TC: O(N)
    # SC: O(N)
    def productExceptSelf_Solution_1(self, nums: List[int]) -> List[int]:
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if not nums: return []
        
        l = len(nums)
        left_arr, right_arr, left, right = [1]*l, [1]*l, 1, 1
        
        for i in range(1, l):
            left *= nums[i-1]
            left_arr[i] = left
        
        for j in range(l-2, -1, -1):
            right *= nums[j+1]
            right_arr[j] = right
        
        return [tup[0]*tup[1] for tup in zip(left_arr, right_arr)]

    #
    # -------------------------------------------------------------------------------------------------------------------------
    # Approach 2: Optimized Space Solution. Without using extra memory of left and right product list.
    # -------------------------------------------------------------------------------------------------------------------------
    # TC: O(N)
    # SC: O(1) [ excluding the output/result array, which does not count towards extra space, as per problem description. ]
    def productExceptSelf_Solution_2(self, nums: List[int]) -> List[int]:
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        length_of_list = len(nums)
		result = [0]*length_of_list
		
        # update result with left product.
        result[0] = 1
        for i in range(1, length_of_list):
            result[i] = result[i-1] * nums[i-1]

        right_product = 1
        for i in reversed(range(length_of_list)):
            result[i] = result[i] * right_product
			right_product *= nums[i]

        return result

class Test(unittest.TestCase):
    def setUp(self) -> None:
        pass

    def tearDown(self) -> None:
        pass

    def test_reverseList(self) -> None:
        sol = Solution()
        for nums, solution in (
            [
                [1,2,3,4],
                [24,12,8,6],
            ],
            [
                [-1,1,0,-3,3],
                [0,0,9,0,0]
            ]
        ):
            self.assertEqual(
                sol.productExceptSelf_Solution_1(nums),
                solution
            )
            self.assertEqual(
                sol.productExceptSelf_Solution_2(nums),
                solution
            )

if __name__ == "__main__":
    ##Input: nums = [1,2,3,4]
    ##Output: [24,12,8,6]
    #nums = [1,2,3,4]
    #print(productExceptSelf(nums))
    ##Input: nums = [-1,1,0,-3,3]
    ##Output: [0,0,9,0,0]
    #nums = [-1,1,0,-3,3]
    #print(productExceptSelf(nums))
	unittest.main()
```

<br/>
<div align="right">
    <b><a href="#algorithms">⬆️ Back to Top</a></b>
</div>
<br/>

####  [LC-1428:Leftmost Column with at Least a One](https://leetcode.com/problems/leftmost-column-with-at-least-a-one/)
##### Solution Explanation:
```
=================================================================================================================================================================
 Approach 1: Linear Search Each Row
=================================================================================================================================================================
Intuition
------------------------------------------------------------------
This approach won't pass, but we'll use it as a starting point.
Also, it might be helpful to you if you just needed an example of how to use the API, but don't want to see a complete solution yet!
------------------------------------------------------------------
The leftmost 1 is the 1 with the lowest column index.

The problem can be broken down into finding the index of the first 1 in each row and then taking the minimum of those indexes.

     0   1   2   3   4   5   6   7   8   9
   +---+---+---+---+---+---+---+---+---+---+   +---+
 0 | 0 | 0 | 0 | 0 | 0 | 0 |*1*|   |   |   |-->| 6 |
   +---+---+---+---+---+---+---+---+---+---+   +---+
 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |*1*|   |   |-->| 7 |
   +---+---+---+---+---+---+---+---+---+---+   +---+
 2 | 0 | 0 | 0 | 0 | 0 |*1*|   |   |   |   |-->| 5 |
   +---+---+---+---+---+---+---+---+---+---+   +---+
 3 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |-->|-1 |
   +---+---+---+---+---+---+---+---+---+---+   +---+
 4 | 0 | 0 | 0 | 0 | 0 |*1*|   |   |   |   |-->| 5 |
   +---+---+---+---+---+---+---+---+---+---+   +---+
 5 | 0 | 0 |*1*|   |   |   |   |   |   |   |-->+ 2 +
   +---+---+---+---+---+---+---+---+---+---+   +---+
 6 | 0 | 0 | 0 | 0 |*1*|   |   |   |   |   |-->| 4 |
   +---+---+---+---+---+---+---+---+---+---+   +---+

The simplest way of doing this would be a linear search on each row.

-------------------------------------------------------------------
Complexity Analysis for Approach-1
-------------------------------------------------------------------
If you ran this code, you would have gotten the following error.

text You made too many calls to BinaryMatrix.get().

The maximum grid size is 100 by 100 , so it would contain 10000 cells. In the worst case, the linear search algorithm we implemented has to check every cell. With the problem description telling us that we can only make up to 1000 API calls, this clearly isn't going to work.

Let N be the number of rows, and M be the number of columns.

Time complexity : O(N * M)

We don't know the time complexity of binaryMatrix.get() as its implementation isn't our concern.
Therefore, we can assume it's O(1).

In the worst case, we are retrieving a value for each of the N * M cells.
At O(1) per operation, this gives a total of O(N * M).

Space complexity : O(1).

We are only using constant extra space.

=================================================================================================================================================================
 Approach-2 ( Binary Search Each Row )
=================================================================================================================================================================
Intuition
------------------------------------------------------------------
This isn't the best approach, but it passes, and coding it up is a good opportunity to practice binary search.
------------------------------------------------------------------
When linear search is too slow, we should try to find a way to use binary search.
If you're not familiar with binary search, [check out this Explore Card!](https://leetcode.com/explore/learn/card/binary-search/).
We recommend doing the first couple of binary search questions to get familiar with the algorithm before coming back to this problem.

Also, have a go at First Bad Version.
The only difference between that problem and this one is that instead of 0 and 1 , it uses false and true.

Like we did with the linear search, we're going to apply binary search independently on each row.
The target element we're searching for is the first 1 in the row .

The core part of a binary search algorithm is how it decides whether the target element 
is to the left or the right of the middle element.
Let's figure this out by thinking through a couple of examples.

In the row below, we've determined that the middle element is a 0.
On what side must the target element (first 1 ) be?
The left, or the right? Don't forget, all the 0's are before all the 1's.
   
                                            middle
											  |
											 \|/
    +----+----+----+----+----+----+----+----******----+----+----+----+----+----+----+----+
*** | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 * 59 * 60 | 61 | 62 | 63 | 64 | 65 | 66 | 67 | ***
    +----+----+----+----+----+----+----+----******----+----+----+----+----+----+----+----+
    +----+----********************+----+----******----+----+******************-+----+----+
*** |    |    *Is it to the left?*|    |    * 0  *    |    |*Or to the right?* |    |    | ***
    +----+----********************+----+----******----+----+******************-+----+----+

In this next row, the middle element is a 1?
What side must the target element be on? Could it also possibly be the 1 we just found?

                                            middle
											  |
											 \|/
    +----+----+----+----+----+----+----+----******----+----+----+----+----+----+----+----+
*** | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 * 36 * 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | ***
    +----+----+----+----+----+----+----+----******----+----+----+----+----+----+----+----+
    +----+----********************+----+----******----+----+******************-+----+----+
*** |    |    *Is it to the left?*|    |    * 1  *    |    |*Or to the right?* |    |    | ***
    +----+----********************+----+----******----+----+******************-+----+----+

For the first example, we can conclude that the target element ( if it exists ) must be to the 
**right** of the middle element.
This is because we know that everything to the left of a 0 must also be a 0.

For the second example, we can conclude that the target element is either the middle element 
itself or it is some other 1 to the **left** of the middle element.
We know that everything to the right of a 1 is also a 1,
but these can't possibly be further left than the one we just found.

In summary, if the middle element is a:

- **0** , then the target must be to the **right**.
- **1** , then the target is either this element or to the **left**.

We can then put this together into an algorithm that finds the index of the target element (first 1)
in each row, and then returns the minimum of those indexes.

-------------------------------------------------------------------
Algorithm
-------------------------------------------------------------------
If you're already quite familiar with binary search, feel free to skip down to the implementation below.
I've decided to include lots of details here, as binary search is one of those algorithms 
that a lot of people get frustrated with easily and find it difficult to master.

In a binary search, we always keep track of the range that the target might be in by using two variables: 
lo to represent the lowest possible index it could be, and hi to represent the highest possible index it could be.
Ignoring the binaryMatrix API details for the moment, here is a rough outline of our binary search in pseudocode.

define function binary_search(input_list):
  lo = the lowest possible index
  hi = the highest possible index
  while the search space contains 2 or more items:
    mid = the middle index in the remaining search space
	if the element at input_list[mid] is 0:
      lo = mid + 1 (the first 1 is *further right*, and can't be mid itself)
	else:
      hi = mid (the first 1 is either mid itself, *or is further left*)
  return the only index remaining in the search space

As always in binary search, there are a couple more key implementation details we still need to deal with:

 1. An even-length search space has two middles. Which do we choose?
 2. The row might be all 0's.

Let's tackle these issues one at a time.

The first issue, the choice of middle, is important, as otherwise, the search space might stop shrinking 
when it gets down to two elements. When the search space doesn't shrink, the algorithm does the exact 
same thing the next loop cycle, resulting in an infinite loop. Remember that when the search space 
only contains two elements, they are the ones pointed to by lo and hi. 
This means that the lower middle equals lo , and the upper-middle equals hi. 
We, therefore, need to think about which cases would shrink the search space, and which would not.

If we use the lower-middle
 - If it is a 0 , then we set lo = mid + 1. Because hi == mid + 1 , this means that lo == hi (search space is of length-one).
 - If it is a 1 , then we set hi = mid . Because mid == lo , this means that hi == lo (search space is of length-one).

If we use the upper-middle
 - If it is a 0 , then we set lo = mid + 1 . Because hi = mid , we now have hi > lo (search space is of length-zero).
 - If it is a 1 , then we set hi = mid . Because hi == mid was already true, the search space stays as is (of length-two).

If we use the lower-middle, we know the search space will always shrink.
If we use the upper-middle, it might not.
Therefore, we should go with the lower-middle.

The formula for this is mid = (low + high) / 2 .

The second issue, a row of all zeroes, is solved by recognizing that the algorithm always shrinks down the 
search space to a single element. This is supposed to be the first 1 , but if that doesn't exist, 
then it has to be a 0 . Therefore, we can detect this case by checking whether or not the last 
element in the search space is a 1 .

It is good practice to think these details through carefully so that you can write your 
binary search algorithm decisively and confidently. Resist the urge to Program by Permutation!

-------------------------------------------------------------------
Complexity Analysis for Approach-2
-------------------------------------------------------------------
Let N be the number of rows, and M be the number of columns.

Time complexity : O(N * logM).

There are M items in each row. Therefore, each binary search will have a cost of O(logM).
We are performing N of these binary searches, giving a time complexity of N * O(logM) = O(N * logM).

Space complexity : O(1).

We are using constant extra space.

=================================================================================================================================================================
 Approach-3 ( Start at Top Right, Move Only Left and Down ) 
=================================================================================================================================================================
Intuition
------------------------------------------------------------------
Did you notice in Approach 2 that we didn't need to finish searching all the rows?
One example of this was row 3 on the example in the animation.
At the point shown in the image below, it was clear that row 3 
could not possibly be better than the minimum we'd found so far.

     0   1   2   3   4   5   6   7   8   9
   +---+---+---+---+---+---+---+---+---+---+
 0 | 0 | 0 | 0 | 0 |+0+|+0+|*1*| 1 | 1 | 1 |
   +---+---+---+---+---+---+---+---+---+---+
 1 | 0 | 0 | 0 | 0 |+0+|+0+|+0+|*1*| 1 | 1 |
   +---+---+---+---+---+---+---+---+---+---+
 2 | 0 | 0 | 0 | 0 |+0+|*1*| 1 |+1+| 1 | 1 |
   +---+---+---+---+---+---+---+---+---+---+
 3 | 0 | 0 | 0 | 0 |+0+| 0 | 0 |*0*|   |   |
   +---+---+---+---+---+---+---+---+---+---+
 4 |   |   |   |   |   |   |   |   |   |   |
   +---+---+---+---+---+---+---+---+---+---+
 5 |   |   |   |   |   |   |   |   |   |   |
   +---+---+---+---+---+---+---+---+---+---+
 6 |   |   |   |   |   |   |   |   |   |   |
   +---+---+---+---+---+---+---+---+---+---+

API Calls: 12

Therefore, an optimization we could have made was to keep track of the minimum index so far,
and then abort the search on any rows where we have discovered a 0 at, or to the right of,
that minimum index.

We can do even better than that; on each search, we can set hi = smallest_index - 1, 
where smallest_index is the smallest index of a 1 we've seen so far.
In most cases, this is a substantial improvement.
It works because we're only interested in finding 1 s at lower indexes than we previously found.

Here is what the worst-case looks like. Like before, its time complexity is still O(N * log(M)).

     0   1   2   3   4   5   6   7   8   9
   +---+---+---+---+---+---+---+---+---+---+
 0 |***|***|***|***| 0 |***|***| 0 | 0 | 1 |
   +---+---+---+---+---+---+---+---+---+---+
 1 |***|***|***|***| 0 |***| 0 | 0 | 1 |***|
   +---+---+---+---+---+---+---+---+---+---+
 2 |***|***|***| 0 |***| 0 | 0 | 1 |***|***|
   +---+---+---+---+---+---+---+---+---+---+
 3 |***|***|***| 0 |***| 0 | 1 |***|***|***|
   +---+---+---+---+---+---+---+---+---+---+
 4 |***|***| 0 |***| 0 | 1 |***|***|***|***|
   +---+---+---+---+---+---+---+---+---+---+
 5 |***|***| 0 | 0 | 1 |***|***|***|***|***|
   +---+---+---+---+---+---+---+---+---+---+
 6 |***| 0 | 0 | 1 |***|***|***|***|***|***|
   +---+---+---+---+---+---+---+---+---+---+

While this is no worse than Approach 2, there is a better algorithm.

-------------------------------------------------------------------
Algorithm
-------------------------------------------------------------------

Start in the top right corner, and if the current value is a 0 , move down. If it is a 1 , then move left.

- When we encounter a 0, we know that the leftmost 1 can't be to the left of it.
- When we encounter a 1, we should continue the search on that row (move pointer to the left), in order to find an even smaller index.
------------------------------------------------------------------

-------------------------------------------------------------------
Complexity Analysis for Approach-3
-------------------------------------------------------------------
Let N be the number of rows, and M be the number of columns.

Time complexity : O(N + M).

At each step, we're moving 1 step left or 1 step down.
Therefore, we'll always finish looking at either one of the M rows or N columns.
Therefore, we'll stay in the grid for at most N+M steps, and therefore get a time complexity of O(N+M).

Space complexity : O(1).

We are using constant extra space.


```
##### Complexity Analysis:
```
N = # of rows 
M = # of columns
=================================================================================================================================================================
 Approach 1: Linear Search Each Row
=================================================================================================================================================================
TC  : O(N * M)
SC  : O(1)

=================================================================================================================================================================
 Approach-2 ( Binary Search Each Row )
=================================================================================================================================================================
TC  : O(N * log(M))
SC  : O(1)

=================================================================================================================================================================
 Approach-3 ( Start at Top Right, Move Only Left and Down ) 
=================================================================================================================================================================
TC  : O(N + M)
SC  : O(1)
```
```python
# """
# This is BinaryMatrix's API interface.
# You should not implement it, or speculate about its implementation
# """
#class BinaryMatrix(object):
#    def get(self, row: int, col: int) -> int:
#    def dimensions(self) -> list[]:
class BinaryMatrix(object):
    def get(self, row, col):
        pass

    def dimensions(self):
        pass

#=================================================================================================================================================================
# Approach 1: Linear Search Each Row
#=================================================================================================================================================================
#N = # of rows 
#M = # of columns
#
# TC: O(N *  M)
# SC: O(1)
# NOTE: This approach won't pass ( so don't use it in an interview ).
class Solution_1:
    def leftMostColumnWithOne(self, binaryMatrix: 'BinaryMatrix') -> int:
        """
        :type binaryMatrix: BinaryMatrix
        :rtype: int
        """
        rows, cols = binaryMatrix.dimensions()
        smallest_index = cols
        # Go through each of the rows.
        for row in range(rows):
            # Linear seach for the first 1 in this row.
            for col in range(cols):
                if binaryMatrix.get(row, col) == 1:
                    smallest_index = min(smallest_index, col)
                    break
        # If we found an index, we should return it. Otherwise, return -1.
        return -1 if smallest_index == cols else smallest_index

#=================================================================================================================================================================
# Approach-2 ( Binary Search Each Row )
#=================================================================================================================================================================
#N = # of rows 
#M = # of columns
#
#TC  : O(N * log(M))
#SC  : O(1)
#
class Solution_2:
    def leftMostColumnWithOne(self, binaryMatrix: 'BinaryMatrix') -> int:
        """
        :type binaryMatrix: BinaryMatrix
        :rtype: int
        """
        rows, cols = binaryMatrix.dimensions()
        smallest_index = cols
        for row in range(rows):
            # Binary Search for the first 1 in the row.
            lo = 0
            hi = cols - 1
            while lo < hi:
                mid = (lo + hi) // 2
                if binaryMatrix.get(row, mid) == 0:
                    lo = mid + 1
                else:
                    hi = mid
            # If the last element in the search space is a 1, then this row
            # contained a 1.
            if binaryMatrix.get(row, lo) == 1:
                smallest_index = min(smallest_index, lo)
        # If smallest_index is still set to cols, then there were no 1's in 
        # the grid. 
        return -1 if smallest_index == cols else smallest_index

#=================================================================================================================================================================
# Approach-3 ( Start at Top Right, Move Only Left and Down ) 
#=================================================================================================================================================================
#N = # of rows 
#M = # of columns
#
#TC  : O(N + M)
#SC  : O(1)
#
#NOTE : This is a solution you should go for in an interview situation.
#
class Solution_3:		
    def leftMostColumnWithOne(self, binaryMatrix: 'BinaryMatrix') -> int:
        """
        :type binaryMatrix: BinaryMatrix
        :rtype: int
        """
        rows, cols = binaryMatrix.dimensions()
        
        # Set pointers to the top-right corner.
        current_row = 0
        current_col = cols - 1
        
        # Repeat the search until it goes off the grid.
        while current_row < rows and current_col >= 0:
            if binaryMatrix.get(current_row, current_col) == 0:
                current_row += 1
            else:
                current_col -= 1
        
        # If we never left the last column, it must have been all 0's.
        return current_col + 1 if current_col != cols - 1 else -1
```

<br/>
<div align="right">
    <b><a href="#algorithms">⬆️ Back to Top</a></b>
</div>
<br/>

####  [Leftmost Column Index of 1]
##### Solution Explanation:
[Refer to LC-1428:Leftmost Column with at Least a One - Complexity Analysis](https://github.com/sm2774us/facebook_interview_prep_2021#solution-explanation-1)
##### Complexity Analysis:
[Refer to LC-1428:Leftmost Column with at Least a One - Complexity Analysis](https://github.com/sm2774us/facebook_interview_prep_2021#complexity-analysis-1)
```python
#=================================================================================================================================================================
# Approach-1 ( Binary Search Each Row )
#=================================================================================================================================================================
#
#N = # of rows 
#M = # of columns
#
#TC  : O(N * log(M))
#SC  : O(1)
#
from typing import List

def leftMostColumnWithOne_using_binary_search(matrix: List[List[int]]) -> int:
    lengthOfRow = len(matrix[0])
    leftMost = float('inf')
    for row in matrix:
        # Start binary search per row
        start = 0
        end = lengthOfRow - 1
        while start < end:
            median = (start + end) // 2
            if row[median] == 1:
                end = median - 1
            else:
                start = median + 1
        if start + 1 < lengthOfRow and row[start] == 0 and row[start + 1] == 1:
            start += 1
        if row[start] == 1:
            leftMost = min(leftMost, start)
    return leftMost if leftMost != float('inf') else -1

if __name__ == "__main__":
    print(leftMostColumnWithOne_using_binary_search(
    [
        [0,0,0,0],
        [0,0,1,1],
        [0,0,1,1],
        [0,1,1,1]
    ]
    ))

    print(leftMostColumnWithOne_using_binary_search(
    [
        [0,0,0,0],
        [0,0,0,0],
        [0,0,0,0],
        [0,0,0,1]
    ]
    ))

    print(leftMostColumnWithOne_using_binary_search(
    [
        [0,0,0,0],
        [0,0,0,0],
        [0,0,0,0],
        [0,0,0,0]
    ]
    ))

    print(leftMostColumnWithOne_using_binary_search(
    [
        [0,1,1,1],
        [0,0,1,1],
        [0,0,1,1],
        [0,0,0,0]
    ]
    ))
    #Output:
    #1
    #3
    #-1
    #1

#=================================================================================================================================================================
# Approach-2 ( Start at Top Right, Move Only Left and Down ) 
#=================================================================================================================================================================
#N = # of rows 
#M = # of columns
#
#TC  : O(N + M)
#SC  : O(1)
#
#NOTE : This is a solution you should go for in an interview situation.
#
from typing import List

def leftMostColumnWithOne_using_optimal_approach(matrix: List[List[int]]) -> int:
    if matrix is None or len(matrix) == 0 or matrix[0] is None or len(matrix[0]) == 0: return -1
    rows, cols = len(matrix), len(matrix[0])

    # Set pointers to the top-right corner.
    current_row = 0
    current_col = cols - 1
        
    # Repeat the search until it goes off the grid.
    while current_row < rows and current_col >= 0:
        if matrix[current_row][current_col] == 0:
            current_row += 1
        else:
            current_col -= 1
        
    # If we never left the last column, it must have been all 0's.
    return current_col + 1 if current_col != cols - 1 else -1
	
if __name__ == "__main__":
    print(leftMostColumnWithOne_using_optimal_approach(
    [
        [0,0,0,0],
        [0,0,1,1],
        [0,0,1,1],
        [0,1,1,1]
    ]
    ))

    print(leftMostColumnWithOne_using_optimal_approach(
    [
        [0,0,0,0],
        [0,0,0,0],
        [0,0,0,0],
        [0,0,0,1]
    ]
    ))

    print(leftMostColumnWithOne_using_optimal_approach(
    [
        [0,0,0,0],
        [0,0,0,0],
        [0,0,0,0],
        [0,0,0,0]
    ]
    ))

    print(leftMostColumnWithOne_using_optimal_approach(
    [
        [0,1,1,1],
        [0,0,1,1],
        [0,0,1,1],
        [0,0,0,0]
    ]
    ))
    #Output:
    #1
    #3
    #-1
    #1	

```

<br/>
<div align="right">
    <b><a href="#algorithms">⬆️ Back to Top</a></b>
</div>
<br/>

####  [LC-240:Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii)
##### Solution Explanation:
```
```
##### Complexity Analysis:
```
```
```python
```

<br/>
<div align="right">
    <b><a href="#algorithms">⬆️ Back to Top</a></b>
</div>
<br/>

https://leetcode.com/problems/employee-free-time/

https://leetcode.com/discuss/interview-question/341247/facebook-leftmost-column-index-of-1

https://www.*.org/lowest-common-ancestor-in-a-binary-tree-set-2-using-parent-pointer/
https://leetcode.com/problems/subarray-sum-equals-k/
https://leetcode.com/problems/copy-list-with-random-pointer/
https://leetcode.com/problems/serialize-and-deserialize-binary-tree/
https://leetcode.com/problems/word-break-ii/
Validate Single Binary Tree
https://leetcode.com/discuss/interview-question/347374/
Task Scheduler
https://leetcode.com/discuss/interview-question/673575/Facebook-or-Phone-or-Task-Scheduler
https://leetcode.com/problems/task-scheduler/
https://leetcode.com/problems/target-sum/
https://leetcode.com/problems/generate-parentheses/
https://leetcode.com/problems/nth-digit/
https://leetcode.com/problems/insert-delete-getrandom-o1/
https://leetcode.com/problems/insert-delete-getrandom-o1-duplicates-allowed/
https://leetcode.com/problems/accounts-merge/
https://leetcode.com/problems/valid-word-abbreviation/
https://leetcode.com/problems/candy-crush/
https://leetcode.com/problems/koko-eating-bananas/
https://leetcode.com/problems/binary-tree-right-side-view/
https://leetcode.com/problems/restore-ip-addresses/
https://leetcode.com/problems/powx-n/
https://leetcode.com/problems/russian-doll-envelopes/
https://leetcode.com/problems/walls-and-gates/
https://leetcode.com/problems/best-time-to-buy-and-sell-stock/
https://leetcode.com/problems/find-largest-value-in-each-tree-row/
https://leetcode.com/problems/add-strings/
https://leetcode.com/problems/combination-sum/
https://leetcode.com/problems/maximum-swap/
Dot Product of Sparse vectors
https://leetcode.com/discuss/interview-question/124823/
Find an efficient way to represent a vector (1,1,1,1,1,1,22,2,2,2,2,2,2,2,3,4,4,5,6,6,7,7,7,8,8,8,9,9,9,99,9,,1,1,1,1,1,1,2,3,34,3,4,,3,3,3,3....)
Use the representation you come up with to compute dot product of two vectors
Ex: If you come up with MyDataStructure to represent a vector, then your function signature will be
int dotProduct(MyDataStructure vector1, MyDataStructure vector2)
// dot product of two vectors [1,2,3,4] and [5,6,7,8] is 1 * 5 + 2 * 6 + 3 * 7 + 4 * 8
Take advantage of your "efficient" representation to compute the dot product faster.
New question added for dot product of sparse vectors,
https://leetcode.com/problems/dot-product-of-two-sparse-vectors/

https://leetcode.com/problems/random-pick-with-weight/

Some questions are the closest that it can get to the actual question. Like Russian Doll envelopes or Task Scheduler.

https://leetcode.com/problems/find-all-anagrams-in-a-string/
A string / array problem involving distinct characters and window
https://leetcode.com/problems/shortest-bridge/
https://leetcode.com/problems/partition-equal-subset-sum/
https://leetcode.com/problems/valid-palindrome-ii/
https://leetcode.com/problems/kth-smallest-element-in-a-bst/
You are given a mn grid. You are asked to generate k mines on this grid randomly. Each cell should have equal probability of k / mn of being chosen. Your algorithm should run in O(m) time.
https://leetcode.com/problems/continuous-subarray-sum/
(Given a list of positive numbers and a target integer k, write a function to check if the array has a continuous subarray which sums to k.)
https://leetcode.com/problems/verifying-an-alien-dictionary/
https://leetcode.com/problems/alien-dictionary/
https://leetcode.com/problems/course-schedule/
https://leetcode.com/problems/interval-list-intersections/
https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/
https://leetcode.com/problems/plus-one/
https://www.***.org/find-index-maximum-occurring-element-equal-probability/***
https://leetcode.com/problems/range-sum-of-bst/
Similar strings ("face", "eacf") returns true if only 2 positions in the strings are swapped. Here 'f' and 'e' are swapped in the example.
https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph
https://leetcode.com/problems/add-binary/
Given two binary search trees how do we merge everything so it prints inorder. The answer I gave was to run inorder on both trees and use "merge" from merge-sort.
https://leetcode.com/problems/valid-palindrome
https://leetcode.com/problems/add-strings
https://leetcode.com/problems/serialize-and-deserialize-binary-tree/
https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree
https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/
https://leetcode.com/problems/binary-tree-paths
https://leetcode.com/problems/minimum-window-substring
How to remove duplicates from a list
https://leetcode.com/problems/maximum-subarray
https://leetcode.com/problems/valid-parentheses
https://leetcode.com/problems/merge-intervals
https://leetcode.com/problems/task-scheduler/
https://leetcode.com/problems/clone-graph/

| [34] - Facebook - Onsite - Generate Random Max Index | <p>Given an array of integers `arr`, randomly return an index of the maximum value seen by far.</p> | [Facebook - Onsite - Generate random max index](https://leetcode.com/discuss/interview-question/451431/facebook-onsite-generate-random-max-index) | [Python](Arrays/034_facebook_onsite_Generate_Random_Max_Index/Solution.py) |
| [20] - Facebook - Interview Question - Minimum number of people to spread a message | <p>You need to find the minimum number of people to reach out so that your promotion message is spread out across entire network in twitter.</p> | [Facebook - Interview Question - Minimum number of people to spread a message](https://leetcode.com/discuss/interview-question/124827/Find-minimum-number-of-people-to-reach-to-spread-a-message-across-all-people-in-twitter/) | [Python](Graph/020_facebook_interview_question_Minimum_Number_Of_People_To_Spread_A_Message/Solution.py) |
| [21] - Facebook - Phone Interview Question - Given a directed graph remove return minimum of edges to keep all paths | <p>Given a directed graph remove return minimum of edges to keep all paths.</p> | [Facebook - Phone Interview Question - Given a directed graph remove return minimum of edges to keep all paths](https://leetcode.com/discuss/interview-question/630806/facebook-phone-transitive-reduction-factorial-trailing-zeroes) | [Python](Graph/021_facebook_phone_interview_question_Transitive_Reduction/Solution.py) |
| [27] - Facebook - Phone screen - Shortest Path with Obstacles | <p>Given a 2D matrix where some of the elements are filled with 1<br>and the rest of the elements are filled with X, except 2 elements, of which one is S (start point) and E (endpoint).<br>Here X means you cannot traverse to that particular point.<br>From a cell you can either traverse to left, right, up or down.<br>Given two points in the matrix find the shortest path between these points.</p> | [Facebook - Phone screen - Shortest Path with Obstacles](https://leetcode.com/discuss/interview-question/301192/Facebook-phone-screen-Shortest-Path-with-Obstacles/283312) | [Python](Graph/027_facebook_PhoneScreen_Shortest_Path_With_Obstacles/Solution.py) |

## 2. Coding Round 1:
https://leetcode.com/problems/insert-interval/
https://leetcode.com/problems/convert-a-number-to-hexadecimal/
https://leetcode.com/problems/rotate-array/
https://leetcode.com/problems/k-closest-points-to-origin/
https://leetcode.com/discuss/interview-question/124759/
https://leetcode.com/problems/product-of-array-except-self
https://leetcode.com/problems/find-all-anagrams-in-a-string/
https://leetcode.com/problems/minimum-window-substring/
https://leetcode.com/problems/closest-binary-search-tree-value/
https://leetcode.com/problems/insert-delete-getrandom-o1/
https://leetcode.com/problems/fraction-to-recurring-decimal/
https://leetcode.com/problems/powx-n
https://leetcode.com/problems/subarray-sum-equals-k
https://leetcode.com/problems/best-time-to-buy-and-sell-stock
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv
https://leetcode.com/problems/add-and-search-word-data-structure-design
https://leetcode.com/problems/sudoku-solver/
https://leetcode.com/discuss/interview-question/338948/Facebook-or-Onsite-or-Schedule-of-Tasks
https://leetcode.com/problems/binary-tree-maximum-path-sum
https://leetcode.com/problems/maximum-subarray
https://leetcode.com/problems/move-zeroes
https://leetcode.com/problems/valid-number
https://leetcode.com/problems/first-bad-version/

## 3. Coding Round 2:
https://leetcode.com/problems/valid-number/
You have an API to check if is it possible to move left, right, up, down and one more method to check if current position is the last one. Find the shortest way to the last position. You don't have any data structure - only API.
https://leetcode.com/problems/serialize-and-deserialize-binary-tree/
https://leetcode.com/problems/group-shifted-strings/
https://leetcode.com/problems/task-scheduler/
Calculate tax if Salary and Tax Brackets are given as list in the form
[ [10000, 0.3],[20000, 0.2], [30000, 0.1], [null, .1]]
null being rest of the salary
Is there a way to reach (0,0) from a mXn matrix to (m-1,n-1) position and give the path.
https://leetcode.com/problems/simplify-path/
n-ary Tree with each node having a boolean flag. Traverse all the nodes with only boolean flag = True. Return the total distance traveled from root to all those nodes.
https://leetcode.com/problems/product-of-array-except-self/
https://leetcode.com/discuss/interview-question/432086/Facebook-or-Phone-Screen-or-Task-Scheduler/394783
https://leetcode.com/problems/find-all-anagrams-in-a-string
https://leetcode.com/problems/is-graph-bipartite
https://leetcode.com/problems/merge-sorted-array
https://leetcode.com/problems/maximum-subarray
https://leetcode.com/problems/serialize-and-deserialize-binary-tree
https://leetcode.com/problems/remove-invalid-parentheses/
https://leetcode.com/problems/subarray-sum-equals-k/
https://leetcode.com/problems/binary-tree-level-order-traversal/
https://leetcode.com/problems/longest-increasing-path-in-a-matrix/
https://leetcode.com/problems/custom-sort-string
https://leetcode.com/problems/read-n-characters-given-read4
https://leetcode.com/problems/remove-invalid-parentheses
https://leetcode.com/problems/palindrome-permutation
https://leetcode.com/problems/max-consecutive-ones-iii
https://leetcode.com/problems/range-sum-of-bst
https://leetcode.com/problems/exclusive-time-of-functions
https://leetcode.com/problems/search-in-rotated-sorted-array/
https://leetcode.com/problems/search-in-rotated-sorted-array-ii/

## 4. Design:

Design Google search
Some question related to caching and balancing. Not exactly the "design twitter" type of question, but expect to talk about different components, latency, throughput, consistency and availability.
A remote server is not responding. Debug the issue. Needed to cover entire TCP/IP stack(fragmentation/icmp/etc) + machine metrics (vmstat,iostat,strace etc). Describe virtual memory in terms of demand paging.
2 machines are connected, suddenly 1 machine is responding slowly. Why ?
We had a good discussion in which we discussed everything under the sun, from NFS being bad to Networking being wrong to Kernel running out of resources(buffer-cache/inodes/virtual memory). Interviewer was interested to know the commands that i would use (strace, lsof, readlink, cat /proc/pid etc).
Copy some resource from N sources to M sinks. where N could be < 10 and M could be 10k/Millions etc.
Design File Storage System. Like Dropbox, Google Drive
Not any fancy one like design Twitter or Uber. More on scheduling service side and i designed using SQL appraoch. Discussed concuurency issues, Table schemas, composite keys etc.
Design recommendation of celebrities to user on Instagram
Design search for Twitter
Design a Content publishing site with privacy restrictions.
System Design of Uber. He liked my design. He was really nice guy, i felt he was interested in my success.
Design a type ahead features for a website. We discussed various data structures, advantage /disadvantages. Lot of different cases, scenario to handle etc.
Design instagram client side.
Design a leetcode contest, leadership board system
Design Instagram
Design keyword search in FB Posts
There are music providers like spotify, apple music etc. Design a service for these providers to display top 10 songs played by each user. Was aked to write ER tables and API's.
Design a system like Hacker Rank for a programming contest and their ranking.

https://leetcode.com/discuss/interview-question/1002218/Facebook-or-Google-or-Top-System-Design-Interview-Questions-(Part-1)
https://leetcode.com/discuss/interview-question/1042229/Facebook-or-Google-or-Top-System-Design-Interview-Questions-(Part-2)
https://leetcode.com/discuss/interview-question/719253/Design-Facebook-%3A-System-Design-Interview
https://leetcode.com/discuss/interview-question/124657/Facebook-or-System-Design-or-A-web-crawler-that-will-crawl-Wikipedia
Found this above list in this LeetCode Post - https://leetcode.com/discuss/interview-question/1140451/Helpful-list-of-LeetCode-Posts-on-System-Design-at-Facebook-Google-Amazon-Uber-Microsoft

## 5. Behavioral:

Work experience, past projects, standard "tell me about a time" questions, hypothetical scenario questions
Usual stuff around things that I am proud of/ projects that I regret etc
Tell me about your current role
Tell me about a projects you are proud of
Tell Me About A Time When You Had To Give Someone Difficult Feedback. How Did You Handle It?(What kind of feedback you give ?)
Tell me about a time when you had a conflict with a manage and how you resolved it
What's the most difficult/challenging problem you have had to solve?
Which environment is best to you to work ?
Tell about best decision in your life from childhood ? Decision that changed your life
On which topics you want improve? What are doing to impoving on that topics ? Did you try build project on that topics ?