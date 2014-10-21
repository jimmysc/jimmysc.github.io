---
layout: post
category: Linux
tagline: "常用linux命令"
tag: [linux]
---
{% include JB/setup %}

## awk命令 
强大的文本分析工具，工作流程是按行读取，以空格为默认分隔符将每行切分，切开的部分在进行各种分析处理

使用语法
	
	awk '{pattern action}' input-file}

调用方式
	
	1.命令方式
		awk [-F fs] 'commands' input-file
	2.脚本方式
		#!/bin/awk
	3.文件方式
		awk -f awk-script-file input-file

好好实践-1.命令方式

	/etc/passwd的数据内容格式
	nobody:*:-2:-2:Unprivileged User:/var/empty:/usr/bin/false
	root:*:0:0:System Administrator:/var/root:/bin/sh
	daemon:*:1:1:System Services:/var/root:/usr/bin/false
	_uucp:*:4:4:Unix to Unix Copy Protocol:/var/spool/uucp:/usr/sbin/uucico
	_taskgated:*:13:13:Task Gate Daemon:/var/empty:/usr/bin/false
	_networkd:*:24:24:Network Services:/var/networkd:/usr/bin/false
	_installassistant:*:25:25:Install Assistant:/var/empty:/usr/bin/false
	
	1.统计所有的账号列表($0,$1....的意思)
		awk -F ':'  '{print $1}' /etc/passwd
		
	2.统计账号的个数(内置变量)
		awk -F ':' '{count++;} END{print "user account size is ",count}' /etc/passwd	
		
	3.去重后的账号列表
		awk -F ':' ''
			
	4.统计/etc/passwd文件名，行号(printf使用)
		 awk  -F ':'  '{printf("filename:%s,linenumber:%s,columns:%s,linecontent:%s\n",FILENAME,NR,NF,$0)}' /etc/passwd
		
	5.输出账号的序号(数组，for循环的使用)
		awk -F ':' 'BEGIN{count = 0;} {account[count]=$1;count++;} END{for(i=0 ;i<count;i++) print i, account[i]}' /etc/passwd
	
	6.统计某个文件夹下文件大于1M
		ls -l |awk '{if ($5 > 1024*1024) {print $9" "$5/1024/1024, "M"}}' 
		ls -l /home/admin/work |awk '{if ($5 > 1024*1024) {print $9" "$5/1024/1024, "M"}}' 				




http://www.cnblogs.com/ggjucheng/archive/2013/01/13/2858470.html
