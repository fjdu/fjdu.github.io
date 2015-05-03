---
layout: post
title:  "Problems from programming pearls chapter 2"
date:   2015-04-25 Sat 23:52:01
categories: algorithm
---

1. Given a sequential file that contains at most \\(4\times 10^9\\)
   32-bit integers in random order, find a 32-bit integer that is NOT in the
   file.
1. Rotate a one-dimensional vector of n elements left by i positions.  Do
   the job with only a few dozen extra bytes of storage.
1. Given a dictionary of English words, find all sets of anagrams (pots -
   stop).

For the first one, I was tempted to apply Cantor's diagonal argument to it.
Obviously this won't work, since we do not have an infinite number of digits
(only 32 digits).

Could one devise a kind of operation, such that when a new element comes, it
always produce a number that differ from all the existing ones?

For the second one, what I thought of is something like
{% highlight haskell %}
    movelist a b = if (length a <= length b)
                   then (take (length a) b) ++ (movelist a (drop (length a) b))
                   else b ++ a
{% endhighlight %}
