Getting Started With Git
======================== 
*Aide Memoire* 

## Github feature branching workflow

This Git Hub pages site was set up thanks to Chad Balwin's [Building a Free Blog with GitHub Pages in Minutes](https://chadbaldwin.net/2021/03/14/how-to-build-a-sql-blog.html) (14 March 2021) instructions and template. 

After publishing the first post, the question was then how to make it more sophisticated by working in Visual Studio Code and using a branching strategy so that each post is a feature branch. 

The first step was easy as source control in VS Code had a big `Clone` button. How to do branching? 

Atlassian's Bitbucket tutorial on [Git Feature Branch Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow) 

### Start up 
Start by checking out the main branch (Atlassian): 

```powershell 
git checkout main
git fetch origin 
git reset --hard origin/main
``` 

### New branch
As this was a brand-new project I had to adapt the instructions to create a develop branch: 

```powershell 
git checkout -b develop 
git push -u origin develop
``` 

So far so good, but having adapted the instructions, there was now a question of how to create a feature branch off the develop branch. 

### New feature branch 
The answer was quite straightforward thanks to Renat Galyamov's [How to create a branch from develop branch in Git](https://renatello.com/create-branch-from-another-branch-in-git/#how-to-create-a-branch-from-develop-branch-in-git). 

```powershell 
git checkout -b post2 develop 
git push --set-upstream origin post2 
``` 

The remote returns a message on how to create a pull request for post2. 

