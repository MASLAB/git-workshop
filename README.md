# Git for MASLAB

## Getting Started

### Installing Git

#### Mac 

Git should already be installed.

#### Linux (including Ubuntu on your NUC)

Run the following commands:

```
sudo apt update
sudo apt install git
```

#### Windows

Go [here](https://git-scm.com/downloads) and select "Windows" as a download option. Make sure to select "Git Bash" when running the installer. 

### Setting up SSH Keys on MIT Github

In order to read or write to repositorites on the MIT Github Enterprise site, you need to use SSH keys. Using a username and password does not work with MIT Github, even if you may have used that method in the past for projects on github.com. 

#### Personal Device

Follow the directions [here](https://help.github.com/en/enterprise/2.18/user/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account) to add a new SSH Key to your MIT Github account at https://github.mit.edu. 

#### NUC

**Note:** this will already be done for you on the NUC that comes in your kit.

 In order to get read/write access to your repository from the NUC, you can add a repository-specific "Deploy Key." This lets you access a repository from the device without having to add a key for your NUC on any individual team member's account. 

To do so, follow the instructions [here](https://developer.github.com/v3/guides/managing-deploy-keys/#deploy-keys). In these instructions, "server" refers to the NUC. Your team repository should have been created for you, and can be found at `github.mit.edu/maslab-2020/team-<number>`. 

### Configuring Git

It's nice to configure the author settings for Git so that your commits are tagged appropriately.

#### Personal Device

```
git config --global user.name "<your name>"
git config --global user.email <your email>
```

#### NUC

**Note:** this will already be done for you on the NUC that comes in your kit, although you are free to use this command to change the user name if you'd like (we defaulted to "Team X")

```
git config --global user.name "<team name>"
git config --global user.email maslab-2020-team-<number>@mit.edu
```

## Git Basics Walkthrough

#### Clone a repository

In a terminal or Git Bash, navigate to a directory where you want to download the example repo. Run `git clone git@github.mit.edu:maslab-2020/git-workshop-team-<number>`. This should download a fully-initialized Git *repository*, stored in a directory called `git-workshop-team-<number>`. Navigate into this directory and run `git status`. You should see output like this:

```
On branch master
nothing to commit, working tree clean
```

This means you haven't made any new changes to your repository. 

Now run `git log` to view a record of previous "commits" or changes to the repository. 

You should see output like this (the details may vary, but the overall structure should look similar): 

```
commit a0e96dbadcb8e7f4c578a327377f28bcdd9e799e (HEAD -> master)
Author: MASLAB Staff <maslab-2020-staff@mit.edu>
Date:   Fri Jan 3 17:19:36 2020 -0500

    Initial commit
```

This is showing a record of the single commit that staff made to set the repository up for you. Once you make more commits, this log will grow to include those as well. 

#### Add a file

Now, we're going to try adding a new file to this repository. The repository is just a normal directory on your computer, and you can manipulate files in this directory like you would any other files. You can easily add an empty file directly from your command line by running `touch <filename>`. Everyone on your team should try this, but make sure everyone has a distinct filename (for example, your kerberos). 

Now, run `git status`. You should see that the directory has indeed changed: 

```
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	maslab

nothing added to commit but untracked files present (use "git add" to track)
```

You can now edit this file in any text editor of your choice (Sublime Text, Vim, Nano, etc). Try making a few edits to the file, and then save it.

Next, we want to add this file to your repository. The first step is to run `git add <filename>` to *stage* the commit. This doesn't mean we've saved the changes to the repository yet, but rather we're preparing to do so. Although this might seem like an annoying extra step, it's useful if you want to split up a bunch of changes to a file into multiple commits (you can do this interactively via `git add -p`). 

The output of `git status` should again be different now, and look something like this:

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   maslab
```

In order to actually record the change you've made to the repository, you need to *commit* it. Do this by running `git commit -m "Your message here"`, where the message (ideally) contains some useful information about your change. 

Now that you've commited the change, your repository should be "clean." If you run `git status` again, you'll get output like what you saw initially. However, if you run `git log`, you should see an additional commit listed with the message you just used!

#### Push and pull changes

Now that you've commited a change, it's time to share it with your teammates! The process of writing a commit to a remote repository (like a repository on Github) is called *pushing*, and we're going to try it now.

For this step, it's important to follow along with *one teammate at a time* so everything goes smoothly. Pick one member to push first. All they have to do is run `git push origin master`, and their changes should be uploaded to Github! 

Now, if you visit your repository on Github, you should see the additional file you added, as well as a record of the commit you made! Try exploring the interface to see the different ways Github shows you all this information. 



Have a second member of your team try to push to the repository (once again, `git push origin master`). This time, you should get an error! It'll look something like this:

```
To github.com:maslab-2020/git-workshop.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:maslab-2020/git-workshop.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

This error means that someone else has already pushed to the repository, and your changes are out of date. You are not able to push to a repository unless your changes start at the latest commit to that repository. 

Luckily, solving this problem is generally pretty easy. All you have to do is *pull* the latest changes from Git, updating your repository to include newer changes your teammates may have made. In order to do so, run `git pull origin master`.  

After downloading the most recent changes, Git will perform a *merge* to combine your changes with the new changes from your teammates. If you haven't modified any of the same code, it should do this entirely automatically. However, in this process Git will add a new commit to your repository, called a "merge commit," and you will have the opportunity to edit this commit's message. At some point during the process Git will pop up a text editor, and you can simply save and close to use the default message (which is usually fine). 

**Note:** Your Git installation may default to using Vim, which can be tricky to get out of if you haven't seen it before. However, you can save and exit by pressing escape followed by typing `:wq <Enter>`. 

Once that's resolved, you should now be able to push those changes using `git push origin master`. Try it, make sure the push succeeds on your terminal, and go take a look at the Github repository page to make sure both partner's changes are there. 

#### Resolve a conflict

Sometimes when you pull new code, Git won't be able to automatically merge it for us like it did in the last step. We were fine just now because each partner was editing a unique file, and Git will even be able to automatically merge changes to *different parts* of the same file. However, automatic merging will fail whenever two people edit the same line of the same file, and this will generate what's called a "merge conflict" when you try to pull. 

While merge conflicts can often be avoided by communicating with your teammates, you're bound to run into one at some point, and therefore it's important to know how to resolve them. To set up a merge conflict, have each of your teammates make a file called `conflict.txt`. Then, have each member edit that file and make the first line their kerberos. Now, add and commit this change, and have one member push their commit to the repository.

The first push should go fine of course, but notice what happens when the other member pulls. They should see an error like this at the bottom of the output:

```
Auto-merging conflict
CONFLICT (add/add): Merge conflict in conflict.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Running `git status` will show a new sort of output:

```
On branch master
Your branch and 'origin/master' have diverged,
and have 3 and 1 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both added:      conflict.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

When a conflict occurs, Git modifies the file in question by inserting funny-looking markers like `<<<<<<<`, `=======`, and `>>>>>>>` to show the different possible values each conflicting line could have. In order to fix the conflict, you have to edit the file to get rid of these markers and select the correct value for each conflicting line. For example, if you open `conflict.txt`, you might see something like this: 

```
<<<<<<< HEAD
maslab2
=======
maslab
>>>>>>> 277e16694781dd60a65e637dc49f8edc98a26b46
```

`maslab2` is the value of my own change (as the user who's currently trying to pull), and `maslab` is the value of the line on the remote repository. In order to resolve this commit, I have to pick one of these two values to include (or just put in a different third value altogether), and delete the markers. Let's say I decide to keep both, with a comma between them. After resolving the conflict, my file should just look like normal: 

```
maslab, maslab2
```

In order to register the conflict as resolved, add your changes and commit them. Finally, you can push your changes to the repository!



That concludes our quick walkthrough and should be enough to get you started using Git to manage your files. However, GIt has many more features than just the ones gone over here, and these can help you create more elaborate workflows to better manage your code. See the references section below to learn more about Git, and happy coding!

## References

- Official Git documentation: https://git-scm.com/docs
  - Probably a bit technical for a beginner, but good to have available
- Hacker Tools IAP class notes: https://hacker-tools.github.io/version-control/ 
  - More in-depth explanation of how Git works (still aimed at beginners), including lecture video
- 6.08 Git walkthrough: https://iesc.io/608/S19/lab08a
  - A similar but slightly more detailed Git walkthrough

## Cheatsheet

| Task                                     | Command(s)                                                  |
| ---------------------------------------- | ----------------------------------------------------------- |
| Add and commit all changes               | `git add --all` <br />`git commit -m "message"`             |
| Push changes to Github                   | `git push origin master`                                    |
| Pull changes from Github                 | `git pull origin master`                                    |
| Check current status of repository       | `git status`                                                |
| View log of previous commits             | `git log`                                                   |
| Start a new repository from scratch      | `git init` (in the directory you want to make a repository) |
| Clone an existing repository from Github | `git clone <url>`                                           |
