# kadane-Algorithm
#### Explanation of Kadane’s Algorithm

Kadane's Algorithm is a weird one to wrap your head around. It definitely took me a while to fully understand how it works and what's going on. Here are my own words about it (which may confuse you more! :sweat_smile:)

Please read what the algorithm is supposed to achieve first before reading.

Here is the algorithm in Python:
```Python
def solution(A):
  maxEnding = maxSlice = A[0]
  for v in A[1:]:
      maxEnding = max(v,maxEnding + v)
      maxSlice = max(maxEnding,maxSlice)
  return maxSlice
```
The above is the simplist form of the algoritim.
The confusing part for me was the:
```Python
maxEnding = max(v,maxEnding + v)
```
this is equivalent to saying:
```Python
if v > maxEnding + v:
  maxEnding = v
else:
  maxEnding = maxEnding + v
```
but again, what is maxEnding and what exactly is it keeping track of? 

Well, it's actually **the max sum of that position**. We are in fact **moving through the array and working out the max sum at that position(index)**.

The best way to understand is by looking at small examples.
Lets say we have the array:
```
[5,-7,3,5]
```
Lets start with the first value `i=0` with the array `[5]`. Of course, the max sum is 5 due to it being the only value

Next we have `i=1` so we're looking at the array `[5,-7]`. We're saying that at position index = 1, what would be the max sum of this spot? Remember that max sums only go left since we are measuring the max sum at this position, so we can only compare `[-7]` and `[5,-7]`. Thus, the max sum would be `-2`, since `5+(-7)` is > than `-7`

So the current list would look like:
```
Looking at [5,-7,3,5]
The max sum so far is [5,-2] up to the first 2 index.
We will designate a paired M list to hold the array that created the max sum. E.g [[5],[5,-7]]
So M[1] = [5,-7]. We assigned this due to the working out above.
```

When we move to `i=2`, this is where it gets interesting. We are now looking at `[5,-7,3]` and determining the max sum at index 2. Thus we are comparing `[3]`,`[-7,3]`,`[5,-7,3]`. 

Thinking about it, the two terms `[-7,3]`,`[5,-7,3]` which corresponds to saying (the max of `[-7]`,`[5,-7]`) + 3. We worked the max result above and assigned the `M[1]`, hence  (`M[1]`) + 3. 

Note, you could also imagine (the max of `[-7]`,the max of `[5]` + (-7)) + 3

Overall, it's comparing `[3]` to `M[1] + 3`. The winner is 3.
```
Looking at [5,-7,3,5]
[5,-2,3]
[[5],[5,-7],[3]]
```

By now, you should have a faint idea on how to work out the next one, and why it works. At `i=3`
We're compairing `[5]`,`[3,5]`,`[-7,3,5]`,`[5,-7,3,5]`. Looking at `[3,5]`,`[-7,3,5]`,`[5,-7,3,5]`, this is (the max of `[3]`,`[-7,3]`,`[5,-7,3]`) + 5 . Following the same steps as above, this is `M[2] + 5`, which is `[3] + 5`.

Expanded out would look like: (the max of `[3]`, (the max of `[-7]`,the max of `[5]` + (-7)) + 3) + 5

The result of the comparison of `M[2] + 5` and `5` is 8.

Thus for the max sum subsection of `[5,-7,3,5]`, the largest value is 8 from `[3,5]`.

Overall, because we don't care about which elements makes the sum (unless the question asks for it), we only need to keep the running total. So really the array M is not needed.
