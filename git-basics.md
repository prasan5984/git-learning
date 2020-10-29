# How git works?
Follow the below steps to understand about git.
## git local init
1. `mkdir basics`
2. `cd basics`
3. `git clone`
4. `tree .git` 
This will be the output
	```
	.git
	├── branches
	├── config
	├── description
	├── HEAD
	├── hooks
    .....
	│   └── exclude
	├── objects
	│   ├── info
	│   └── pack
	└── refs
	    ├── heads
	    └── tags
	```
	What is happening? git is actually saving all the meta files required to maintain your repository in your local machine. To the question **What is a repo?** - Put it simply, it is a collection of files and folders that are versioned (i.e., changes made to them are tracked)
5.  Run a few more commands and see what is happening to our local .git folder. The local `.git` folder is called as local repo as well.
      ```bash
      echo "hello git" > hellogit.txt
      git add hellogit.txt
      git commit -m"create file"
      ```
6. Below is `tree .git` output.
	```
		.git
	...
	├── logs
	│   ├── HEAD
	│   └── refs
	│       └── heads
	│           └── master
	├── objects
	│   ├── 30
	│   │   └── b257b175e2ad09ef0040cd488bb980fb9a4a69
	│   ├── 8d
	│   │   └── 0e41234f24b6da002d962a26c2495ea16a425f
	│   ├── de
	│   │   └── f4476eb3fb2a4ae140f2bd3ffca9c77fe4fa6f
	│   ├── info
	│   └── pack
	└── refs
	    ├── heads
	    │   └── master
	    └── tags
	15 directories, 23 files
	```
	Under objects you can see new files created. 1 of this file is our `hellogit.txt` file stored created in git's format in this directory. This is the reason git will be able to revert the file even if you delete it. Try it out!
	```bash
	rm hellogit.txt
	git checkout hellogit.txt
	```
## git remote tracking
A versioning system must allow multiple user to work concurrently on the same project.  For this apart from your local repo, the files needs to be stored in a common repository and mapped to your local repo. 
1. For our example. lets create remote repo also locally and map it.
	```bash
	mkdir -p ../remote-basics
	git init --bare ../remote-basics
	git remote add origin ../remote-basics/	
	git push --set-upstream origin master
	```
	'origin' is the remote repo name. A branch named master will be created in remote repo and will track changes in our local master branch.
2. Now, `tree .git`. 
    ```
	  ...
	├── logs
	│   ├── HEAD
	│   └── refs
	│       ├── heads
	│       │   └── master
	│       └── remotes
	│           └── origin
	│               └── master
	...
	└── refs
	    ├── heads
	    │   └── master
	    ├── remotes
	    │   └── origin
	    │       └── master
	    └── tags
    ```
    Notice how origin is created under refs and logs folder. 
    
    **What is the difference between `git fetch` and `git pull`?**
    fetch will download the latest to our local repo. Specifically, only the remote branch origin master will have the changes updated. Our local branch master will not be affected and our local files will be untouched as well. If you want your local files to be updated you need to merge to the remote branch using `git merge` command. `git pull` is a shortcut to execute both these commands together.
    Let's  experiment.
    ```bash
    git clone ../remote-basics ../local-basics-repo2
    cd ../local-basics-repo2
    echo "hi git" > ../local-basics-repo2/hellogit.txt
    git commit -am "say hi"
    git push
    cd -
    git fetch
    ```
    Run the commands to see the file content in local source and origin/master remote branch.
    ```bash
    cat hellogit.txt
    git show origin/master:hellogit.txt
    ```
   You can now merge using `git merge origin/master`
   
   **What is a local branch and what other branches are there?**
   In our previous example, `master` is a local branch. Another type of branch is remote branch `origin/master`.  'origin' is the remote repo name and master is the branch name.
