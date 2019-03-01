# LeetCode - CheatSheet

A compilation of notes that I made when working with Leetcode problems - Note a majority of the content and code was taken off of 
the solution and discuss sections of the Leetcode website so I do not take any owenership of the below. The following is simply 
notes compiled for learning purposes.


## Getting Started

Currently LeetCode-CheatSheet contains various text files for notes but is primarily composed of this README file which supports 
the markdown formatting language. This makes the notes and code look a lot better and supports many awesome features like quick links 
and makes formatting much easier. 


### Prerequisites

In the future I might add a html document which also performs some formatting and if that comes out, ideally the html and css
would be written for web browsers on large screens which support the newest version of bootstrap and html 5.
I use the following application when building and testing html.

```
Chrome Version 71.0.3578.98
```


## Built With

* [Markdown](https://guides.github.com/features/mastering-markdown/) - Syntax styling



## Authors

* **Lichen Ma** - *compiling information* 



## Acknowledgments

* **Leetcode** - *content belongs to respective creators*


## Quick Access Links 
1. [Two Sum](#twoSum)
    1. [Brute force](#twoSumBruteForce)
    2. [One pass Hash Table](#twoSumOnePassHashTable) 
2. [Add Two Numbers](#addTwoNumbers)
    1. [Elementary Math Solution](#addTwoNumbersElementaryMath)
3. [Substring No Repeat](#substringNoRepeat)
    1. [Brute force](#substringNoRepeatBruteForce)
    2. [Sliding Window](#substringNoRepeatSlidingWindow)
    3. [Optimized Sliding Window](#substringNoRepeatOptimized)
4. [Median of Two Sorted Arrays](#medianofTwoSortedArrays)
    1. [Recursive Approach](#medianofTwoSortedArraysRecursiveApproach)
5. [Longest Palindromic Substring](#longestPalindromicSubstring)
    1. [Longest Common Substring](#longestPalindromicSubstringLongestCommonSubstring)
    2. [Brute Force](#longestPalindromicSubstringBruteForce)
    3. [Dynamic Programming](#longestPalindromicSubstringDynamicProgramming)
    4. [Expand Around Center](#longestPalindromicSubstringExpandAroundCenter)
    5. [Manacher's Algorithm](#longestPalindromicSubstringManacherAlgorithm)
6. [ZigZag Conversion](#zigZagConversion)
    1. [Sort by Row](#zigZagConversionSortbyRow)
    2. [Visit by Row](#zigZagConversionVisitbyRow)
7. [Reverse Integer](#reverseInteger)
    1. [Pop and Push Digits and Check Before Overflow](#reverseIntegerPopandPush)
8. [String to Integer](#stringtoInteger)
    1. [ASCII Conversion](#stringtoIntegerASCII)
9. [Palindrome Number](#palindromeNumber)
    1. [Revert Half of the Number](#palindromeNumberRevertHalf)
10. [Regular Expression Matching](#regularExpressionMatching)
    1. [Recursion](#regularExpressionMatchingRecursion)
    2. [Dynamic Programming](#regularExpressionMatchingDynamicProgramming)
    3. [Non - Recursive](#regularExpressionMatchingNonRecursive)
11. [Container With the Most Water](#containerwiththeMostWater)
    1. [Brute Force](#containerwiththeMostWaterBruteForce)
    2. [Two Pointer Approach](#containerwiththeMostWaterTwoPointer)
12. [Integer To Roman](#integertoRoman)
    1. [String Array](#integertoRomanStringArray)

<br><br><br>
***
<a name="twoSum"></a>
# 1-Two Sum 

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.
You may assume that each input would have **exactly one solution**, and you may not use the same element twice.

Example:
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```
<a name="twoSumBruteForce"></a>
## Brute Force 

```java 
public int[] twoSum(int[] nums, int target) {
	for (int i=0; i<nums.size; i++){
		for (int j=i+1;j<nums.length;j++){
			if (nums[j]==target-nums[i]){
				return new int[] {i,j};
			}
		}
	}
	throw new IllegalArgumentException("No two sum solution");
} 
```
**Complexity Analysis** 
```
* Time complexity:   O(n^2)       we have a nested loop 
* Space complexity:  O(1) 	  we do not allocate any additional memory
```
<a name="twoSumOnePassHashTable"></a>
## One Pass Hash Table 

```java 
public int[] twoSum(int[] nums, int target) {
	Map<Integer, Integer> map = new HashMap<>();
	for(int i=0; i<nums.length; i++){
		int complement=target-nums[i];
		if (map.containsKey(complement)){
			return new int[] {map.get(complement),i};
		}
		map.put(nums[i],i);
	}
	throw new IllegalArgumentException("No two sum solution"); 
}
```
**Complexity Analysis**
```
* Time complexity:   O(n)		each lookup in the hash table only requires O(1) time
* Space complexity:  O(n)		we require additional space for the hash table which stores at most n
```

<br><br><br>
***
<a name="addTwoNumbers"></a>
# 2-Add Two Numbers 

Given two non-empty linked lists representing two non-negative integers with the digits stored in 
reverse order and each node containing a single digit, add the two numbers and return as a linked list

Example: 
```
Input (2 -> 4 -> 3) + (5 -> 6 -> 4) 
Output 7 -> 0 -> 8 

342 + 465 = 807
```
<a name="addTwoNumbersElementaryMath"></a>
## Elementary Math Solution

```java 
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */



class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummyHead= new ListNode(0); 
        ListNode p=l1, q=l2, curr=dummyHead; 
        int carry=0; 
        while (p!=null||q!=null){
            int x= (p!=null) ? p.val :0; //if (p!=null) then x contains p.val
            int y= (q!=null) ? q.val :0;
            int sum=carry+x+y;
            carry=sum/10;
            curr.next=new ListNode(sum%10);
            curr=curr.next; 
            if (p!=null) p=p.next; 
            if (q!=null) q=q.next; 
        }
        if (carry>0){
            curr.next= new ListNode(carry);
        }
        return dummyHead.next; 
    }
}
```

**Complexity analysis** 
```
* Time Complexity:  O(max(m,n))         depends on the lengths of the two linked lists 
* Space Complexity: O(max(m,n))		the maximum length of the new list is max(m,n)+1
```
<br><br><br>
***
<a name="substringNoRepeat"></a>
# 3-Substring No Repeat 

Longest Substring Without Repeating Characters 

Given a string find the length of the longest substring without repeating characters. 

```
Example
Input: 		"abcabcbb"
Output:		3
Explanation:	The answer is "abc", with the length of 3
```
```
Example 2
Input:		"bbbbb"
Output:		1
Explanation:	The answer is "b", with the length of 1
```
```
Example 3
Input:		"pwwkew"
Output:		3
Explanation: 	The answer is "wke", with the length of 3. Note that the answer must be a substring
		"pwke" is a subsequence and not a substring 
```

<a name="substringNoRepeatBruteForce"></a>
## Brute Force 

*Algorithm* 


Suppose we have a function "boolean allUnique(String substring)" which returns true if all the
characters in the substring are unique and false otherwise. We can iterate through all the possible 
substrings of the given string s and call the function allUnique. If it turns out to be true, then we 
update our answer of the maximum length of substring without duplicate characters. 

To enumerate all substrings of a given string we enumerate the start and end indices of them. Suppose
the start and end indices are i and j respectively. Then we have 0 <= i <= j <= n. Thus using two 
nested loops with i from 0 to n-1 and j from i+1 to n, we can enumerate all the substrings of s

To check if one string has duplicate characters we can use a set. We iterate through all the 
characters in the string and put them into the set one by one. Before putting one character, we check
if the set already contains it. If so we return false and after the loop we return true.


```java 
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int ans = 0;
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j <= n; j++)
                if (allUnique(s, i, j)) ans = Math.max(ans, j - i);
        return ans;
    }

    public boolean allUnique(String s, int start, int end) {
        Set<Character> set = new HashSet<>();
        for (int i = start; i < end; i++) {
            Character ch = s.charAt(i);
            if (set.contains(ch)) return false;
            set.add(ch);
        }
        return true;
    }
}
```

**Complexity Analysis** 

```
* Time Complexity:   O(n^3)		Verifying if characters in   [i,j) are unique requires us to scan all of
					them which would cost O(j-i) time. 

					For a given i, the sum of time costed by each j -> [i+1,n] is 
					"Summation from i+1 to n O(j-1)"

					Thus, the sum of all the time consumption is: 
					O(summation from 0 to n-1(summation from j=i+1 to n (j-1))) 
					O(summation from i=0 to n-1(1+n-i)(n-i)/2)) = O(n^3)


					*Note that the sum of all numbers up to n 1+2+3+...+n = n(n+1)/2


* Space Complexity:  O(min(n,m))	We require O(k) space for checking a substring has no duplicate 
					characters, where k is the size of the set. The size of the Set is 
					upper bounded by the size of the string n amd the size of the charset
					or alphabet m 
				
				
```
<a name="substringNoRepeatSlidingWindow"></a>
## Sliding Window 

A sliding window is an abstract concept commonly used in array/string problems. A window is a range of 
elements in the array/string which usually defined by the start and end indices
```
Ex. [i,j) left-closed, right-open
```
A sliding window is a window that slides its two boundaries in a certain direction, for example if we
slide \[i,j) to the right by 1 element, then it becomes \[i+1, j+1) - left closed, right open.



Sliding Window approach, whenever we are looking at a section on an array usual to perform calculations
we don't need to completely recalculate everything for every section of the array. Usually we can use
the value obtained from another section of the array to determine something about this section of the 
array. For example if we are calculating the sum of sections of an array we can use the previously 
calculated value of a section to determine the sum of an adjacent section in the array. 
```
Ex. 1 2 3 4 5 6 7 8 
```
If we calculate the first section of four values we get 1+2+3+4 = 10 , then to calculate the next section
2+3+4+5 we can just take our first section (window_sum) and perform the operation: 
```
window_sum-first entry + last entry = 10-1+5= 14
```
So essentially for the window sliding technique we use what we know about an existing window to 
determine properties for another window. 

<br><br>
*Algorithm*

In the brute force approach, we repeatedly check a substring to see if it has duplicate characters but
this is unnecessary. If a substring from index i to j-1 is already checked to have no duplicate 
characters we only need to check if s[j] is already in the substring. 

To check if a character is already in the substring we can scan the substring which leads to an O(n^2)
algorithm but we can improve on this runtime using a HashSet as a sliding window to check if a 
character exists in the current set O(1). 

We use a HashSet to store the characters in the current window \[i,j) and then we slide the index j to
the right, if it is not in the HashSet, we slide j further until s[j] is already in the HashSet. At
this point we found the maximum size of substrings without duplicate characters starting with index i.
If we do this for all i, then we obtain our answer. 


```java 
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        Set<Character> set = new HashSet<>();
        int ans = 0, i = 0, j = 0;
        while (i < n && j < n) {
            // try to extend the range [i, j]
            if (!set.contains(s.charAt(j))){
                set.add(s.charAt(j++));
                ans = Math.max(ans, j - i);
            }
            else {
                set.remove(s.charAt(i++));
            }
        }
        return ans;
    }
}
```

**Complexity Analysis**
```
Time complexity:	O(2n)=O(n)	Worst case each character will be visited twice by i and j

Space complexity: 	O(min(m,n))	Same as the brute force method, we need O(k) space for the 
					sliding window where k is the size of the set. The size of the
					set is bounded by the size of the string n and the size of the
					charset/alphabet m
```


<a name="substringNoRepeatOptimized"></a>

## Sliding Window Optimized 

The previously discussed sliding window approach requires at most 2n steps and this could in fact be
optimized even further to require only n steps. Instead of using a set to tell if a character exists or
not, we could define a mapping of the characters to its index. Then we can skip the characters 
immediately when we found a repeated character 

If s[j] has a duplicate in the range \[i , j) with index j', we don't need to increase i little be little
we can just skip all the elements in the range \[i , j'] and let i be j'+1 directly 

```java 
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), j + 1);
        }
        return ans;
    }
}
```

<br><br><br>
***
<a name="medianofTwoSortedArrays"></a>
# 4-Median of Two Sorted Arrays 

There are two sorted arrays num1 and num2 of size m and n respectively. Find the median of the two 
sorted arrays. The overall run time complexity should be O(log (m+n)). You may assume nums1 and nums2
cannot be both empty. 

```
Example 

nums1 = [1, 3] 
nums2 = [2]

The median is 2.0
```

```
Example 2

nums1= [1, 2] 
nums2= [3, 4] 

The median is (2+3)/2 = 2.5
```

<a name="medianofTwoSortedArraysRecursiveApproach"></a>
## Recursive Approach

In statistics the median is used for dividing a set into two equal length subsets with one set being
always greater than the other set. To approach this problem first we cut A into two parts at a random
position i: 

```
         left_A                |           right_A 

  A[0], A[1], ... , A[i-1]         A[i], A[i+1], ... , A[m-1]
```

Since A has m elements, there are m+1 kinds of cutting as i can range from 0-m. We can also see that
left_A is empty when i is zero and right_A is empty when i=m

```
len(left_A) = i and len(right_A)= m-i
```

We can similarly cut B into two parts at a random position j: 

```
	left_B			|	right_B

  B[0], B[1], ... , B[j-1]	   B[j], B[j+1], ... , B[n-1]
```
  
Now if we put left_A and left_B into one set and put right_A and right_B into another set and name 
them left_part and right_part, then we get

```
	left_part		|	right_part
  A[0], A[1], ... , A[i-1]	  A[i], A[i+1], ... , A[m-1]
  B[0], B[1], ... , B[j-1]	  B[j], B[j+1], ... , B[n-1]
```

If we can ensure that 
1. the len(left_part) = len(right_part)
2. max(left_part) <= min(right_part)

then we divide all the elements in {A,B} into two parts with equal length and one part is always
greater than the other. Then 

```
median= (max(left_part)+min(right_part))/2
```
To ensure these two conditions, we need to ensure: 
1. i+j= m-i+n-j (or: m-i+n-j+1) if n>m, we just need to set i=0~m, j= (m+n+1)/2 - i
2. B\[j-1]<=A\[i] and A\[i-1]<=B\[j]

So, all we need to do is search for i in \[0,m] to find an object i such that 
B\[j-1]<=A\[i] and A\[i-1]<=B\[j] where j=(m+n+1)/2 -i

Then we perform a binary search following the steps described below: 

1) Set imin=0, imax=0, then start searching in \[imin, imax]
2) Set i=(imin+imax)/2 , j=(m+n+1)/2 - i
3) Now we have len(left_part) = len(right_part) and there are only 3 more situations which we may 
   encounter: 
   
```
   - B[j-1] <= A[i] and A[i-1]<=B[j] 
     This means that we have found the object i, so we can stop searching

   - B[j-1] > A[i]
     Means A[i] is too small, we must adjust i to get B[j-1]<=A[i] so we increase i because this will
     cuase j to be decreased. We cannot decrease i because when i is decreased, j will be increased
     so B[j-1] is increased and A[i] is decreased (B[j-1]<= A[i] will never be satisfied)

   - A[i-1] > B[j] 
     Means A[i-1] is too big and thus we must decrease i to get A[i-1]<=B[j]. In order to do that we 
     must adjust the searching range to [imin, i-1] so we set imax=i-1 and go back to step 2
```
When the object i is found, then the media is: 

 max(A\[i-1],B\[j-1]), when m+n is odd
(max(A\[i-1],B\[j-1])+min(A\[i],B\[j]))/2, when m+n is even

Next is to consider the edge values i=0, i=m, j=0, j=n where A\[i-1], B\[j-1], A\[i], B\[j] may not exist

```java 
class Solution {
	public double findMedianSortedArrays(int[] A, int[] B) {
		int m=A.length;
		int n=B.length;
		if (m>n) {   	//ensuring that m<=n
			int[] temp=A; A=B; B=temp;
			int tmp=m; m=n; n=tmp;
		}
		int iMin=0, iMax=m, halfLen=(m+n+1)/2;
		while (iMin<=iMax) {
			int i=(iMin+iMax)/2
			int j= halfLen - i;
			if (i<iMax && B[j-1] > A[i]){
				iMin=i+1; //i is too small
			}
			else if (i>iMin && A[i-1]>B[j]) {
				iMax=i-1; //i is too big
			}
			else{ //we have found the object i 
				int maxLeft=0; 
				if (i==0) {
					maxLeft=B[j-1];
				}
				else if (j==0){
					maxLeft=A[i-1];
				}
				else{
					maxLeft=Math.max(A[i-1], B[j-1]);
				}

				if ((m+n)%2 ==1) {
					return maxLeft;
				}

				int minRIght=0;
				if (i==m) {
					minRight=B[j];
				}
				else if (j==n) {
					minRight=A[i];
				}
				else {
					minRight=Math.min(B[j], A[i]);
				}

				return (maxLeft+minRight)/2.0;
			}
		}
		return 0.0;
	}
}
```

**Complexity Analysis**

```
Time Complexity: O(log(min(m,n)))	At first the searching range is [0,m] and the length of this 
					searching range will be reduced by half after each loop so we
					only need log(m) loops. Since we do constant operations in 
					each loop the time complexity is O(log(m) and since m<=n the
					time complexity is O(log(min(m,n))

Space Complexity: O(1)			We only need constant memory to store 9 local variables so the
					space complexity is O(1)
```


<br><br><br>
***
<a name="longestPalindromicSubstring"></a>
# 5-Longest Palindromic Substring

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length
of s is 1000. 

```
Example 1: 

Input: "babad" 
Output: "bab" 

Note: "aba" is also a valid answer 
```

```
Example 2: 

Input: "cbbd"
Output: "bb" 
```

<a name="longestPalindromicSubstringLongestCommonSubstring"></a>
## Longest Common Substring

Some people will be tempted to come up with this quick solution which is unforunately flawed, "reverse
S and become S'. Find the longest common substring between S and S' and that will be the longest
palindromic substring." This will work with some examples but there are some cases where the longest
common substring is not a valid palindrome. 

	Ex. S="abacdfgdcaba", S'="abacdgfdcaba" 	

	The longest common substring between S and S' is "abacd" and clearly this is not a valid 
	palindrome

We can solve this problem however by checking if the substring's indices are the same as the reversed
substring's original indices each time we find a longest common substring. If it is, then we attempt
to update the longest palindrome found so far, if not we skip this and find the next candidate

**Complexity Analysis**
```
Time Complexity: O(n^2) 
Space Complexity: O(n^2) 
```

<a name="longestPalindromicSubstringBruteForce"></a>
## Brute Force 

The obvious brute force solution is to pick all possible starting and ending position for a substring 
and verify if it is a palindrome 

**Complexity Analysis**
```
Time Complexity: O(n^3)		If n is the length of the input string, there are a total of 
				(n 2) = n(n-1)/2 substrings and since verifying each substring takes 
				O(n) time, the run time complexity is O(n^3)

Space Complexity: O(1) 
```

<a name="longestPalindromicSubstringDynamicProgramming"></a>
## Dynamic Programming 

We can improve on the brute force solution by avoid some unnecessary re-computation while validating 
palidromes. Consider the word "ababa", if we already know that "bab" is a palindrome then we can 
determine that ababa is a palindrome by noticing that the two left and right letters connected to bab
are the same. 

This yields a straight forward dynamic programming solution where we initialize the one and two letters
palindromes and then work our way up finding all three letters palindromes and so on.

**Complexity Analysis**
```
Time Complexity: 	O(n^2)	

Space Complexity: 	O(n^2)	Using O(n^2) space to store the table 
```

<a name="longestPalindromicSubstringExpandAroundCenter"></a>
## Expand Around Center

This approach allows us to solve this problem in O(n^2) time using only constant space complexity. We
observe that a palindrome mirrors around its enter and therefore a palindrome can be expanded from its
center and there are only 2n-1 such centers (for palindromes with an even number of letters like 
"abba" its center is in between two letters).

```java 
public String longestPalindrome(String s) {
	if (s==null || s.length() < 1) return "";     //edge case 
	int start=0, end=0;
	for (int i=0; i<s.length(); i++) {
		int len1=expandAroundCenter(s,i,i);
		int len2=expandAroundCenter(s,i,i+1);
		int len=Math.max(len1,len2);
		if (len>end-start) {
			start= i-(len-1)/2;
			end=i+len/2
		}
	}
	return s.substring(start,end+1);
}

private int expandAroundCenter(String s, int left, int right) {
	int L=left, R=right;
	while(L>=0 && R<s.length() && s.charAt(L)==s.charAt(R)) {
		L--;
		R++;
	}
	return R-L-1;
}
```

<a name="longestPalindromicSubstringManacherAlgorithm"></a>
## Manacher's Algorithm 

There is an O(n) algorithm called Manacher's algorithm, however, it is a non-trivial algorithm and no 
one would expect you to come up with this algorithm in a 45 minute coding session


<br><br><br>
***
<a name="zigZagConversion"></a>
# 6-ZigZag Conversion 

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: 
```
P   A   H   N
A P L S I I G
Y   I   R
```
And then read line by line: "PAHNAPLSIIGYIR". Write a code that will take a string and make this 
conversion given a number of rows: 

```
string convert(string s, int numRows);
```

```
Example 1: 

Input: s="PAYPALISHIRING", numRows=3
Output: "PAHNAPLSIIGYIR"
```

```
Example 2:

Input: s="PAYPALISHIRING", numRows=4
Output: "PINALSIGYAHRPI"

Explanation:

P           I          N
A       L   S      I   G
Y   A       H   R
P           I
```

<a name="zigZagConversionSortbyRow"></a>
## Sort by Row 

By iterating through the string from left to right we can easily determine which row in the Zig-Zag
pattern that a character belongs to

<br><br>
*Algorithm*

We can use min(numRows,len(s)) lists to represent the non-empty rows of the Zig-Zag Pattern. 
Iterate through s from left to right appending each character to the appropriate row. The appropriate
row can be tracked using two variables: the current row and the current direction. 

The current direction only changes when we moved to the topmost row or moved down to the bottommost 
row 

```java
class Solution {
	public String convert(String s, int numRows) {
		if (numRows==1) return s;		//if there is only one row return string

		List<StringBuilder> rows=new ArrayList<>();
		for (int i=0; i<Math.min(numRows, s.length()); i++){
			rows.add(new StringBuilder());
		}
		int curRow=0;
		boolean goingDown=false;

		for(char c: s.toCharArray()) {
			rows.get(curRow).append(c);
			if (curRow==0 || curRow==numRows-1) {
				goingDown=!goingDown;
			}
			curRow+=goingDown ? 1 : -1;
		}	

		StringBuilder ret= new StringBuilder();
		for(StringBuilder row:rows) {
			ret.append(row);
		}
		return ret.toString();
	}
}
```

**Complexity Analysis**

```
Time Complexity:  O(n)	where n==len(s)
Space Complexity: O(n)
```

<a name="zigZagConversionVisitbyRow"></a>
## Visit by Row

Visit the characters in the same order as reading the Zig-Zag pattern line by line

<br><br>
*Algorithm*

Visit all characters in row 0 first, then row 1, then row 2, and so on.
For all whole numbers k, 
	* characters in row 0 are located at indexes  k\*(2\*numRows-2)
	* characters in row numRows -1 are located at indexes  k\*(2\*numRows-2)+ numRows -1 
	* characters in inner row i are located at indexes  k\*(2\*numRows-2)+i and (k+1)(2\*numRows-2)-i

```java
class Solution {
	public String convert(String s, int numRows) {
		if (numRows==1) return s; 

		StringBuilder ret=new StringBuilder();
		int n=s.length();
		int cycleLen= 2* numRows -2;

		for (int i=0; i<numRows; i++) {
			for (int j=0; j+1<n; j+= cycleLen) {
				ret.append(s.charAt(j+i));
				if (i!=0 && i!=numROws-1 && j+cycleLen-i<n) {
					ret.append(s.charAt(j+cycleLen-i));
				}
			}
			return ret.toString();
		}
	}
}
```

**Complexity Analysis**
```
Time Complexity: O(n)	where n==len(s) Each index is visited once

Space Complexity: O(n) 	C++ implementation can achieve O(1) if the return string is not considered 
			extra space
```


<br><br><br>
***
<a name="reverseInteger"></a>
# 7-Reverse Integer 

Given a 32- bit signed integer, reverse digits of an integer. 

```
Example 1: 

Input: 123
Output: 321
```

```
Example 2: 

Input: -123
Output: -321
```

```
Example 3: 

Input: 120 
Output: 21
```

For the purpose of this problem assume that your function returns 0 when the reversed integer overflows

<a name="reverseIntegerPopandPush"></a>
## Pop and Push Digits and Check Before Overflow 

We can build up the reverse integer one digit at and time and before doing so we can check whether or
not appedning another digit would cause overflow 

<br><br>
*Algorithm*

Reversing an integer can be done similarly to reversing a string. We want to repeatedly "pop" the last
digit off of x and push it to the back of the rev so that in the end rev is the reverse of x. 

To push and pop digits without the help of some auxiliar stack/array we can use math 

```
//pop operation: 
pop = x%10; 
x/=10;

//push operation:
temp=rev*10+pop;
rev =temp;
```

This statement is dangerous however as the statement temp=rev\*10+pop may cause an overflow and luckily
it is easy to check beforehand whether or not this statement would cause an overflow. 

1. If temp=rev\*10+pop causes an overflow, then rev>=INTMAX/10
2. If rev> INTMAX/10, then temp=rev\*10+pop is guaranteed to overflow
3. if rev==INTMAX/10, then temp=rev\*10 + pop will overflow if an only if pop>7

```java
class Solution {
	public int reverse(int x) {
		int rev=0; 
		while (x!=0) {
			int pop=x%10;
			x/=10;
			if (rev>Integer.MAX_VALUE/10||(rev==Integer.MAX_VALUE/10 && pop>7)) return 0;
			if (rev<Integer.MIN_VALUE/10||(rev==Integer.MIN_VALUE/10 && pop<-8)) return 0;
			rev=rev*10 +pop;
		}
		return rev;
	}
}
```

**Complexity Analysis**
```
Time Complexity:  O(log(x))	There are roughly log10(x) digits in x 
Space Complexity: O(1)
```


<br><br><br>
***
<a name="stringtoInteger"></a>
# 8-String to Integer (atoi)

Implement atoi which converts a string to an integer 

The function first discards as many whitespace characters as necessary until the first non-whitespace
character is found. Then, starting from this character, takes an optional initial plus or minus sign
followed by as many numerical digits as possible and interprets them as a numerical value. 

The string can contain additional characters after those that form the integral number, which are 
ignored and have no effect on the behavior of this function. 

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such
sequence exits because either str is empty or it contains only whitespace characters, no conversion is
performed. 

If no valid conversion could be performed a zero value is returned 

Note: 
* only the space character ' ' is considered as whitespace character 
* assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [-2^31, 2^31-1]. If the numerical value is out of the range of representable values, INT_MAX (2^31-1) or INT_MIN (-2^31) is returned

```
	Example 1: 

	Input: "42"
	Output: 42
```

```
	Example 2: 

	Input: "      -42" 
	Output: -42
```

```
	Example 3:

	Input: "4193 with words "
	Output: 4193
```

```
	Example 4: 
	
	Input: "words and 987"
	Output: 0
```

```
	Example 5:
	
	Input: "-91283472332"
	Output: -2147483648 	//out of the range of a 32-bit signed integer so INT_MIN is returned
```

<a name="stringtoIntegerASCII"></a>
## ASCII Conversion

Recognize that ASCII characters are actually numbers and 0-9 digits are numbers starting from decimal
48 (0x30 hexadecimal) 

```
	'0' is 48
	'1' is 49
	...
	'9' is 57
```
So to get the value of any character digit you can just remove the '0' 

```
	'1' - '0' => 1
	49  -  48 => 1
```

```java
public int myAtoi(String str) {
	int index=0, sign=1, total=0;
	
	//1. Empty string 
	if (str.length() ==0) return 0;

	//2. Remove Spaces 
	while(str.charAt(index)==' ' && index < str.length())
		index++;
	
	//3. Handle signs 
	if (str.charAt(index)=='+' || str.charAt(index)=='-'){
		sign= str.charAt(index) == '+' ? 1:-1;
		index++;
	}

	//4. COnvert number and avoid overflow
	while(index<str.length()){
		int digit= str.charAt(index) - '0'; 
		if (digit<0||digit>9) break;

		//check if total will overflow after 10 times and add digit
		if (Integer.MAX_VALUE/10 < total || Integer.MAX_VALUE/10 == total 
		    && Integer.MAX_VALUE%10<digit) {    
		    return sign==1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
		}
		total= 10* total+digit;
		index++;
	}
	return total*sign;
}
```


<br><br><br>
***
<a name="palindromeNumber"></a>
# 9-Palindrome Number


Determines whether an interger is a palindrome. An integer is a palindrome when it reads the same 
backward as forward. 

```
Example 1: 

Input: 121
Output: true
```

```
Example 2: 

Input: -121
Output: false 
Explanation: 	From left to right, it reads -121, meanwhile from right to left it becomes 121- . 
		Therefore it is not a palindrome
```

```
Example 3: 

Input: 10 
Output: false 
Explanation: 	Reads 01 from right to left. Therefore it is not a palindrome
```

<a name="palindromeNumberRevertHalf"></a>
## Revert Half of the Number

A first idea which may come to mind is to convert the number into a string and check if the string is a
palindrome but this would require extra non-constant space for creating the string not allowed by the 
problem description 

Second idea would be reverting the number itself and comparing the number with the original number, if
they are the same then the number is a palindrome, however if the reversed number is larger than 
int.MAX we will hit integer overflow problem. 

To avoid the overflow issue of the reverted number, what if we only revert half of the int number? The
reverse of the last half of the palindrome should be the same as the first half of the number if the 
number is a palindrome. 

If the input is 1221, if we can revert the last part of the number "1221" from "21" to "12" and compare
it with the first half of the number "12", since 12 is the same as 12, we know that the number is a 
palindrome. 

<br><br>
*Algorithm* 

At the very beginning we can deal with some edge cases. All negative numbers are not palindrome and 
numbers ending in zero can only be a palindrome if the first digit is also 0 (only 0 satisfies this 
property) 

Now let's think about how to revert the last half of the number. For the number 1221 if we do 1221%10 
we get the last digit 1. To get the second last digit we divide the number by 10 1221/10=122 and then
we can get the last digit again by doing a modulus by 10, 122%10=2. If we multiply the last digit by 
10 and add the second last digit 1\*10+2=12 which gives us the reverted number we want. COntinuing this
process would give us the reverted number with more digits. 

Next is how do we know that we've reached the half of the number? 
Since we divided the number by 10 and multiplied the reversed number by 10 when the original number is
less than the reversed number, it means we've gone through half of the number digits. 

```java
class Solution {
    public boolean isPalindrome(int x) {
        if (x<0 || (x%10==0 && x!=0)) {
            return false;
        }
        
        int revertedNumber=0;
        while (x>revertedNumber){
            revertedNumber=x%10+revertedNumber*10;
            x/=10;
        }
        //when the length is an odd number, we can get rid of the middle digit by 
        //revertedNumber/10
        
        //For example when the input is 12321, at the end of the while loop we get x=12, 
        //revertedNumber=123, since the middle digit doesn't matter in a palindrome we can
        //simply get rid of it 
        
        return x==revertedNumber||x==revertedNumber/10;
    }
}
```


<br><br><br>
***
<a name="regularExpressionMatching"></a>
# 10-Regular Expression Matching

Given an input string (s) and a pattern (p), implement regular expression matching with support for '.'
and '*'

```
	'.' Matches any single character
	'*' Matches zero or more of the preceding element 
```
The matching should cover the entire input string (not partial) 

Note: 
* s could be empty and contains only lower case letters a-z
* p could be empty and contains only lower case letters a-z and characters like . or * 

```
Example 1: 

Input:
	s="aa" 
	p="a" 
	Output: false 
	Explanation: 	"a" does not match the entire string "aa" 
```

```
Example 2: 

Input: 
	s="aa"
	p="a*" 
	Output: true 
	Explanation: 	'*' means zero of more of the preceding element, 'a'. Therefore, by repeating
			'a' once it becomes "aa"
```

```
Example 3: 

Input: 
	s="ab" 
	p=".*" 
	Output: true 
	Explanation: 	'.*' means "zero or more (*) of any character (.)"
```

```
Example 4: 

Input: 
	s="aab" 
	p="c*a*b" 
	Output: true
	Explanation: 	c can be repeated 0 times, a can be repeated 1 time. Therefore it matches 
			"aab" 
```

```
Example 5: 

Input: 
	s="mississippi" 
	p="mis*is*p*."
	Output: false 
```


<a name="regularExpressionMatchingRecursion"></a>
## Recursion

If there were no Kleene stars (the * wildcard characters for regular expressions), the problem would 
be easier- we simply check from left to right if each character of the text matches the pattern. When
a star is present we may need to check for may different suffixes of the text and see if they match
the rest of the pattern. A recursive solution is a straightforward way to represent this relationship

```java
class Solution {
	public boolean isMatch(String text, String pattern) {
		if (pattern.isEmpty()) return text.isEmpty(); 
		
		boolean first_match=(!text.isEmpty() && 
				    (pattern.charAt(0)==text.charAt(0) || pattern.charAt(0)=='.'));

		if (pattern.length()>=2 && pattern.charAt(1) =='*'){
			return (isMatch(text,pattern.substring(2))||
			       (first_match && isMatch(text.substring(1),pattern)));
		
		//note: pattern.substring(2) returns all of the characters after index 2 of pattern
		}
		else {
			return first_match && isMatch(text.substring(1), pattern.substring(1));
		}
		
	}
}
```

**Complexity Analysis**

```
Time Complexity: 	Let T, P be the lengths of the text and the pattern respectively. In the worst
			case, a call to match(text[i:],pattern[2j:]) will be made (i+j i) times, and 
			strings of the order O(T-i) and O(P-2*j) will be made. Thus the complexity has
			the order: 

			summation from i=0 to T * summation from j=0 to P/2 * (i+j i) O(T+P-i-2j).

			We can show that this is bounded by O((T+P)2^(T+P/2))

Space Complexity:	For every call to match, we will create those strings as described above 
			possibly creating duplicates. If memory is not freed, this will also take a
			total of O((T+P)2^(T+P/2)) space even though there are only order O(T^2+P^2) 
			unique suffixes of P and T that are actually required 
```

<a name="regularExpressionMatchingDynamicProgramming"></a>
## Dynamic Programming

As the problem has an optimal substructure, it is natural to cache intermediate results. We ask the 
question dp(i,j): does text[i:] and pattern[j:] match? We can describe our answer in terms of answers
to questions involving smaller strings

<br><br>
*Algorithm* 

We proceed with the same recursion as in Approach 1, except because calls will only ever be made to 
match(text[i:], pattern[j:]), we use dp(i,j) to handle those calls instead, saving us expensive 
string-building operations and allowing us to cache the intermediate results 


**Java Top-Down Variation**

```java
enum Result {
	TRUE, FALSE
	
}

class Solution {
	Result[][] memo; 

	public boolean isMatch(String text, String pattern) { 
		memo=new Result[text.length() +1][pattern.length() +1];
		return dp(0,0,text,pattern);
	}

	public boolean dp(int i, int j, String text, String pattern) {
		if (memo[i][j]!=null) {
			return memo[i][j]==Result.TRUE;
		}
		boolean ans; 
		if (j==pattern.length()){
			ans=i==text.length();
		}
		else {
			boolean first_match=(i<text.length() && (pattern.charAt(j) == text.charAt(i) ||
					     patter.charAt(j) == '.'));

			if (j+1<pattern.length() && pattern.charAt(j+1)=='*'){
				ans=(dp(i,j+1,text,pattern)||first_match&& dp(i+1,j,text,pattern));
			}
			else {
				ans=first_match && dp(i+1, j+1, text, pattern); 
			}
		}
		memo[i][j]=ans? Result.TRUE: Result.FALSE; 
		return ans; 
	}
}
```

**Complexity Analysis**

```
Time Complexity: 	Let T, P be the lengths of the text and the pattern respectively. The work 
			for every call to dp(i,j) for i=0,...,T; j=0,...,P is done once and it is O(1) 				work. Hence the time complexity is O(TP)

Space Complexity:	The only memory we use is the O(TP) boolean entries in our cache. Hence, the 
			space complexity is O(TP) 
```

<a name="regularExpressionMatchingNonRecursive"></a>
## Non-Recursive

The recursive programming solutions are pretty confusing so this implementation uses 2D arrays and 
Dynamic Programming 


The logic works as follows: 
```
1. If p.charAt(j) == s.charAt(i) : dp[i][j] = dp[i-1][j-1]; 
2. If p.charAt(j) == '.' : dp[i][j] = dp[i-1][j-1]; 
3. If p.charAt(j) == '*': 
	
	Subconditions
	1. If p.charAt(j-1)!= s.charAt(i):dp[i][j]=dp[i][j-2]  	//in this case a* only counts as empty
	2. If p.charAt(i-1)== s.charAt(i) or p.charAt(i-1) == '.': 
		
		dp[i][j] = dp[i-1][j]	//in this case a* counts as multiple a 
	     or dp[i][j] = dp[i][j-1]	//in this case a* counts as single a 
	     or dp[i][j] = dp[i][j-2]	//in this case a* counts as empty 
```

```java
public boolean isMatch(String s, String p) {
	if (s==null || p==null){
		return false;
	}
	boolean[][] dp=new boolean[s.length()+1][p.length()+1];
	dp[0][0]=true; 
	for (int i=0;i<p.length(); i++){
		if (p.charAt(i)=='*' && dp[0][i-1]){
			dp[0][i+1]=true; 
		}
	}
	for (int i=0;i<s.length();i++){
		for (int j=0;j<p.length();j++){
			if (p.charAt(j)=='.'){
				dp[i+1][j+1]=dp[i][j];
			}
			if (p.charAt(j)==s.charAt(i)){
				dp[i+1][j+1]=dp[i][j];
			}
			if (p.charAt(j)=='*'){
				if (p.charAt(j-1)!=s.charAt(i) && p.charAt(j-1) !='.'){
					dp[i+1][j+1]=dp[i+1][j-1];
				}
				else{
					dp[i+1][j+1]=(dp[i+1][j] || dp[i][j+1] || dp[i+1][j-1]);
				}
			}
		}
	}
	return dp[s.length()][p.length()];
}
```


<br><br><br>
***
<a name="containerwiththeMostWater"></a>
# 11-Container with the Most Water 

Given n non negative integers a1,a2, ... , an where each represents a point at coordinate (i, ai). n 
vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two 
lines, which together with x-axis forns a container such that the container contains the most water. 

```Example: The array [1,8,6,2,5,4,8,3,7] would have a max area of water which is 49. 

		      ^		    ^
	 These two values form the container which could hold water at a max height of 7, these values
	 are also 7 array indexes apart from each other so it could hold water at a max width of 7. The
	 area of water which could be held is thus 7 x 7 = 49
```


<a name="containerwiththeMostWaterBruteForce"></a>
## Brute Force

In this case we simply consider the area for every possible pair of the lines and find out the maximum
area out of those. 

```
public class Solution {
	public int maxArea(int[] height) {
		int maxarea=0; 
		for (int i=0; i<height.length; i++){
			for (int j=i+1;j<height.length;j++){
				maxarea=Math.max(maxarea, Math.min(height[i],height[j])*(j-i));
			}
		}
		return maxarea;
	}
}
```

**Complexity Analysis**
```
Time complexity: 	O(n^2) 	Calculating the area for all n(n-1)/2 height pairs 
Space complexity: 	O(1) 	Constant extra space is used 
```

<a name="containerwiththeMostWaterTwoPointer"></a>
## Two Pointer Approach

The intuition behind this approach is that the area formed between the lines will always be limited by 
the height of the shorter line. Further, the farther the lines, the more will be the area obtained. 

We take two pointers, one at the beginning and one at the end of the array constituting the length of 
the lines. Further, we maintain a variable maxarea to store the maximum area obtained till now. At 
every step, we find out the area formed between them, update maxarea and move the pointer pointing to 
the shorter line towards the other end by one step. 

Initially we consider the area constituting the exterior most lines. Now to maximize the area we need
to consider the area between the lines of larger lengths. If we try to move the pointer at the longer
line inwards, we won't gain any increase in area, since it is limited by the shorter line. But moving
the shorter line's pointer could turn out to be benefical, as per the same argument, despite the 
reduction in width. This is done since a relatively longer line obtained by moving the shorter line's 
pointer might overcome the reduction in area caused by the width reduction. 

```java
public class Solution {
	public int maxArea(int[] height) {
		int maxarea=0, l=0, r=height.length-1; 
		while (l<r){
			maxarea=Math.max(maxarea,Math.min(height[l],height[r])*(r-l));
			if (height[l]<height[r]){
				l++;
			}
			else{
				r--;
			}
		}
		return maxarea; 
	}
}
```

**Complexity Analysis**
```
Time complexity: 	O(n) 	Single pass
Space complexity: 	O(1) 	Constant space is used 
```

<br><br><br>
***
<a name="integertoRoman"></a>
# 12-Integer To Roman 

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M
```
Symbol		Value 
I		1
V		5
X		10
L		50
C		100
D		500
M		1000
```

For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as
XII which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II. 

Roman numerals are usually written largest to smallest from left to right. However, the numeral for 
four is not IIII. Instead, the number four is written as IV. Because the one is before the five we 
subtract it making four. The same principle applies to the number nine which is written as IX. There 
are six instances where subtraction is used: 

* I can be placed before V (5) and X (10) to make 4 and 9 
* X can be placed before L (50) and C(100) to make 40 and 90 
* C can be placed before D (500) and M(1000) to make 400 and 900

Given an integer, convert it to a roman numeral, input is guaranteed to be within the range from 
1 to 3999

```
Example 1: 

Input: 3 
Output: "III" 
```

```
Example 2: 

Input: 4
Output: "IV" 
```

```
Example 3: 

Input: 9 
Output: "IX" 
```

```
Example 4: 

Input: 58 
Output: "LVIII" 
Explanation: L=50, V=5, III=3
```

```
Example 5: 

Input: 1994
Output: "MCMXCIV"
Explanation: M=1000, CM=900, XC=90 and IV=4 
```

<a name="integertoRomanStringArray"></a>
# String Array

```java
public static String intToRoman(int num) { 
	
	String M[]={"", "M", "MM", "MMM"};
	//represents 1000, 2000, and 3000 since we know the number is in the range 1 to 3999
	
	String C[]={"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
	//represents 0, 100,  200,   300,  400, 500,  600,   700,    800,  900

	String X[]={"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
	//represents 0,  10,   20,    30,   40,  50,   60,    70,     80,   90

	String I[]={"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"}; 
	//represents 0,   1,    2,     3,    4,  5,    6,     7,      8,    9

	return M[num/1000] + C[(num%1000)/100] + X[(num%100)/10] + I[num%10]; 
} 
```
















































