---
layout: post
title:  "Hello World!"
date:   2015-11-05 12:45:09 +0200
categories: jekyll update
---

One-Two-Three... One-Two-Three... We're up and running!

Ok, TeX is supported:

$$P(A|B) = {P(B|A)P(A) \over P(B)}$$

Some code snippet here:

{% highlight cpp %}
#include <metal_stdlib>
using namespace metal;

kernel void sigmoid(const device float *inVector [[ buffer(0) ]],
                    device float *outVector [[ buffer(1) ]],
                    uint id [[ thread_position_in_grid ]]) {
    // This calculates sigmoid for _one_ position (=id) in a vector per call on the GPU
    outVector[id] = 1.0 / (1.0 + exp(-inVector[id]));
}
{% endhighlight %}

Some image:

![Electrical telegraph](/images/morse.jpg)
<strong>Electrical telegraph</strong>

`` .... . .-.. .-.. --- / .-- --- .-. .-.. -.. ``

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
