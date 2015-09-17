---
layout: post
title:  "A problem caused by ShareThis"
date:   2015-09-17 Thu 16:17:19
categories: coding
---

I included a `ShareThis` button in these blog pages just for fun.  However, it
caused a major headache to me today.

The `ShareThis` button is included as follows:

{% highlight html linenos=table %}
<script type="text/javascript" src="http://w.sharethis.com/button/buttons.js">
</script>
<script type="text/javascript">
    stLight.options({publisher: "5f23ad09-232d-48f3-8b7b-89f9c51d2a5d",
                     doNotHash: true,
                     doNotCopy: false,
                     hashAddressBar: false});
</script>
{% endhighlight %}

By default `doNotHash` is set to `false`, this will make the copy-and-paste
function of the page misbehave: all the lines will be copied into a single line
(if only copying part of the page), appended with a "See more at url" message,
which is not needed (and very annoying in any imaginable context).
