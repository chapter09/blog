---
layout: post
title: "Using python's eval() vs. ast.literal_eval()?"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

`datamap = eval(raw_input('Provide some data here: ')` means that you actually evaluate the code before you deem it to be unsafe or not. It evaluates the code as soon as the function is called. See also the dangers of eval.

`ast.literal_eval` raises an exception if the input isn't a valid Python datatype, so the code won't be executed if it's not.

Use `ast.literal_eval` whenever you need eval. If you have Python expressions as an input that you want to evaluate, you shouldn't (have them).

[Reference Link](http://stackoverflow.com/questions/15197673/using-pythons-eval-vs-ast-literal-evals)