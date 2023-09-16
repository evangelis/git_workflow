This repository will show you a basic git workflow for individuals or small teams
The basic steps in this flow are as follows:

1.Create a new branch from the develop branch and call it something like feature-<describe feature here, or give it an ID>.
2.Work on your feature, committing to this feature branch
3.Test your feature
4.Merge your feature into the develop branch
5.Delete your feature branch
6.Once enough features have been added, prepare your release
7.When the release is tested and prepped, merge the develop branch into master
8.Tag the master branch commit to the correct version (i.e. v1.1)
Repeat the above steps
Note that we may need to set up the remote url for pushing to the remote repo
So let's examine step by step the tutorial 
###(I)1. Start the project manually setup remote name & url, create 2 files & 
  #perform Edit/Stage/Commit
git init 
git remote add origin https://github.com/evangelis/git_workflow
git remote set-url origin origin https://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@github.com/evangelis/git_workflow
git remote -v 
touch index.html README.md
echo "<html><head><title>Simple Webpage</title></head><body><h1>Hello World</h1></body></html>" > index.html
#Start editing README file
git add README.md index.html
git commit -m "First Commit"
git push origin master
###2. Add a License to the repo ->Stage/Commit/Push upstream to our remote repo
touch LICENSE
################################################################################################
echo "Copyright 2019 Zach Gollwitzer

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE." >LICENSE
################################################################################################
git add LICENSE
git commit -m "Add license to project"
git push origin master
###3.Tagging the 1st production release

git tag -a v1.0 -m "Added first tag"
git show v1.0
git push --tags origin master
###4.Work on branches :Work on develop [add css to index.html] Stage/Commit/Push to a remote     #develop branch-->The master branch is 1 commit behind 
git branch develop 
git branch --list
git log --pretty=oneline #2 commits 
git checkout develop 
#work on index.html :Edit/Stage/Commit/Push
#######################################################################
echo "<html>
  <head>
    <title>Simple Webpage</title>
    <style>
      body {
        font-family: monospace;
        color: navy;
        padding: 40px;
      }

      .header {
        font-weight: 500;
      }
    </style>
  </head>
  <body>
    <h1 class="header">Hello World</h1>
    <br />
    <p>Welcome to the Git Tutorial</p>
  </body>
</html> " >>index.html
###############################################################################
git add index.html
git commit -m "Add CSS to HTML"
git push origin develop
###4.Continue work on develop branch :Modify index.html &create anew file style.css
#Edit index.html locally
echo "<html>
  <head>
    <title>Simple Webpage</title>
    <link rel="stylesheet" href="./style.css" type="text/css" />
  </head>
  <body>
    <h1 class="header">Hello World</h1>
    <br />
    <p>Welcome to the Git Tutorial</p>
  </body>
</html>" >> index.html
################################################################################
touch style.css
#####################################
echo "body {
  font-family: monospace;
  color: navy;
  padding: 40px;
}

.header {
  font-weight: 500;
}" >style.css
#####################################
git add index.html style.css 
git commit -m "Split HTML & CSS into 2 files"
git push origin develop
###5.Create a feature branch 'feat1' from the develop 
git checkout -b feat1
#Edit index.html , create a script.js & update CSS file ->Push to origin/feat1
###################################################################
echo "<html>
  <head>
    <title>Simple Webpage</title>
    <link rel="stylesheet" href="./style.css" type="text/css" />
  </head>
  <body>
    <h1 class="header hidden" id="header-id">Hello World</h1>
    <br />
    <p>Welcome to the Git Tutorial</p>
    <script src="./script.js"></script>
  </body>
</html>" >>index.html
########################################################################
touch script.js
echo "/ Wait for window to load
document.addEventListener("DOMContentLoaded", function (event) {
  // Get reference to header object
  let myHeader = document.getElementById("header-id");

  // Wait 3 seconds, then display the header
  setTimeout(() => {
    myHeader.classList.remove("hidden");
  }, 3000);
});" > script.js
echo \
"body {
  font-family: monospace;
  color: navy;
  padding: 40px;
}

.header {
  font-weight: 500;
}

.hidden {
  display: none;
}" >> style.css
git add index.html style.css script.js
git status #On branch feat1 .Modified:{index.html,style.css}, new file : script.js
git commit -m "Add javascript to code"
git push origin feat1

