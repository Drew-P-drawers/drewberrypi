# Using Git on Raspberry Pi OS

If you were able to install Raspberry Pi OS on the Raspberry Pi 5, it should already have Git installed. Let's verify it.

---
**Resources**

Learn more about git
[https://git-scm.com/docs/git](https://git-scm.com/docs/git)

Learn more linux commands
[https://www.hostinger.com/tutorials/linux-commands](https://www.hostinger.com/tutorials/linux-commands) 

---

## Check for Git

First, checked if git was installed.

```
    Git –version
```
It should return the git version you are currently running.

---
**NOTE**

If you don't currently have git installed (Maybe you installed a lite version) then you can install it quite simply. `sudo apt install git`

---

## Configure Git

Here, I configured Git with my username and email

```
git config –global user.name dr3wpdraw3rs`
git config –global user.email drewpdrawers****@gmail.com
```

Then I checked to see if it applied the configuration.

```
git config –list
```

I definitely missed a step here, but it was quite late. But initially, I was trying to figure out how to get this in my linux terminal and sync it to GitHub.
My thought was to create a file in /Documents and name it demo

```
mkdir demo
```
Could have named this file anything but demo is fine. mkdir makes a new directory, or folder as we typically call it.
Make sure this folder knows what it’s about to do, time to make it a git folder

```
git init 
```

If you list the contents of the new git folder, you get nothing.
```
ls
```
---
**NOTE**

`ls` lists the content of the current folder
You can add the argument “ls -la” and it will show the hidden contents
Should see .git in there

---
    
Now we need to prove we can make changes

Nano is a text editor in Linux. Let’s use it to make a new README.md
Type whatever you want in here, like it’s a Google Document. But go ahead and use some markdown if you’d like.

---
**NOTE**
`README.md` ends with `.md`, signifying that it’s a markdown file. Simple code, very similar to HTML, but ya know, simpler.

---

```
nano README.md
```

I wondered, Did the file save?
You can use `ls` again to see if the README.md is in there.
It is. So what?

Let’s check the status of git again

```
git status
```

![](assets\images\git-status-1.png)
Because git is a cool tool for version control, you can see the one change you’ve made to the file directory

Let’s add the `README.md` to my repo. I created the [https://github.com/Dr3wPdraw3rs/drewberrypi](https://github.com/Dr3wPdraw3rs/drewberrypi) repo for this purpose. It’s where it will end up. But let’s make sure it’s ready to go there.
You don't really need to double check, but I did make sure the README.md is “tracked” for posterity. "tracked" means it’s a change to the directory we can commit. Commits are the basis for actually making changes in on github.

We’ll run:

```
 git add README.md
```

![](assets\images\git-add-2.png)

We’re going to commit this change to main and add a comment on the commit by using the -m flag
[https://git-scm.com/docs/git-commit](https://git-scm.com/docs/git-commit)

```
git commit -m "Just wanted to add a note"
```

![](assets\images\git-commit-3.png)

## Pushing changes to GitHub (or trying)

Started figuring out what I needed to get my new `README.md` on GitHub

I realized later, this was the step I missed in the beginning. I had created a folder on my local Linux machine, but never connected Dr3wpdraw3rs/drewberrypi repo I had made on GitHub earlier.

Wanted to tell my new `demo` file which remote repository we’re going to push these committed changes.

Going to use `git remote add origin` to do it.

```
git remote add origin https://github.com/Dr3wPdraw3rs/drewberrypi.git
```

![](assets\images\git-remote-4.png)

Let’s check to see what’s going on first with git remote first to see how we can get our local file, up to GitHub
``
git remote -v
``

![](assets\images\git-remote-5.png)

We’ve confirmed that adding the `origin` means we’ll be pulling from the same repo and pushing to that same repo when we make changes. 
Cool

Time to push the change up to GitHub... right?

```
git push origin master
```

I got a weird failed authentication for the repo when trying to push after following the on screen prompts...

![](assets\images\git-push-6.png)

## Authenticate gh (GitHub apt)

Looks like we're trouble shooting.

Started by following the link: [https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls](https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls) 

Ahhh okay, it’s not my password.

It wants me to “enter your personal access token”
Let’s go down this rabbit hole
I ended up here: [https://docs.github.com/en/github-cli/github-cli/quickstart](https://docs.github.com/en/github-cli/github-cli/quickstart)

Next, I wanted to try authenticating with GitHub CLI


The documentation, said I needed to install GitHub CLI, but let’s see if it’s already installed

Try sudo apt upgrade gh

```
sudo apt upgrade gh
```

Click yes when prompted. (Y)
Looks like it was already there as a part of RaspberryPiOS.
COOL

---
**NOTE**

`sudo apt upgrade <apt>` is great to check installations, and keep them updated, but `gh –version` would've been the best option here. Updates could take a while.

But... What’s an apt?

It’s like a .exe or .pkg but for Linux, and you can typically just run commands in the CLI to install them.
CLI is command line interface, and you can manage your installs straight from here without navigating to web pages or app stores.
EVEN COOLER

---

Back to authenticating

I ended up getting through a bunch of “login screens” and arrow key options and allowing a lot of cookies. It never opened up a screen. So the “Authenticate Git CLI” should be done with that.
Probably best to do this on my raspberry pi, and not through SSH. but I am undeterred. Let’s keep trying through SSH to authenticate GitHub

I guess I can just generate a new token, instead of logging in through the browser

![](assets/images/gh-auth-7.png)

Let’s make one: (**Follow these instructions silly**)
[https://github.com/settings/tokens]([https://github.com/settings/tokens)

![](assets\images\gh-auth-8.png)

Created a new one with full permissions ghp_mt********** (Censored for reasons)

Maybe I can push it to the repo now...

```
git push origin master
```

![](assets\images\git-push-9.png)

Looks like it’s working, but what do I do here? Did it make it to my repo on github?
It did! Kind of.

![](assets\images\git-repo-10.png)

Alright, it looks like I should’ve actually pulled the origin before making any changes and went through another rabbit hole, learning how to use the CLI to move my second `README.md` and change the branch I was on.. since I accidentally created the `master` branch when I ran `git push origin master` earlier. It really should've been `git push origin main`.

Anyways, time to fix my mistakes again.

```
git clone Dr3wpdraw3rs/drewberrypi
```
This created the drewberrypi folder in my /demo folder. It pulls the current repo from GitHub. 
If you ever need to update the local directory to match what's on GitHub:

```
git pull origin
```
I moved the README.md into drewberry pi using the -force argument to overwrite the previous README.md that I created when setting up the repo. Oops.

```
cd ..
mv README.md -force drewberrypi
```

`cd` changes the directory. I was initally at ~/Documents/demo/drewberrypi. `cd ..` moved me into ~/Documents/demo where I created the `README.md` that I actually wanted (And where I spent a bunch of time writing the first draft of this guide in the nano CLI text editor).

To make sure I had the correct readme file now, I ran
```
Open README.md
```
It had the right text that I added with nano. Nice.

Q on the keyboard closes the file

I deleted the other “Master” branch to remove my mistake.

“Delete a branch with git branch -d <branch> . The -d option will delete the branch only if it has already been pushed to remote branch. Use -D instead if you want to force the branch to be deleted, even if it hasn't been pushed or merged yet. The branch is now deleted locally.” - [Git docs](https://git-scm.com/docs/git-branch)

Time to "git" this on github and be sad about the extra steps.

```
git add README.md
```

```
git commit
```
Add the note so you know what changes you made, and why, at any point later. Or so that other people can understand the changes you made as well.

![](assets\images\git-add-11.png)

```
git status
```
Looked like this:

```
drew@drewberrypi:~/Documents/demo/drewberrypi $ git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)
nothing to commit, working tree clean
drew@drewberrypi:~/Documents/demo/drewberrypi $ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 403 bytes | 403.00 KiB/s, done.
Total 3 (delta 0), reused 2 (delta 0), pack-reused 0
To https://github.com/Dr3wPdraw3rs/drewberrypi/
   f6822ab..82c143a  main -> main
```
Sweet! That was a whole lot of dumb, but I think I learned what not to do. I definitely respect GitHub desktop and vscode a lot more after this.

## Using branches the right way

Let’s look at setting up a branch properly, since I want to do it the right way this time. (Branches are great for making staged changes, and not applying mistakes directly to the main page/project/repo on GitHub)

```
[https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
    Let’s you stage work before committing to publishing in main
    git checkout -b BRANCH_NAME creates a new branch and checks out the new branch while git branch BRANCH_NAME creates a new branch but leaves you on the same branch.
    In other words git checkout -b BRANCH_NAME does the following for you.
    git branch BRANCH_NAME # create a new branch git switch BRANCH_NAME # then switch to the new branch
```

Basically, `git branch` shows the available branches
`git checkout` switches to the branches
`- b` let’s you do the branch command and checkout command at the same time

Example:

![](assets\images\gut-branch-12.png)

`git branch probably-added-something` created a new branch, but I’m still working in the main branch. I don’t want my commits to go directly to main, and I don’t want to have to use two commands just to create a new branch and switch to it.
I should’ve just done `git checkout -b probably-adding-something`
From here, I can do `git branch -d probably-adding-something` and it will delete it

```
drew@drewberrypi:~/Documents/demo/drewberrypi $ git branch -d probably-adding-something 
Deleted branch probably-adding-something (was 82c143a).
```
---
**Note:** 

You can press tab once you get the first few letters in whatever you’re trying to match and it will autocomplete.

---

## Conclusion

So, there's some takeaways here. I probably shouldn't write a guide if I haven't figured it all out yet. But I figured this might be helpful for troubleshooting, since I was initially following other guides and there was a lot of context missing. Things that I wouldn't have known by just copy/pasting. This helped me identify the importance of certain commands, why they exist, and the explicit arguments that each command uses.

Hope it helps somehow.

Cheers,
Drew
