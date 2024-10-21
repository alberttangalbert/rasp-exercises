1. Write an s-op that contains the indices in reverse order.
```
>> reverse_indices = length - (indices + 1);
     s-op: reverse_indices
 	 Example: reverse_indices("hello") = [4, 3, 2, 1, 0] (ints)
>> reverse_indices("hello");
	 =  [4, 3, 2, 1, 0] (ints)
```

2. Write a function that "rotates" the input text by the specified number of characters. 
```
>> def rotate(tokens, k) {return aggregate(select(indices, (indices + k) % length, ==), tokens);}
     console function: rotate(tokens, k)
>> rotate(tokens, 0)("hello");
	 =  [h, e, l, l, o] (strings)
>> rotate(tokens, 1)("hello");
	 =  [e, l, l, o, h] (strings)
>> rotate(tokens, 2)("hello");
	 =  [l, l, o, h, e] (strings)
```

3. Write a function that takes a sequence as input and "swaps every letter with its neighbor". Specifically, for every even index i, positions i and i + 1 will be swapped; if the length of the sequence is odd, then the last element should not move. 
```
>> def swap(s) {sn = select(indices, indices + (1 - 2 * (indices % 2)), ==); li = select(indices, (length % 2 == 1) * ((indices==length-1) * length) - 1, ==); sORl = sn or li; return aggregate(sORl, s);}
     console function: swap(s)
>> swap(tokens)("hello");
	 =  [e, h, l, l, o] (strings)
>> swap(tokens)("ababab");
	 =  [b, a, b, a, b, a] (strings)
```


4. Write a function that returns the maximum value in the sequence repeated for every position.
```
>> def maxseq(s) {return aggregate(select(aggregate(select(tokens, tokens, >), 1) == 0, 1, ==), tokens);}
     console function: maxseq(s)
>> maxseq(tokens)("ababcabab");
	 =  [c]*9 (strings)
>> maxseq(tokens)("hello");
	 =  [o]*5 (strings)
```

5. Skipped 

6. Write a function that performs sequence reversal "autogeneratively". That is, it will take a sequence as input, and the sequence will contain a special token $ that marks the "end of the prompt". The text before the $ should be unchanged, and the text after the $ should be the text before the $ reversed (this text represents the model's response to the prompt). The code should be robuse to the case when the length of text after $ is not the same as the length of text before $. 
```
>>  def reverse_ag(s) {l = aggregate(select(tokens, "$", ==), indices); bf =  select(indices, l, <=) and select(indices, indices, ==); af = select(indices, l, <=) and select(indices, l * 2 - indices, ==); last = select(indices, l * 2, >) and select(indices, indices, ==); return aggregate(bf or af or last, tokens);}
     console function: reverse_ag(s)
>> reverse_ag(tokens)("hello$     ");
	 =  [h, e, l, l, o, $, o, l, l, e, h] (strings)
>> reverse_ag(tokens)("hello$ ");
	 =  [h, e, l, l, o, $, o] (strings)
>> reverse_ag(tokens)("hello$X");
	 =  [h, e, l, l, o, $, o] (strings)
>> reverse_ag(tokens)("hello$XXXXXXXXXX");
	 =  [h, e, l, l, o, $, o, l, l, e, h, X, X, X, X, X] (strings)
```

7. Write a function that counts the number of times a certain token appears in the input sequence.
```
>> def howmany(s, c) {return selector_width(select(s, c, ==));}
     console function: howmany(s, c)
>> howmany(tokens, "a")("hello");
	 =  [0]*5 (ints)
>> howmany(tokens, "h")("hello");
	 =  [1]*5 (ints)
>> howmany(tokens, "l")("hello");
	 =  [2]*5 (ints)
```

8. Write a function that counts the number of times a certain token has appeared in the input sequence so far. 
```
>> def howmany_accumulative(s, c) {return selector_width(select(tokens, "l", ==) and select(indices, indices, <=));}
     console function: howmany_accumulative(s, c)
>> howmany_accumulative(tokens, "a")("hello");
	 =  [0, 0, 1, 2, 2] (ints)
>> def howmany_accumulative(s, c) {return selector_width(select(s, c, ==) and select(indices, indices, <=));}
     console function: howmany_accumulative(s, c)
>> def howmany(s, c) {return selector_width(select(s, c, ==) and select(indices, indices, <=));}
     console function: howmany(s, c)
>> howmany(tokens, "a")("hello");
	 =  [0]*5 (ints)
>> howmany(tokens, "h")("hello");
	 =  [1]*5 (ints)
>> howmany(tokens, "e")("hello");
	 =  [0, 1, 1, 1, 1] (ints)
>> howmany(tokens, "l")("hello");
	 =  [0, 0, 1, 2, 2] (ints)
```

9. Find the Length Longest Sequence of Consecutive Letters. Return length of input if there is not max length.
```
>> def max_ss_len(s) {widths = selector_width(select(s,s, ==)); max_width_selector = select(widths, widths, >); return selector_width(select(selector_width(max_width_selector), 0, ==));}
     console function: max_ss_len(s)
>> max_ss_len(tokens)("albert");
	 =  [6]*6 (ints)
>> max_ss_len(tokens)("hello");
	 =  [2]*5 (ints)
>> max_ss_len(tokens)("byebye");
	 =  [6]*6 (ints)
>> max_ss_len(tokens)("elephant");
	 =  [2]*8 (ints)
>> max_ss_len(tokens)("mississippi");
	 =  [8]*11 (ints)
```

