Going to just jot down everything that’s happening since I got the RaspberryPi.

- Found a 5v5A charger
- Found a portable display
	Micro to miniHDMI adapter was an afterthought
- Set up ssh and connected
```
	drew@192.168.*.*** 
```
If you want to checkout setting up SSH, go for it. But it's not necessary if you're just using the terminal on your RaspberryPi
- Checked if git was installed
```
	Git –version
```
- Configured Git with username and email
```
Git config –global user.name dr3wpdraw3rs`
Git config –global user.email drewpdrawers9090@gmail.com
```
- Checked it
```
Git config –list
```
- Trying to figure out how to get this in my linux terminal and sync it to GitHub
	Gonna create a file in /Documents
```
Mkdir demo
```
Could have named this file anything but demo is fine.
Make sure this folder knows what it’s about to do, time to make it a git folder
```
Git init 
```
If you list the contents of the new git folder, you get nothing.
```
ls
```
	This command (LS, but lowercase) lists the content of the current folder
	You can add the argument “ls -la” and it will show the hidden contents
	Should see .git in there
- Now we need to prove we can make changes
	Nano is a text editor in Linux. Let’s use it to make a different README.md
	Type whatever you want in here, like it’s a Google Document. But go ahead and use some markdown if you’d like.
	README.md ends with .md, signifying that it’s a markdown file. Simple code, very similar to HTML, but ya know, simpler.
```
nano README.md
```

- Did the file save?
	You can use ls again to see if the README.md is in there.
	It is. So what?
- Let’s check the status of gi
```
Git status
```
	Because git is a cool tool for version control, you can see the one change you’ve made to the file directory

- Let’s add the README.md to my repo
	I created the https://github.com/Dr3wPdraw3rs/drewberrypi repo for this purpose. It’s where it will end up.
	But let’s make sure it’s ready to go there.
- Let’s make sure the README.md is “tracked”. Meaning it’s a change we’d like to commit. Commits are the basis for making changes in git/ on github
	We’ll run:
```
 git add README.md
```

- Honestly this is random but you should start looking at this stuff.

Learn more about git
[https://git-scm.com/docs/git](https://git-scm.com/docs/gi)
Learn more linux commands
[https://www.hostinger.com/tutorials/linux-commands](https://www.hostinger.com/tutorials/linux-commands) 
We’re going to commit this change to main and add a comment on the commit by using the -m flag
[https://git-scm.com/docs/git-commit](https://git-scm.com/docs/git-commit)

- Let’s get it on github
	Time to set the remote repository where we’re going to push these committed changes
	and we are going to use “git remote add origin” to do it

- Let’s check to see what’s going on first with git remote first to see how we can get our local file, up to GitHub
``
git remote -v
``

	Learn more here:
	[https://git-scm.com/docs/git-remote](https://git-scm.com/docs/git-remote)

	We’ve confirmed we’ll be pulling from the same  repo and pushing to that same repo when we make changes. Cool
- Let’s push
```
git push origin master
```
	Press enter and it’ll ask for you github username and password
	I got a weird failed authentication for the repo when trying to push
- Let’s troubleshoot

	Follow the link: [https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls](https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls) 

	Ahhh okay, it’s not my password.

	It wants me to “enter your personal access token”
- Let’s go down this rabbit hole
	I ended up here: [https://docs.github.com/en/github-cli/github-cli/quickstart](https://docs.github.com/en/github-cli/github-cli/quickstart)

- Let’s try authenticating with GitHub CLI


	It said I needed to install GitHub CLI, but let’s see if it’s already installed

- Try sudo apt upgrade gh
```
sudo apt upgrade gh
```
	Click yes when prompted.
	Looks like it was already there as a part of RaspberryPiOS.
	COOL
- What’s an apt?
	 It’s like a .exe or .pkg but for Linux
	CLI is command line interface, and you can manage your installs straight from here without navigating to websites and accidentally downloading malware.
	EVEN COOLER
- Back to authenticating
	I ended up getting through a bunch of “login screens” and arrow key options and allowing a lot of cookies. It never opened up a screen. So the “Authenticate Git CLI” should be done with that.
	Probably best to do this on my raspberry pi, and not through SSH. but I am undeterred. Let’s keep trying through SSH to authenticate GitHub
- I guess I can just generate a new token, instead of logging in through the browser

	Let’s make one
	[https://github.com/settings/tokens]([https://github.com/settings/tokens)

	Created a new one with full permissions ghp_mt********** (Censored for reasons)

- Maybe I can push it to the repo now

	Looks like it’s working, but what do I do here? Did it make it to my repo on github?
	It did!
- Let’s look at the documentation to see how I can merge the pull request from the CLI (command line interface)
	Alright, it looks like I should’ve actually pulled the origin before making any changes and went through another rabbit hole, learning how to use the CLI to move my second read me and change the branch I was on
	```
	git clone Dr3wpdraw3rs/drewberrypi
	```
	This created the drewberrypi folder in my demo folder
- If you ever need to update
```
git pull origin
```
	 when you’re in that folder.
	I moved the README.md into drewberry pi using the -force argument
- To make sure I had the correct readme file, I ran
```
Open README.md
```
	It had the right text.
	Q on the keyboard closes the file
- I deleted the other “Master” branch
	“Delete a branch with git branch -d <branch> . The -d option will delete the branch only if it has already been pushed and merged with the remote branch. Use -D instead if you want to force the branch to be deleted, even if it hasn't been pushed or merged yet. The branch is now deleted locally.”
- Time to git this on github
```
git add README.md
```
```
git commit
```
	Add the note

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
drew@drewberrypi:~/Documents/demo/drewberrypi $ git status
On branch main
Your branch is up to date with 'origin/main'.
```
	Sweet
- Let’s look at setting up a branch
[https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
	Let’s you stage work before committing to publishing in main
	git checkout -b BRANCH_NAME creates a new branch and checks out the new branch while git branch BRANCH_NAME creates a new branch but leaves you on the same branch.
	In other words git checkout -b BRANCH_NAME does the following for you.
	git branch BRANCH_NAME # create a new branch git switch BRANCH_NAME # then switch to the new branch

- Basically, git branch shows the available branches
- git checkout switches to the branches
- b let’s you do the branch command and checkout command at the same time
Example:

	git branch probably-added-something created a new branch, but I’m still working in the main branch. I don’t want my commits to go directly to main, and I don’t want to have to use two commands just to create a new branch and switch to it.
	I should’ve just done git checkout -b probably-adding-something
	From here, I can do git branch -d probably-adding-something and it will delete it
```
drew@drewberrypi:~/Documents/demo/drewberrypi $ git branch -d probably-adding-something 
Deleted branch probably-adding-something (was 82c143a).
```
	Note: You can press tab once you get the first few letters in whatever you’re trying to match and it will autocomplete.

Let’s see what horrendous mess happens if I want this whole doc to update the README.md on Dr3wpdraw3rs/drewberrypi
Alright, so this is just a test of adding some sort of README.md.

I'll probably be adding my thought process as I go about adding new projects here.

**Also, I think I deserve some bonus points because I edited the markdown and text ALL WITHIN NANO. hahaha. Probably going to be moving to VSCODE for my future endeavors whenever I'm uploading to GitHub for this awfule documentation. I'll clean this all up later haha.**
