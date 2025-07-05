There are a few things to just keep in your mind when it comes to [[Bit manipulation]]. They are [[Unintuitive]] so you can't understand them. Just remember the tricks

1. **Finding 2 elements occurring an odd number of times in an array of elements occurring even number of times.**
	Ans. First we need to take a xor of the entire array and then we'll be left with a^b where a and b are the elements we want. Then we do **==xor & ~(xor-1)==** to get the value of the lsb. Then we divide the array into two based on if the lsb is set or not. And then we perform xor on each of the arrays to get the 2 elements. 
	This has been shown in [[Problems- Find 2 elements occurring once]].
2. **Find the xor of elements from 1 to N**
	Ans. Here N can be very large so doing it in a brute force manner won't work. It can be done in O(1). First we **==mod the number with 4==**. Then the answer depends upon the value of the remainder. 
	0: N
	1: 1
	2: N+1
	3: 0
	Extremely unintuitive but worth keeping in mind all the same. 