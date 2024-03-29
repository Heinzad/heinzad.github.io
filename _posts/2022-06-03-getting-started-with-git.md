Aide Memoire 

Getting Started With Git
======================== 
*How to use a feature branching workflow in Github*

This Git Hub pages site was set up thanks to Chad Balwin's [Building a Free Blog with GitHub Pages in Minutes](https://chadbaldwin.net/2021/03/14/how-to-build-a-sql-blog.html) (14 March 2021) instructions and template. 

After publishing the first post, the question was then how to make it more sophisticated by working in Visual Studio Code and using a branching strategy so that each post is a feature branch. 

We could visualise this with [Mermaid](https://mermaid-js.github.io/mermaid/#/) , if it worked :

````
```mermaid
gitGraph
       commit
       commit
       branch develop
       commit
       commit
       commit
       checkout main
       commit
       commit 
``` 
````

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
``` 

The remote returns a message on how to create a pull request for post2. 

pseudocode: 

```powershell 
cd "_posts" 
git add "2099-12-31_post2" 
git commit -m "getting started initial commit" 
git push --set-upstream origin post2 
``` 

Great! Now we have an error message from the remote saying "Your push would publish a private email address". 

### Email privacy settings 

We can resolve this thanks to Bryan Jimin Son's [How to resolve a GitHub error “push declined due to email privacy restrictions” when you try to push a change](https://bryantson.medium.com/how-to-resolve-a-github-error-push-declined-due-to-email-privacy-restrictions-when-you-try-to-b748f6ca0bcd). 

```powershell 
git config --global user.email <some number>+<your username>@users.noreply.github.com 
git commit --amend --reset-author
``` 

Second time lucky: 
pseudocode: 

```powershell 
cd "_posts" 
git add "2099-12-31_post2" 
git commit -m "getting started initial commit" 
git push --set-upstream origin post2 
``` 

After publishing the post we will have to start a pull request in the browser to get the commandline instructions: 

```powershell 
git pull origin develop
git checkout develop
git merge post2
git push -u origin develop
``` 

### Tagging 
Before we close out the feature branch it might be helpful to tag it in case we need to find something again. 

Atlassian documents [gis tag](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-tag). 

An annotated tag can carry a message that might prove useful in the future: 

```powershell 
git tag -a p2 -m "Post number two"
``` 

### Cleanup 
When we're done with the feature branch we may want to clean it out. 

Free Code Camp describes [How to Delete a Git Branch Both Locally and Remotely](https://www.freecodecamp.org/news/how-to-delete-a-git-branch-both-locally-and-remotely/). 

```powershell 
git branch -d post2 
git push origin --delete post2
``` 

QED