###6.Add a .gitignore to develop branch & a todo-list text file
git checkout develop
touch .gitignore todo-list.txt
echo "todo-list.txt" > .gitignore
git add .
git commit -m "Add .gitignore file while on develop branch"
git push origin develop
###7.Merge branches 
#(i) Merge feat1 to develop [divergent histories ,no conflict] ,delete feat1
# develop :{.gitignore ,no js},feat1:{no .gitignore,script.js present}
git branch
git log --oneline --decorate --graph --all
git merge feat1 
git branch --merged
git branch -d feat1
git log --oneline --decorate --graph --all

#(ii) Merge develop into master branch & tag latest commit on master
git checkout master
git merge develop 
git tag -a v1.1 -m "Added second release tag"
###Next continue work on develop braanch for the next software release
###(II) Merge Conflicts 
##################################################################################################
# Happens when trying to combine (merge) >=2 snapshots into a single commit but there is 
#  a point in each snapshot that conflicts
##################################################################################################
# (1) Merge conflict between local & remote repo
# The upstream repo has been updated by >=1 team members & you try to pull changes to your 
# local repo
##(i) Create a testUserGit11 that will make a small change  to a new  file -->Commit changes 
touch conflict.txt 
git add .
git commit -m "Added conflict.txt file "
#(ii) testUserGit11 on Github edits this file: "Added some info to conflict.txt to create a merge conflict" =>Commit
#(iii)Edit the file localy & do a git pull
echo "A local edit that will conflict with the upstream repository." >> conflict.txt
git pull ## Conflict ,divergent file contents -->
# says that our local changes will be overwritten by the merge .There are 3 solutions :
#(a)reset our local repo :git reset --hard HEAD~1 
#(b)Commit our changes in local master &pull :git commit -m "msg"; git pull origin master -->Pull overwrites our local changes
#(c)git stash ;git pull origin master ->Store the conflicting local changes & get most recent version of the remote repo .Let's do the 3rd option
git stash
git pull origin master

#Suppose we decide to keep our local changes ,discarding contributor's edit
git stash apply stash@{0}
git stash list  ## stash@{0}: WIP on master: 2bb9989 Create intentional merge conflict
git add conflict.txt
git commit -m "Merge stashed changes banck into the text file"
git push origin master
##(2)Merge conflicts in local repo #Edit/Stage/Commit conflict.txt in a new branch & try merging into master
git checkout -b merge-conflict-branch 
echo "I made this change from the `merge-conflict-branch`" >> conflict.txt
git commit -a -m "Created another merge conflict from merge-conflict branch"
git checkout master
git add .
git commit -m "Create merge conflict"
git merge merge-conflict-branch ##CONFLICT in conflict.txt
#Use IntelliJ idea to fix it

################################################################################################
###8.Checkout,Revert,Reset,Restore,Rebase
git checkout master; touch three-trees.txt
git status #Untracked (unstaged) files: three-trees.txt
git add -A
git status ##Changes to be commited :new file :three-trees.txt ,uncommit:git reset HEAD <file> 
touch additional-file.txt
echo "Random text contents" > additional-file.txt
git status # {Untracked files: additional-file.txt, changes to be committed (new file):three-trees.txt}
git add additional-file.txt;
git commit -m "Create three trees tutorial"
git log --oneline --decaorate --graph --all 
git checkout develop
git log --oneline --decorate --graph
-------------------------------------------------------------------------------
#*   2f7765d (HEAD -> develop, tag: v1.1) Merge branch 'feat1' into develop
\
| * 69bdc19 (origin/feat1) Add javascript to code
* | 547a448 (origin/develop) Add .gitignore file
|/
#\

#* a4879ce Split HTML and CSS into two files
#* 682f2aa Add CSS to HTML
#* ccb5d8e (tag: v1.0) Add license to project
#* 7087a7e First commit 

