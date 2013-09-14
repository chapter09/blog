---
author: chapter09
comments: true
date: 2013-07-15 15:24:32+00:00
layout: post
title: Django 下拉登陆框
categories:
- Python
tags: [Python]
---
{% include JB/setup %}

	<ul class="nav pull-right">
	  <li class="dropdown" id="menuLogin">
	    <a class="dropdown-toggle" href="#" data-toggle="dropdown" id="navLogin">
	        Login
	    <span class="caret"></span>
	    </a>
	    <div class="dropdown-menu" style="padding:17px;">
	      <form class="form" id="formLogin">
	        <label>电子邮件</label>
	        <input name="username" id="siteuserLoginEmail" type="text" placeholder="电子邮件">
	        <label>密码</label>
	        <input name="password" id="siteuserLoginPassword" type="password" placeholder="密码"><br>
	        <button type="button" id="siteuserLogin" class="btn" referer={{ referer }}>Login</button>
	        <a class="pull-right"  style="margin-top: 15px;" href="">
	            忘记密码？
	        </a>
	      </form>
	        <div id="siteuserLoginWarning" class="alert alert-error" style="display: none;"></div>
	    </div>
	  </li>
	<a class="btn btn-primary" href="/auth/account/register">Register</a>
	</ul>
	


![](/img/uploads/2013/07/django-dropdown.png)