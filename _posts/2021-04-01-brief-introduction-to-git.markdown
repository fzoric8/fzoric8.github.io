---
layout: post
title:  "Brief Git Introduction!"
---


# Brief introduction to git 

Git is software for version control. I know what’s software, but version control was a tricky one. 
Version control software is used by software developers to maintain documentation and configuration files as well as source code. 
For bigger software projects with multiple contributors, it’s impossible to track progress, activity, development status, bugs, errors, issues, conflicts without version control. 
Therefore, software for version control is rather a necessity than a luxury. 
Everyone working with a piece of software (development/maintaining) or aiming to program at some part of their career, (un)fortunately must-know fundamentals of version control.

## What is Git? 

Main difference between Git and other VCS is how Git stores and thinks about information. 
Most of the VCS before git used something called `delta-based` version control 
which tracked changes in source code files and created chains of changes overtime. It's 
possible as shown in:

![Illustration of delta-based VCS]( {{ BASE_PATH }} /assets/images/git1/git1.png)  
<div align="center"> Figure 1. Illustration of delta-based VCS </div>   


Git doesn't think of or store its data this way. Git thinks of its data more like a series 
of snapshots of miniature filesystem. Every time you commit (e.g. save current state of the code) 
Git basically takes picture of what your files look like and stores a reference to that 
snapshot. Therefore every change in file as treated as new file. Old files that haven't 
changed are not saved again for memory conservation. On next image, we can see different 
code versions (commits) that share unchanged files in checkins (changes) over time, yet
save files that have been changed since last checkin. 

![Illustration of git VCS]( {{ BASE_PATH }} /assets/images/git1/git2.png)  
<div align="center"> Figure 2. Illustration of git VCS </div>   


## Basic terms explained:

It's easier to get started when you understand basic terms and concepts:  
 * **Repository** &rarr; contains all of your project's files and each file's revision history  
 * **Fork** &rarr; personal copy of someone else's project   
 * **Clone** &rarr; creating local copy of the remote repo  
 * **Pull** &rarr; fetch and download content from a remote repository and immediately update the local repository to match the content
 * **Branch** &rarr; basically pointer to a snapshot of your changes 
 * **Commit** &rarr; save your changes to local repository 
 * **Push** &rarr; command that is used to upload local repository content to a remote repository 


## Resources 

- [What is version control](https://www.google.com/search?channel=fs&client=ubuntu&q=why+do+we+need+software+version+control) 
- [Why do we need version control?](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwiM6PKg5NrvAhUF_aQKHRb9A1MQFjACegQIAhAD&url=https%3A%2F%2Fmedium.com%2F%40lanceharvieruntime%2Fversion-control-why-do-we-need-it-1681f4888cec&usg=AOvVaw29wT804SmlRfVMF1MPEZGD) 
- [Getting started - What is Git?](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F) 
- [Git documentation](https://git-scm.com/docs) 
