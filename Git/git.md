# Git Cheat Sheet

*Repository* : A repository is a directory that contains all of your project's files and stores each file's revision history. You can also think of a repository as a project's history.

*Branch* : A branch is a parallel version of a repository. It is contained within the repository, but does not affect the primary or master branch allowing you to work freely without disrupting the "live" version. -a pointer to a snapshot of your changes.-

*local repo* : A local repository is a Git repository that is stored on your computer. It is the repository that you have on your computer that you can use to work on your project.

*remote repo* : A remote repository is a Git repository that is stored on a server. It is the repository that you and your collaborators use to exchange your project history.

*commit* : A commit, or "revision", is an individual change to a file (or set of files). It's like when you save a file, except with Git, every time you save it creates a unique ID (a.k.a. the "SHA" or "hash") that allows you to keep record of what changes were made when and by who. Commits usually contain a commit message which is a brief description of what changes were made.

Commits can be thought of as snapshots or milestones along the timeline of a Git project.

*clone* : A clone is a copy of a repository that lives on your computer instead of on a website's server somewhere, or the act of making that copy. With your clone you can edit the files in your preferred editor and use Git to keep track of your changes without having to be online. You can push your local changes to the remote repository to keep them synced when you're online.

*push* : Pushing refers to sending your committed changes to a remote repository, such as a repository hosted on GitHub. For instance, if you change something locally, you'd want to then push those changes so that others may access them.

In other words, push is used to upload local repository content to a remote repository.

*pull* : Pulling refers to when you are fetching in changes and merging them. If you and another developer were both working on a file and both made changes, you'll want to pull in those changes and merge them into your branch.

*pull request* : Pull requests are proposed changes to a repository submitted by a user and accepted or rejected by a repository's collaborators. Pull requests show diffs, or differences, of the content from both branches. The changes, additions, and subtractions are shown in green and red.

Lets create a new repository on github and clone it to our local machine.

```bash
$ mkdir github-repositories
$ cd github-repositories
$ git clone https://github.com/taham8875/test-git.git
``` 

Now we have a local copy of the repository. Let change the directory to the repository and create new files.

```bash
$ cd test-git
$ touch test.txt index.html style.css main.js 
```

Now we have 4 files in our repository. Let's add them to the staging area.

```bash
$ git add .
```

Note that we have used the dot(.) to add all the files in the current directory to the staging area. You can add just one file by using the following command.

```bash
$ git add <file>
```

Now we have added all the files to the staging area. We can see the status of the repository by using the following command.

```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   index.html
        new file:   main.js
        new file:   style.css
        new file:   test.txt
```

to unstage a file use the following command.

```bash
$ git restore --staged <file>
```

or you can use the following command.

```bash
$ git reset head <file>
```

```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   main.js
        new file:   test.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)    
        index.html
        style.css
```

Now let's commit the changes.

```bash
$ git commit -m "initial commit"
```

lets add the remaining files to the staging area and commit them.

```bash
$ git add .
$ git commit -m "added index.html and style.css"
```


Now we have committed the changes. Let's push the changes to the remote repository.

```bash
$ git push
```

Now we are done. You can see the changes on the github repository.

---

Let create a new file `main.py` from the browser (to simulate that other collaborate changed something in the remote repo), it is now not in the local repo, to add it to the local repo we can use the following command.

```bash
$ git pull
```

## Git Configurations

We can list the configurations using the following command.

```bash
$ git config --list
```

We can set the user name and email address for the repository using the following commands.

```bash
$ git config --global user.name "Taha Ahmed"  
$ git config --global user.email "taha.m8875@gmail.com
```

to change the configurations via vscode use the following command.

```bash
$ git config --global --edit
```
## Generate SSH Key

SSH keys are used to authenticate users and provide secure access to your repositories. You can generate a new SSH key and add it to the ssh-agent.

```bash
$ ssh-keygen -t rsa -b 4096 -C "taha.m8875@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/Taha/.ssh/id_rsa): 
...
```

Now we can navigate to the `.ssh` directory and copy the contents of the `id_rsa.pub` file, than open github setting and add the key.

## Create repository from existing files

```bash
$ cd existing_folder
$ git init
$ git remote add origin git@github.com:taham8875/test-git.git
$ git add .
$ git commit -m "Initial commit"
