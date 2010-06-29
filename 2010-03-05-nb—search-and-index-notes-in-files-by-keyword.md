---
layout: post
title: nb—search and index notes in files by keyword 
categories: manual
---
nb name search index keyword  
nb—search and index notes in files by keyword  
  
  
nb keyword  
  
  
nb description search keyword path file line acme  
Nb searches for the given keyword in each nbindex file listed  
in $HOME/nbindexes.  If it finds a match, nb prints the path,  
filename, and line number of the indexed file in the format  
acme uses to refer to lines in a file.  
  
  
nb file format store index line path file line acme nbindexes  
This file is an example.  Nb searches all files in the current  
directory for lines which begin `nb `, and copies them into  
the file `nbindex`.  Each match is listed with its filename,  
and line number, e.g.  for acme to show the line on a  
right-click of the mouse.  It then adds the path and filename  
of the current directory’s `nbindex` file to the file  
`$HOME/nbindexes`.  
  
By convention I use 1 blank line to separate paragraphs after  
an nb line, and 2 blank lines to separate nb lines, but nb  
doesn’t care about this.  
  
  
nb nbindex example search keyword  
This is the nbindex file for this file.  

	nb.1:1 name search index keyword
	nb.1:5 keyword
	nb.1:8 description search keyword path file line acme
	nb.1:15 file format store index line path file line acme nbindexes
	nb.1:28 nbindex example search keyword
	nb.1:50 source plan9port rc script
	nb.1:70 port plan9port rc shell script grëp
	nb.1:75 author jason.catena@gmail.com
	nb.1:78 bugs

This is the output of the command `nb keyword`.  It lists the  
full path since it may refer to files anywhere in the filesystem.   

	/usr/local/plan9/bin/nb.1:1 name search index keyword
	/usr/local/plan9/bin/nb.1:5 keyword
	/usr/local/plan9/bin/nb.1:8 description search keyword path file line acme
	/usr/local/plan9/bin/nb.1:28 nbindex example search keyword
  
  
nb source plan9port rc script  

	#!/usr/local/plan9/bin/rc
	flag e +
	
	catalog=$HOME^'/nbindexes'
	if(test -r $catalog && ! ~ $1 ''){
		list=`{cat $catalog}
		grëp $* $list | sed 's,nbindex:[0-9]+: ,,'
	}
	
	grep -n '^nb ' * >[2]/dev/null | sed 's,: ?nb +, ,' > nbindex
	echo `{pwd}^'/nbindex' >> $catalog
	if(~ $TMPDIR ''){
		TMPDIR=/var/tmp
	}
	tmp=$TMPDIR^'/nbindexes.'^$USER^'.'^$pid
	sort -u $catalog > $tmp
	mv $tmp $catalog
  
  
nb port plan9port rc shell script grëp  
Nb in written in plan9port’s rc, but should be straightforward  
to port to any shell.  Use grep instead of grëp.  
  
  
nb author jason.catena@gmail.com  
  
  
nb bugs  
Nb generates the index after it searches, to present results  
immediately, so it does not show changes since its last run.  
If you want to see the latest changes, then run nb with no  
parameters before you supply a keyword (e.g.  in acme, middle-  
click on nb after you edit an nb line or following text).  
