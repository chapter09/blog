---
layout: post
title: "ptrace() and wait() 的用法"
description: ""
category: 
tags: []
---
{% include JB/setup %}

在子进程execvp的程序为：

	#include <stdio.h>
	#include <stdlib.h>
	#include <sys/wait.h>
	#include <sys/types.h>
	#include <sys/ptrace.h>
	#include <unistd.h>
	#include <signal.h>
		
	int main() {
	    char * argv[] = {"ls", "-a", "/home", 0};
	    printf("[child]before exe\n");
	    execvp(argv[0], argv);
	    return -1;
	
	}
其中还有一个execvp的调用，这同样会导致子进程suspend，因为子进程被ptrace 跟踪了。

	#include <stdio.h>
	#include <stdlib.h>
	#include <sys/wait.h>
	#include <sys/types.h>
	#include <sys/ptrace.h>
	#include <unistd.h>
	#include <signal.h>
	
	static int a = 0;
	
	int child() {
	    a = 1;
	    ptrace(PTRACE_TRACEME, 0, NULL, NULL);
	
	    char * argv[] = {"./exec", 0};
	    printf("before exe\n");
	    a = 2;
	    printf("[3] a: %d\n", a);
	    execvp(argv[0], argv);
	
	    return -1;
	}
	
	static void cld_handler(int n) {
	    printf("cld_handler\n");
	}
	
	int main() {
	    int ret;
	    int status;
	    int pid = fork();
	
	    if(pid == 0) {
	        //child
	        exit(child());
	    }
	
	    ret = waitpid(pid, &status, WUNTRACED); // 等待子进程最外面的execvp
	    //for(;;);
	    ptrace(PTRACE_CONT, pid, NULL, NULL); //如果PTRACE_CONT换成PTRACE_DETACH，
	    //则第二execvp不会导致子进程挂起
	
	    ret = waitpid(pid, &status, WUNTRACED); // 等待子进程最里面的execvp
	    for(;;); // 因为这个死循环的存在，子进程仍然suspend在那
	
	    signal(SIGCHLD, cld_handler);
	    //a = 3;
	    printf("[2] a: %d\n", a);
	
	    return 0;
	}
