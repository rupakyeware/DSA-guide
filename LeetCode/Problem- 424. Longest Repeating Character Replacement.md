Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[2 pointers]], [[Sliding Window]], [[Frequency Map]]

At first I thought this problem is similar to [[Problem- 3. Longest Substring Without Repeating Characters]] but as I went on trying to come up with a solution, I realized it's very different. I was not able to come close to the final solution by myself. The only part I got correct was using 2 pointers and a [[Frequency Map]]. But anybody could've got that. I learned something important in this though.

`Note: 2 pointers can be implemented with the help of a for loop too!`
2 pointers is != while(r < n)

## Approach
We want to iterate through the array by keeping 2 pointers, l and r. The aim is to find the longest window that can be formed by iterating through the string n times using the r. I made a function that checks if the current window is valid or not by iterating through the freq vector, finding the highest occurring element, and then returning true if the window_size - max_frq <= k

If the window is not valid, we decrease the frq of the lth element, and move the l pointer forward. We also have to decrement the value of the curr_len variable.

Then we update the value of longest if needed.

### Code 
```
class Solution {

public:

// Function to check if the current window is valid

bool isValid(vector<int> &freq, int k, int window_size) {

int highest = 0; // Store the frq of the highest occuring element

for(int i: freq) {

if(i > highest) highest = i;

}

  

// We want to flip the elements other than the highest occuring element

// To find their count, we subtract the frq of the highest occuring element

// from the window size

// If the count is <= k, the current window is valid

return window_size - highest <= k;

}

  

int characterReplacement(string s, int k) {

vector<int> freq(26, 0); // Frq array to store count of elements

int n = s.size();

if(n == 1) return 1;

  

// We will be iterating on r

// And checking the longest valid subarr that can be formed with l

int l = 0; // Init l to 0

int longest = 0;

for(int r = 0; r < n; r++) {

freq[s[r] - 'A']++; // Consume the rth element

int curr_len = r - l + 1; // Find len of curr window

// Find the longest valid window that can be formed with r

while(!isValid(freq, k, curr_len)) {

// Current window is not valid

freq[s[l] - 'A']--; // Reduce the count of the lth element

l++; // Move l forward

curr_len--; // new len of window

}

  

longest = max(longest, curr_len); // Update longest if needed

}

  

return longest;

}

};
```
