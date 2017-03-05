---
layout: post
title: "French character processing in Python 3 [Foolish Me]"
description: ""
categories: 
tags: []
---

#### Problem:

In Sublime Text 3, my Python3 script failed at processing a list of names and affiliations. The failure was due to French characters like these:

```
Olivier,Bonaventure,Olivier.Bonaventure@uclouvain.be,Universit̩ catholique de Louvain
``` 

The processing script is as follows:

```python
#!/usr/bin/python3.5

with codecs.open(CSV_FILE, 'r', encoding='utf-8') as csvfile:
    csv_reader = csv.reader(csvfile, delimiter=',')
    for i, row in enumerate(csv_reader):
        try:
            pre_st = ""
            suf_st = ""
            print(pre_st+"    "+row[0]+" "+row[1]+" ("+row[3]+")"+suf_st)
        except UnicodeEncodeError:
            print(i)
```

The error is `UnicodeEncodeError`, which says "__'ascii' codec can't encode character '\u0329' in position 34: ordinal not in range(128)__"

#### Solution:

I modified my script as follows:

```python
print(row[3].encode('utf-8'))
print(bytes(row[3], encoding='utf-8'))
print(row[3].encode('utf-8').decode('utf-8'))
```

The error remained after I ran the script in Sublime. It might be resulted from my incorrect configuration of Sublime's build system for Python 3. This guess was verified after I successfuly executed in iTerm.

With modification on Python 3 build system like below:

```json
{
	"cmd": ["/usr/local/bin/python3", "-u", "$file"],
    "env": {"PYTHONIOENCODING": "utf-8"},
	"file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
	"selector": "source.python"
}
```

`"env": {"PYTHONIOENCODING": "utf-8"},` is the key to save me from the failure. The correct execution result should like this:

```
b'Universit\xcc\xa9 catholique de Louvain'
b'Universit\xcc\xa9 catholique de Louvain'
Universit̩ catholique de Louvain
```


#### Reference:
* [Python unicode cheatsheet](https://www.pythonsheets.com/notes/python-unicode.html)
* [Mark Needham](http://www.markhneedham.com/blog/2015/05/21/python-unicodeencodeerror-ascii-codec-cant-encode-character-uxfc-in-position-11-ordinal-not-in-range128/)
* [从Python 3的bytes/str之别学编码Unicode](http://www.ituring.com.cn/article/61192)
* ["env": {"PYTHONIOENCODING": "utf8"}](http://stackoverflow.com/a/39583630)