------------------------------------------------------------------------------------
#(i)Checkout :Set the HEAD pointing at develop on 1st commit -->Detached HEAD state
git checkout 7087a7e#We can start a new branch & work from the begining
git checkout master
git log --oneline --decorate --graph --all
--------------------------------------------------------------------------------------
#* 09dda7b (HEAD -> master) Create three trees tutorial section
#*   fa61317 (origin/master) Resolved merge conflict from VSCode
#|\
#| * d13ee52 Create merge conflict
#* | 6598c74 Add new text to README to create merge conflict
#|/
#* e9d7818 Created another merge conflict from merge-conflict-branch
#* 01bc44a Merge stashed changes back into README
#* a589f47 Create intentional merge conflict
#* 2bb9989 Create intentional merge conflict
#*   2f7765d (tag: v1.1, develop) Merge branch 'feat1' into develop
#|\
#| * 69bdc19 (origin/feat1) Add javascript to code
#* | 547a448 (origin/develop) Add .gitignore file
#|/
#* a4879ce Split HTML and CSS into two files
#* 682f2aa Add CSS to HTML
#* ccb5d8e (tag: v1.0) Add license to project
#* 7087a7e First commit
------------------------------------------------------------------------------------------
#(ii)Revert to the previous commit (09dda7b) & then to the merge commit (2f7765d)
#git revert <commitId> #Creates a new commit that undoes changes introduced by previous commits
git checkout master
git show 09dda7b # git show <commitId>
git revert HEAD #HEAD represents the most recent commit #(three-trees.txt and additional #file.txt) no longer appear in WD
git log --oneline --decorate --graph --all
-------------------------------------------------------------------------------------------
#* 76ba921 (HEAD -> master) Revert "Create three trees tutorial section"
#* 09dda7b Create three trees tutorial section
#*   fa61317 (origin/master) Resolved merge conflict from VSCode
#|\
#| * d13ee52 Create merge conflict
#* | 6598c74 Add new text to README to create merge conflict
#|/
#* e9d7818 Created another merge conflict from merge-conflict-branch
#* 01bc44a Merge stashed changes back into README
#* a589f47 Create intentional merge conflict
#* 2bb9989 Create intentional merge conflict
#*   2f7765d (tag: v1.1, develop) Merge branch 'feat1' into develop
#|\
#| * 69bdc19 (origin/feat1) Add javascript to code
#* | 547a448 (origin/develop) Add .gitignore file
#|/
#* a4879ce Split HTML and CSS into two files
#* 682f2aa Add CSS to HTML
#* ccb5d8e (tag: v1.0) Add license to project
#* 7087a7e First commit
------------------------------------------------------------------------------------------------
git reset --hard HEAD~ #Delete changes from last commit & WD changes
git show 2f7765d

#(iii)Reset:
------------------------------------------------------------------------------------------------
#git reset --soft <commitSha> :Moves the curr branche's HEAD to commit <commitId>
#git reset --mixed <commitSha>:>> >> & unstages changes (changes are kept in WD)
#git reset --hard <commitId>:>> >> & unstages changes & discards changes in WD
------------------------------------------------------------------------------------------------
git reset --soft 2f7765d# HEAD points to the specified commit
#Let's commit all files again after v1.1 
git commit -m "Add all files since v1.1 release" 
git log --oneline --decorate --graph --all 
-------------------------------------------------------------------------------------------------
#* dc8e366 (HEAD -> master) Add all files since v1.1 release
#| *   fa61317 (origin/master) Resolved merge conflict from VSCode
#| |\
#| | * d13ee52 Create merge conflict
#| * | 6598c74 Add new text to README to create merge conflict
#| |/
#| * e9d7818 Created another merge conflict from merge-conflict-branch
#| * 01bc44a Merge stashed changes back into README
#| * a589f47 Create intentional merge conflict
#| * 2bb9989 Create intentional merge conflict
#|/
#*   2f7765d (tag: v1.1, develop) Merge branch 'feat1' into develop
#|\
#| * 69bdc19 (origin/feat1) Add javascript to code
#* | 547a448 (origin/develop) Add .gitignore file
#|/
#* a4879ce Split HTML and CSS into two files
#* 682f2aa Add CSS to HTML
#* ccb5d8e (tag: v1.0) Add license to project
#* 7087a7e First commit
#Notice that it added files directly after the v1.1 release 
--------------------------------------------------------------------------------------------------
git reset --mixed 2f7765d # Unstage (modified & new) files
git status #Changes not staged for commit :{conflict.txt,index.html,script.js,style.css}
            #Untracked files :{additional-file.txt,three-trees.txt}
