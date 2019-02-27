# LeetCode- CheatSheet

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
I used the following application when building and testing html.

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
1. [1-Two Sum](#twoSum)
    1. [Brute force](#twoSumBruteForce)
    2. [One pass Hash Table](#twoSumOnePassHashTable) 
    
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

* Time complexity:   O(n^2)         we have a nested loop 
* Space complexity:  O(1) 	  we do not allocate any additional memory

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

* Time complexity:   0(n)		each lookup in the hash table only requires O(1) time
* Space complexity:  O(n)		we require additional space for the hash table which stores at most n







