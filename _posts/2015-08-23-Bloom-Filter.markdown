---
layout: post
title:  "Bloom Filter"
date:   2015-08-23 Sun 22:36:29
categories: algorithm
---

You have a large number of email addresses that are marked as spam senders and
needed to be filtered.  The idea is to set up a moderately large bit vector
initialized to all zero, and map each input email address (as a seed to a
number of different random number generators) into a small number of locations
in the vector and set these bits to one.

Now, when a new email address comes, the same number of locations will be
generated, and if all the locations have a bit value of one, then it is very
likely that this new email address is sending spam.
