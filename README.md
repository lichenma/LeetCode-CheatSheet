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
    
<br><br><br>
***
<a name="twoSum"></a>
## 1-Two Sum 

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.
You may assume that each input would have **exactly one solution**, and you may not use the same element twice.

Example:
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```
<a name="twoSumBruteForce"></a>
### Brute Force 

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
### One Pass Hash Table 

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
## 2-Add Two Numbers 

Given two non-empty linked lists representing two non-negative integers with the digits stored in 
reverse order and each node containing a single digit, add the two numbers and return as a linked list

Example: 
```
Input (2 -> 4 -> 3) + (5 -> 6 -> 4) 
Output 7 -> 0 -> 8 

342 + 465 = 807
```
<a name="addTwoNumbersElementaryMath"></a>
### Elementary Math Solution

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
## 3-Substring No Repeat 

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
### Brute Force 

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
### Sliding Window 

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

### Sliding Window Optimized 

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
## 4-Median of Two Sorted Arrays 

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
















