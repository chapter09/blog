---
author: chapter09
comments: true
date: 2012-03-01 07:57:49+00:00
layout: post
slug: how-to-auto-generate-doc-for-python-code
title: 'How to Auto Generate Doc for Python Code '
wordpress_id: 602
categories:
- Tech
tags:
- Python
---




### 



	
  * **Index**

	
    * **Requirements:**

	
    * For CLI user:

	
    * **For GUI user:**

	
    * Tips for comment blocks in Python:








Here there is a tool for generating documentation for Python (not only Python). Let's take a look:


#### **Requirements:**





	
  * Download [Doxygen](http://www.stack.nl/~dimitri/doxygen/download.html#latestsrc) and install

	
  * Download [Graphviz](http://www.graphviz.org/Download_windows.php) (help in generating graphs such as UML)




#### For CLI user:





	
  1. Creating a configuration file:
To create a template of config file:
**_doxygen -g <config-file><!-- more --> _**

	
  2. Set up the value of **INPUT** = path of files and/or directories that contain documented source files

	
  3. Add file patterns to the **FILE_PATTERNS** (like: *.py, *.h)

	
  4. Since python looks more like Java than like C or C++, you should set **OPTIMIZE_OUTPUT_JAVA** to **YES** in the config file.

	
  5. After the setting up of other tags, run:
**_doxygen config-file_**

	
  6. Get the documentation files




#### **For GUI user:**





	
  1. In the Wizard mode, you only need to set up a few important items, such as **working directory**, **the source code path** etc.
After setting, click the tab 'Run', Run doxygen then you could generate a set of Documentation for your source code :) 

[caption id="attachment_607" align="alignnone" width="300" caption="doxygen"][![doxygen](http://haow.ca/wp-content/uploads/2012/03/doxygen-300x285.png)](http://haow.ca/wp-content/uploads/2012/03/doxygen.png)[/caption]

	
  2. In the Expert mode, there are far more parameters for you to edit. There is detailed [documentation](http://www.stack.nl/~dimitri/doxygen/config.html) describing the config format.
Even you can adjust the font color, size and some filters here.




#### Tips for comment blocks in Python:


There are two methods to make up comment blocks, the first is just using """ """ as usually we do to leave comments, but in this case, none of Doxygen's [special commands](http://www.stack.nl/~dimitri/doxygen/commands.html#cmd_intro) is supported.

the grammar of those special commands looks like Latex. So we just apply the second one  which uses ###:

[![code](http://haow.ca/wp-content/uploads/2012/03/Capture2.png)](http://haow.ca/wp-content/uploads/2012/03/Capture2.png)[![doc](http://haow.ca/wp-content/uploads/2012/03/Capture.png)](http://haow.ca/wp-content/uploads/2012/03/Capture.png)