git commit -a -m "Add all files since v1.1 release"	
git log --oneline --decorate --graph --all 
--------------------------------------------------------------------------------------------------
#* 942fac1 (HEAD -> master) Add all files since v1.1 release
#| *   fa61317 (origin/master) Resolved merge conflict from VSCode
#| |\
#| | * d13ee52 Create merge conflict
#| * | 6598c74 Add new text to README to create merge conflict
#| |/
#| * e9d7818 Created another merge conflict from merge-conflict-branch
#| * 01bc44a Merge stashed changes back into README
#| * a589f47 Create intentional merge conflict
#| * 2bb9989 Create intentional merge conflict
#|/
#*   2f7765d (tag: v1.1, develop) Merge branch 'feat1' into develop
#|\
#| * 69bdc19 (origin/feat1) Add javascript to code
#* | 547a448 (origin/develop) Add .gitignore file
#|/
#* a4879ce Split HTML and CSS into two files
#* 682f2aa Add CSS to HTML
#* ccb5d8e (tag: v1.0) Add license to project
#* 7087a7e First commit
--------------------------------------------------------------------------------------------------
#create afile to demonstrate git reset --hard <commitSha>
touch useless-file.txt
echo "useless data" >useless-file.txt
git commit -a -m "Make useless commit #1"

echo "more useless data" >> useless-file.txt
git commit -a -m "Make useless commit #2"

echo "and some more" >> useless-file.txt
git commit -a -m "Make useless commit #3"

echo "one more time" >> useless-file.txt
git commit -a -m "Make useless commit #4"
git log --oneline --graph --decorate --all 
---------------------------------------------------------------------------------------------------
#*b3a997c (HEAD -> master) Make useless commit #4
#* 38e7d0d Make useless commit #3
#* b7748e3 Make useless commit #2
#* e6752da Make useless commit #1
#* 942fac1 Add all files since v1.1 release --> (use git reset --hard 942fac1)
#| *   fa61317 (origin/master) Resolved merge conflict from VSCode
#| |\
#| | * d13ee52 Create merge conflict
#| * | 6598c74 Add new text to README to create merge conflict
#| |/
#| * e9d7818 Created another merge conflict from merge-conflict-branch
#| * 01bc44a Merge stashed changes back into README
#| * a589f47 Create intentional merge conflict
#| * 2bb9989 Create intentional merge conflict
#|/
#*   2f7765d (tag: v1.1, develop) Merge branch 'feat1' into develop
#|\
#| * 69bdc19 (origin/feat1) Add javascript to code
#* | 547a448 (origin/develop) Add .gitignore file
#|/
#* a4879ce Split HTML and CSS into two files
#* 682f2aa Add CSS to HTML
#* ccb5d8e (tag: v1.0) Add license to project
#* 7087a7e First commit
--------------------------------------------------------------------------------------------------
git reset --hard 942fac1 #return to commit 942fac1,useless-file.txt will disappear from WD
git pull origin master
git push origin master 
#or use directly git push --force origin master if we dont care,forcing push

##(iv) Restore:Restore or discard changes in WD or from staging area to our WD
#git restore :[-p,--patch,--staged,--source=<commit>,--worktree]:
#git restore -p #restore fromlatest commit 
#git restore additional-file.txt #restore from HEAD
#git restore --source=<942fac1> style.css 

##(v)Rebase Branch:Maintain linear history
#git rebase [-i] <mainBranch> #incorporate changes from curr branch to mainBranch [interactively]
git checkout master
git checkout -b feature master #Edit/Stage/Commit
git checkout -b hotfix master #Edit/Stage/Commit & merge into master
git touch hotfix.txt
echo "Work on a fix to be merged on master" >hotfix.txt
git checkout master 
git merge hotfix
git branch -b hotfix
git checkout feature 
touch feature.txt 
echo "Work on feature text file" >feature.txt
git commit -a -m "Work on feature branch "

echo "Continue work on feature file " >> feature.txt
git commit -a -m "More work on feature branch"

echo "Finalizing work on feature text file" >>feature.txt
git commit -a -m "Final editing of feature text file on feature file "
git log --oneline --graph --decorate --all
git rebase master
git checkout master
git merge feature
git branch -d feature
git push origin master




