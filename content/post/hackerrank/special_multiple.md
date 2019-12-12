
---
title: "Hacker Rank Special Multiple"
thumbnailImagePosition: left
thumbnailImage: /img/thumb_cards.jpg
metaAlignment: center
coverMeta: out
date: 2019-07-18
categories:
- hackerrank
tags:
- random
- code
---

Not a hard one: `You are given an integer N. Can you find the least positive integer X made up of only 9's and 0's, such that, X is a multiple of N?`

Any number consisting of just 9s and 0s is divisible by 9.  I see a few ways to use that, also 9s and 0s are two unique characters, so another way would be to process numbers are strings matching those characters rather than evaluating every multiple of N until you hit one thats divisible by 9, as something can be divisible by 9 but not be only 9s and 0s.


## Brute force

Also the scenario limits N to up to 500, so its a small set, and doesn't take long to brute force with the any() function.

```
def solve_brute(n):
    #brute solution
    org_num = n
    while True:
        if (n % 9) == 0:
            if not any(x != '9' and x != '0' for x in list(str(n))):
                print(n)
                break
        n+=org_num
```

But we hit a timeout, its not obvious until you run it up to 500. And its a trap as you do the test cases and they complete nearly instantaneously.

And many numbers complete instantly.  Lets look at numbers 180-200

```
number,multiple,time to find in seconds
180,900,0.0000
181,9009999,0.0078
182,90090,0.0001
183,900909,0.0014
184,990909000,0.9173
185,9990,0.0000
186,9990990,0.0170
187,9009099,0.0068
188,9009900,0.0079
189,90909,0.0003
190,990090,0.0007
191,99909999,0.0837
192,9000000,0.0166
193,99009,0.0001
194,999000090,0.8485
195,90090,0.0001
196,990000900,0.7912
197,9909000909,9.6778
198,990,0.0000
199,999000099,0.8587
```

Its almost random.  Odd numbers seem to stand out as particularly cumbersome.  And anything divisible by 10 (ending with a 0), is superfast.  Even numbers will add up to 10 more often than odd, so that makes sense.

Graphing the first 200, which takes 4 minutes to generate!

![Multiples up to 200](/img/multiples_graph.png)

Ok, so primes/odds are going to have weird multiples and the line up to a specific set divisible by 9 is going to be unlikely, so it takes forever just iterating one multiple at a time.

## The other side

Lets try the other direction, how many numbers even exist with just 9s and 0s?

```
def gen_nines():
    nines = []
    n=1
    while n < 100000:
        if not any(x != '9' and x != '0' for x in list(str(n))):
            nines.append(n)
            print(n)
        n+=1
    return nines
```

Running that we get only 31 results such as:

```
9
90
99
900
909
990
999
...
```

For a million there are 63,for 100 million 255 numbers.  And improving the above a bit to just add 9 instead of every number n, we can generate the 255 in 10 seconds ahead of time.  

## Pre-generated multiples ?

The question is how high do we need to go ? I do not see an obvious way to know. I can make sure the code returns a result for every number **N**, as there must be an answer.

From the first 200 in the brute force I could see the result for 76 was 990,909,090 which is way above 100 million.  And that takes forever to generate. Ok, well lets see if there is a pattern in the numbers of 9s and 0s.

Generating the number + how many times its divisible by 9:

```
9 1.0
90 10.0
99 11.0
900 100.0
909 101.0
990 110.0
999 111.0
9000 1000.0
9009 1001.0
9090 1010.0
9099 1011.0
9900 1100.0
9909 1101.0
9990 1110.0
9999 1111.0
```

Suspicious the the right column is the binary number line counting up from 1. 9090 is divisible by 9 1010 times, but each multiple is going up by 1 if you think of the number as binary.

That gives us a mapping to generate any number of 9 multiples we want, so we can quickly generate say, the first thousand of them.

The steps would be:

* Count up binary numbers by 1
* Multiple the binary number as a literal base 10 int by 9
* Add result to list

Python trickery:

```
def binary_nines(n):
    nines = []
    for i in range(1,n):
        bin_int = int(bin(i).replace("0b",""))
        nines.append(bin_int)
    return nines

# for the first 10000 numbers of only 9s and 0s
time python test.py
99999

real    0m0.087s
user    0m0.078s
sys     0m0.000s
```

That is a lot better! Lets see if we get all the numbers checking this list quickly.

```
def binary_nines(n):
    nines = []
    for i in range(1,n):
        bin_int = int(bin(i).replace("0b",""))
        #print(i,bin_int*9)
        nines.append(bin_int*9)
    return nines
nines = binary_nines(100000)


for i in range(1,501):
    for nine in nines:
        if nine % i  == 0 and i < nine:
            print(i,nine)
            break
```

Gets all 500 in 0.123s !

But hilariously the hackerrank scenario at https://www.hackerrank.com/challenges/special-multiple/problem has bad test cases.  So you can't submit correctly :)

Fun problem, though.
