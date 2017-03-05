---
layout: post
title: "French character processing in Python 3"
description: ""
categories: 
tags: []
---


My Python3 script failed at processing a list of names and affiliations. The failure was due to French characters like these:

```
Olivier,Bonaventure,Olivier.Bonaventure@uclouvain.be,Universit̩ catholique de Louvain
``` 

The processing script is as follows:

```python
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
