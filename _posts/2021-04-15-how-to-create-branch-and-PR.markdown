### Create new branch and PR 

Git branches are essentialy pointer to a snapshot of your changes. 
If you want to keep your master clean (advisable) and develop on new branches, you can use command `git branch --help` to inspect 
all available commands for branching. 

You can create new branch with command: 
```
git checkout -b <branch_name>
```
e.g.
```
git checkout -b devel
```

All your changes since last commit are now part of newly created branch with `<branch_name>` 

If you commit your changes, you can push it to git repository (including new branch) with following command: 
```
git push -u origin <branch_name>
```
e.g.
```
git push -u origin devel 
```

After adding new branch, you can merge those changes by creating pull request. 

