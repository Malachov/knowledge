# Documentation

This repository contains snippets used across our portfolio

The format for snippets is markdown to be able to easily edit and also to have nice readable format.

`README.md` contain helpers with something specific across all the projects, it is language agnostic.

There are also language specific references - helpers for particular languages.

When you are contributing, there are two possible ways. For adding new snippets, put it directly to main. If
you want to
change the structure, create feature branch and merge request to main. If you edit some headings, do not
forget to
update table of contents in JetBrains - just show Quick fixes and do Update table...

## TOC

<!-- TOC -->

* [Documentation](#documentation)
    * [TOC](#toc)
    * [OS](#os)
        * [Linux](#linux)
            * [Essentials](#essentials)
            * [Do after every fresh install](#do-after-every-fresh-install)
    * [Terminals](#terminals)
        * [Linux](#linux-1)
            * [Bash](#bash)
        * [Windows](#windows)
            * [PowerShell](#powershell)
            * [Cmd](#cmd)
    * [Git](#git)
        * [Glossary](#glossary)
        * [Essentials](#essentials-1)
        * [Misc](#misc)
        * [.gitignore](#gitignore)
        * [Git hooks](#git-hooks)
    * [Docker](#docker)
        * [Installation](#installation)
        * [Commands](#commands)
        * [DOCKERFILE](#dockerfile)
    * [Misc](#misc-1)
        * [LDAP](#ldap)

<!-- TOC -->

## OS

### Linux

#### Essentials

```shell
# Update all packages
sudo apt-get update

# Upgrade all packages
sudo apt-get update

# Install package
sudo apt-get install python3.5-dev
```

Run on bash startup - `~/.bashrc`
Run on every startup - `~/.profile`

TODO add folders description, fstab description etc.

#### Do after every fresh install

```shell
# Install all building compilers (g++ error etc...)
sudo apt-get install build-essential
```

## Terminals

### Linux

```bash
ls  # Print files and dirs from current folder 
ls -a # Also prints hidden files

grep # Search text for text occurence. -i for case insensitivity, -r for recursive in dirs

tree  # Prints also nested dirs and folders in tree structure

env  # List environment variables

set -a; source /etc/environment; set +a;  # Update values from /etc/environment

which python  # Print where command binaries are located

cat  # Print file content to console

# Searching, find and replace, insertion or deletion without opening file
# The below simple sed command replaces the word “unix” with “linux” in the file.

du -h -t 1000000 -d 1 /home/mda2bj/venvs/3.10/lib/python3.10/site-packages/ | sort -h  # Show directory sizes sorted and thresholded
sed 's/unix/linux/' geekfile.txt
```

#### Bash

Redirect output from stdout to file

	cat test > test1  # Overwrite content if file exists
	cat test >> test1  # Append to file

Restart terminal

    bash --login

Execute file

    . path

Get DNS server address

    nmcli dev show | grep 'IP4.DNS'

Resolve IP address of domain name

    dig @<DNS_server_IP_address> google.com +short

#### Azure cli

Get access token

    az account get-access-token

### Windows

Open bat script and see result in terminal (keeping output after finishing script)

    cmd /k my_script.bat

**netstat**
count number of localhost connections

    netstat -n | findstr 127.0.0.1 | find /v /c ""

#### PowerShell

**Filter strings**

	echo $Env:PATH | Select-String  substring

Inverse filter

    echo $Env:PATH | Select-String substring -NotMatch

**Print content**

	Get-Content file

**Print env var**

	echo $Env:PATH

#### Cmd

Where command binaries are located

    where python

## Git

### Glossary

- origin - Remote url shorthand
- merge - Add changes from some branch into another in new commit
- rebase - Add changes from some branch via changing history (no new commit) !!!Use only when only you are
  using the branch. If already on remote, need force push!!!
- fast-forward - It's possible to do merge without new commit (only possible if there are no changes on the
  other branch)

### Essentials

```bash
    # Create .git folder
    git init

    # Stage all
    git stage -A

    # Commit all
    git commit -a -m "new message"

    git remote add origin www.github...repo-path

  # Download existing repo

    git clone repo-url

    # Configuration

    git config --global user.name "Your name"
    git config --global user.email "yourname@provider.com"

    # Print what is changed
    git status

    # Stage
    git add add.js

    # Reset to commit and keep changes in code in stage
    git reset --soft 852309

    # Reset 5 commits back
    git reset --soft 'HEAD{5}'

    # Reset to commit and delete changes
    git reset --hard 852309

    # Create branch
    git branch dev

    # Move to branch
    git checkout dev

    # Merge branches (if join master with dev branch, first checkout back to master then)
    git merge dev

    # Delete branch locally
    git branch -d dev

    # Delete branch remotely
    git push --delete origin dev
    
    # Change upstream (e.g. after branch renaming)
    git push --set-upstream origin "branch-name"

```

### Misc

**Clean all history (create, stage, commit, push)**

Delete `.git` folder and

```bash
git init
git add .
git commit -m "Removed history, due to sensitive data"
git remote add origin github.com:your-project/your-repo.git
git push -u --force origin master
```

**`.gitignore` seems to have no effect**

    git rm --cached spark-warehouse/ -r -f

**How to contribute on gitHub**

Create fork in GitHub, clone forked repository, add upstream with

    git remote add upstream original-repo-url

Update upstream repo in your IDE, create new branch, push changes to the fork, then create pull request in
GitHub, resolve comments and if it's accepted, delete branch from local repo.

**Delete last commit**

    git reset --hard <commit-id>
    git push origin -f

**Delete tag**

```bash
# Locally
git tag -d v1.0

# Remote
git push --delete origin tagname
```

### .gitignore

Create file called `.gitignore` on root. Only code is supposed to be on git. No binaries, libraries etc.
You can define what files will be sent to git and what files will be ignored

```gitignore
    # Comments must begin on new line !!!

    # Ignore one file
    readme.txt

    # Ignore only in current folder
    /readme.txt

    # Ignore folder
    output/

    # ignore all *java files
    *.java

    # All files in directory
    abc/**

    # Zero or more directories
    a/**/b

    # Do not ignore
    !readme.txt
```

### Git hooks

First setup hooks path, because normal hooks are gitignored by default

    git config core.hooksPath git_hooks_path

## Docker

### Installation

Install on linux [tutorial](https://docs.docker.com/engine/install/ubuntu/)
Install on windows [tutorial](https://docs.docker.com/docker-for-windows/install/)

### Commands

**Build**

    docker build -f /path/to/a/Dockerfile .

        -f - Point to dockerfile if not in current folder
        -t - Add tag
        -o - Output destination

**Run**

    docker run -d -p 80:80 docker/getting-started
    
        -d -  Run in the background
        -p 80:80 - Map port 80 of the host to port 80 in the container
        docker/getting-started - Image to use
    
        -i - Keep STDIN open even if not attached
        --expose - Expose a port or a range of ports
        -h - Container host name

**Download image**

    docker pull alpine

**Misc**

```shell
# List all your containrers
docker ps

# Remove container with it's id from docker ps
docker rm a39c259df46

# List all images
docker images

# List all running container
docker container ls

# Delete image
docker rmi 60959f29de3a

# Example - run linux
docker run --interactive --tty ubuntu bash
# Then you can use host, ls...

# Example 2 = nginx
docker run --detach --publish 80:80 --name webserver nginx
# If open http://localhost in browser, you see nginx welcome

# Stop container
docker container stop webserver

# Copy to container
docker cp source/path IMAGE_ID:/path/to
````

### DOCKERFILE

```dockerfile
    # This is comment ignored by docker.  # Inline comment also possible

    FROM - Starting container <image>[:tag]. E.g alpine:3.4,
    RUN - Run command such as apt-get install or pip install

    # Combine RUNs into one
        RUN apk update && \
            apk add curl && \
            apk add vim && \
    ENV PG_MAJOR=9.3  # Add environment variable
    EXPOSE 80/tcp # Allow to access this port
    CMD - Run this script after docker run
    ENTRYPOINT - Similar to CMD - run this script after docker run - If user should not change the script (arguments)

    COPY - (Preferred) Add directories/files to your image
    ADD - Add directories/files to your image
```

## Misc

### OpenSSL

Show SSL certificate used for https page

	openssl s_client -showcerts -connect <git_url>:443 </dev/null 2>/dev/null |openssl x509 -outform PEM > gitlab.crt


### LDAP

```python
from ldap3 import Server, Connection, SAFE_SYNC

server = Server('rb-gc-lb.bosch.com', port=3268)
conn = Connection(server, 'ntuser@bosch.com', 'XXX', client_strategy=SAFE_SYNC, auto_bind=True,
                  auto_referrals=False)

# In search function, there are two important positional params.
# First define space to return the results and second means filter, that is applied on searched results
# Find Group by CN
status, result, response, _ = conn.search('DC=bosch,DC=com', '(CN=Bj_PS_dux_streaming_c-schicht_editor)')

# Find user
status, result, response, _ = conn.search('DC=bosch,DC=com', '(sAMAccountName=psx6fe)')

# Find type of records e.g. user
status, result, response, _ = conn.search('DC=bosch,DC=com', '(Objectclass=User)')

# Using wildcard to get all the records
status, result, response, _ = conn.search('DC=bosch,DC=com', '(sAMAccountName=*)')
```
