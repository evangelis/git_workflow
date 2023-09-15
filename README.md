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
#1 Start the project manually setup remote name & url, create 2 files & 
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
