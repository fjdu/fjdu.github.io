---
layout: post
title:  "On the python destructor"
date:   2015-03-31 Tue 17:32:16
categories: coding
---

I have never used the python destructor before.  Today I played with it
after seeing a question on Zhihu.

{% highlight python %}

class test:
    tag = '1111'

    def __init__(self, a):
        self.name = a

    def __del__(self):
        print 'In __del__:', 'deleting ', self.name
        print '\t', id(test.tag)
        test.tag = '0000'
        print '\t', id(test.tag)

a = test('a')
print 'a: ', id(a.tag), id(test.tag)

class test:
    tag = '2222'

    def __init__(self, a):
        self.name = a

    def __del__(self):
        print 'In __del__:', 'deleting ', self.name
        print '\t', id(test.tag)
        test.tag = '3333'
        print '\t', id(test.tag)
        print test.tag

b = test('b')
print 'b: ', id(b.tag), id(test.tag)

class test1:
    tag = '3333'

    def __init__(self, a):
        self.name = a

    def __del__(self):
        print 'In __del__:', 'deleting ', self.name
        print test.tag


c = test1('c')

print 'c: ', id(c.tag), id(test1.tag)

# Result:
# a:  4517161072 4517161072
# b:  4517161312 4517161312
# c:  4517161456 4517161456
# In __del__: deleting  a
# 	4517161312
# 	4517161264
# In __del__: deleting  c
# 0000
# In __del__: deleting  b
# 	4517161264
# 	4517161456
# 3333

{% endhighlight %}

Note that the first two classes have the same name "test", while the third one
is named "test1".

Here are two observations:

- `a` is destructed first.  In its destructor, the static class variable `test.tag` is set to '0000'.
- `b` is destructed last.  Its destructor prints out the the static class variable `test.tag` as '0000'.

The reason is:

- When `a` is destructed, the '`test`' class of `a` is destructed, *before* the
  `__del__` function is called, so the `test.tag = ...` in its destructor is
  actually referring to the '`test`' class of `b` (they have the same `id`).
  That's why when `b` is destructed, the `test.tag` has a value pf '0000'
  rather than '2222'.

Note that when `b` is being destructed, the `test.tag` still exists, which
means the class can be destructed later than the object itself.

`b` is destructed later than `c`, because in the definition of `test1` it
refers to `test.tag` (which is not a typo).  But in general, the order of
calling the destructors does not have a simple pattern.

<!---
[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
-->
