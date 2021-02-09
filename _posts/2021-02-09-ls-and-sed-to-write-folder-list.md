---
layout: post
title: "How to write all foldernames in a directory to a file (using ls and sed)"
---

### Problem: 
I want to write all (and only) folder names in a directory to an empty file called 'foldernames.txt'.
### Solution
First thing to tackle would be how to list only folders but no files. While you can achieve this with
```
ls -l | grep "^d"
```
it makes you rely on the ```-l``` flag and gives you the long output with all the meta infos. You can actually do this without using ```grep``` by typing
```
ls -d  */
```
which will list you everyting ending on ```/```. To get rid of that character too, pipe the list to the ```sed``` command
```
sed "s/\///"
```
to replace the forward slash. If you want a comma instead, adjust it to the following:
```
sed "s/\//,/"
```
To write to a file, you have to add ``` > foldernames.txt ``` .
Puttin it all together:
```
ls -d  */ | sed "s/\///" > foldernames.txt
```
