---
title: 'C++ Generating code with macro'
date: 2016-09-22
permalink: /posts/2016/09/generating-code-with-macro/
tags:
  - C++
  - Code
published: true
excerpt: "In order to avoid repetition of code, which means a big No to copy-and-paste code, I need to put similar code into a macro, or a function. Considering this, macro wins over the choice of using C++ function."
---
In order to avoid repetition of code, which means a big No to copy-and-paste code, I need to put similar code into a macro, or a function. Considering this, macro wins over the choice of using C++ function. This is not something new, absolutely not but it was so clumsy for my first and second attempts. Therefore, this post summarizes hopefully in a clear manner what is needed to achieve "code generation".

Everything you should know about C/C++ marco is listed here <a href="https://gcc.gnu.org/onlinedocs/cpp/Macros.html#Macros" title="Marcos">Marcos</a>. In particular, I refer to the subsection <a href="https://gcc.gnu.org/onlinedocs/cpp/Concatenation.html#Concatenation" title="Marcos - Concatenation">Marcos - Concatenation</a>.

In this post, I will describe here some simple code with generating some function declaration.  The first thing first, I create <strong>Test.hpp</strong> file to host macros. <strong>Line 2-4</strong> defines a dictionary that contains

{% highlight shell %}
#pragma once
#define MY_MACRO_DICTIONARY(macro) \
	macro(Integer, integer, int, int ) \
	macro(Float, float, float, float ) \

#define MY_DECLARE_GENERATED_FUNCS(name, kname, in_type, out_type) \
	\
	 out_type sum_two_numbers_ ## name(in_type a, in_type b) \
	{ \
		return a + b; \
	} \
	\
   out_type substract_two_numbers_ ## name(in_type a, in_type b) \
	{ \
		return a - b; \
	} \

 MY_MACRO_DICTIONARY(MY_DECLARE_GENERATED_FUNCS)
#undef MY_DECLARE_GENERATED_FUNCS
#undef MY_MACRO_DICTIONARY
{% endhighlight %}

The code is self-explanatory I think but <strong>Line 8 and Line 13</strong> use ** ## name** to concatenate the left and the right tokens with the argument <strong>name</strong> as token.

The last piece of this puzzle is the <strong>Test.cpp</strong> that includes <strong>Test.hpp</strong> and use the generated functions.

{% highlight shell %}
#include <iostream>
#include &quot;generate_macro.hpp&quot;

int main()
{
	sum_two_numbers_Float(1.0, 2.0);
	return sum_two_numbers_Integer(1, 2);
}
{% endhighlight %}
Notes
By the way, there is a link which describes how to wrap your code block in Wordpress <a href="https://en.support.wordpress.com/code/posting-source-code/">here</a>.  And of course, you should enable writing in Markdown for your site as described <a href="https://en.support.wordpress.com/markdown/">here</a>