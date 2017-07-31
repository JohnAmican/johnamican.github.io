---
layout: post
title:  "Bug in old Ruby"
date:   2017-07-30 19:04:54 -0400
categories: ruby
---
A long time ago I found [a bug in Ruby core][bug]. If you're on any `p` version of
1.9.3, you're affected by this.

{% highlight ruby %}
def foo(bizz=:bizz, baz=:baz, buzz=:buzz)
end

Object.send(:define_method, :bar) do |bizz=:bizz, baz=:baz, buzz=:buzz|
end

method(:foo).arity # expected: -1, actual: -1
method(:bar).arity # expected: -1, actual: 0
{% endhighlight %}

The "arity" of a function is the number of arguments it accepts. `arity` in
ruby respects this definition if the method only has required arguments. If
there are optional arguments (ex. `arg=:arg`), it should return **-n-1** where
n is the number of required arguments.

[bug]: https://bugs.ruby-lang.org/issues/8411
