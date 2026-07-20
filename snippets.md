# Snippets

## TOC

- [Snippets](#snippets)
  - [TOC](#toc)
  - [OS](#os)
    - [Linux](#linux)
      - [Essentials](#essentials)
      - [Do after every fresh install](#do-after-every-fresh-install)
      - [Structure](#structure)
      - [Users, groups, permissions](#users-groups-permissions)
      - [systemd](#systemd)
        - [systemctl](#systemctl)
        - [journalctl](#journalctl)
      - [CA certificates](#ca-certificates)
  - [Terminals](#terminals)
    - [POSIX](#posix)
    - [Linux](#linux-1)
      - [Bash](#bash)
      - [Azure cli](#azure-cli)
    - [Windows](#windows)
      - [PowerShell](#powershell)
      - [Cmd](#cmd)
  - [Git](#git)
    - [Glossary](#glossary)
    - [Essentials](#essentials-1)
    - [Misc](#misc)
    - [.gitignore](#gitignore)
    - [Git hooks](#git-hooks)
  - [Docker](#docker)
    - [Installation](#installation)
    - [Commands](#commands)
    - [Dockerfile](#dockerfile)
  - [Networking](#networking)
    - [TCP, HTTP, REST API etc.](#tcp-http-rest-api-etc)
      - [Headers](#headers)
        - [Cache-Control](#cache-control)
        - [ETag](#etag)
        - [If-None-Match](#if-none-match)
      - [Swagger and OpenAPI](#swagger-and-openapi)
      - [Standard endpoints](#standard-endpoints)
  - [AI](#ai)
    - [Ollama](#ollama)
  - [Query languages](#query-languages)
    - [PromQL](#promql)
  - [Misc](#misc-1)
    - [OpenSSL](#openssl)
    - [LDAP](#ldap)

## OS

### Linux

#### Essentials

```shell
# Update all packages
sudo apt-get update

# Upgrade all packages
sudo apt-get upgrade

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

#### Structure

/dev - Devices (like keyboard or mouse) files
/etc - Configuration files
/home - User personal files, configs etc.
/opt - External big apps, that don't follow unix conventions
/proc
/usr - Apps installed via package manager defined here (lib + bin)
/local - Apps installed manually
/var - Things that change frequently like logs

#### Users, groups, permissions

```bash
sudo adduser alice            # Create a new user 'alice'
sudo deluser alice            # Delete user 'alice'
sudo passwd alice             # Change password for 'alice'
su - alice                    # Switch to user 'alice'

sudo addgroup developers      # Create a new group 'developers'
sudo usermod -aG developers $USER  # Add current user to 'developers' group
sudo deluser alice developers # Remove 'alice' from 'developers' group
groups                        # List groups for current user

sudo chown alice file.txt     # Change owner of file.txt to 'alice'
sudo chgrp developers file.txt # Change group of file.txt to 'developers'
chmod 640 file.txt            # Set permissions: rw- r-- --- on file.txt
chmod -R 755 /path/to/dir     # Recursively set permissions rwxr-xr-x on directory

whoami                        # Show current user
cat /etc/passwd               # Show all users
cat /etc/group                # Show all groups
ls -l file.txt                # Show permissions and details for file.txt
```

#### systemd

Default init system and service manager.

##### systemctl

CLI to control systemd

With systemctl, you can do commands: status, start, restart, stop, enable, disable, list-unit-files

    systemctl --user restart pulseaudio

To show definition e.g.

    systemctl cat --user pulseaudio.service

To edit service, edit file content on

System unit files: Usually in /lib/systemd/system/ or /usr/lib/systemd/system/
User unit files: ~/.config/systemd/user/
Custom overrides: /etc/systemd/system/ (system-wide) or ~/.config/systemd/user/ (user)

Then

    systemctl --user daemon-reload

Beware, that system only restart system and user only user.

##### journalctl

journalctl is the tool to view logs collected by systemd’s journal service. It provides access to all system logs, including boot, kernel, service, and application logs.

```bash
journalctl -b (logs from current boot)
journalctl -u ssh (logs for ssh named service)
journalctl -p err (only error logs)
```

#### CA certificates

If you want to add new CA Certs, you paste those to `/usr/local/share/ca-certificates/` you can use new folders there. Write one cert per file, not bundle of many certs in one file.

Run `sudo update-ca-certificates`

There is `/etc/ssl/certs/ca-certificates.crt` containing all certs generated. You can use it with env vars like

- NODE_EXTRA_CA_CERTS
- REQUESTS_CA_BUNDLE
- SSL_CERT_FILE


## Terminals

### POSIX

```bash
if [ "$VAR" = "develop" ]; then
    TARGET="development"
elif [ "$VAR" = "main" ]; then
    TARGET="production"
else
    echo "Only main and develop branches can be released"
    exit 1
fi
```

**DNS**

    nslookup url.com

### Linux

```bash
ls  # Print files and dirs from current folder
ls -a # Also prints hidden files

grep # Search text for text occurrence. -i for case insensitivity, -r for recursive in dirs

tree  # Prints also nested dirs and folders in tree structure

env  # List environment variables

# Checks if variable is defined
if [ -z "$VAR" ]; then
    echo "Variable VAR not set."
    exit 1
fi

set -a; source /etc/environment; set +a;  # Update values from /etc/environment without restart

which python  # Print where command binaries are located

cat  # Print file content to console

# Searching, find and replace, insertion or deletion without opening file
# The below simple sed command replaces the word “unix” with “linux” in the file.

du -h -t 1000000 -d 1 /home/mda2bj/venvs/3.10/lib/python3.10/site-packages/ | sort -h  # Show directory sizes sorted and thresholded
sed 's/unix/linux/' geekfile.txt
```

#### Bash

Redirect output from stdout to file

cat test > test1 # Overwrite content if file exists
cat test >> test1 # Append to file

Hide error output

    2>/dev/null

Suppress both output and errors (shorthand)

    &>/dev/null

Restart terminal

    bash --login

Execute file

    . path

Bash Parameter Expansion - Removing Prefix

`##` means, that last symbol will be removed
`*:` is the pattern to match - it means "any characters (\*) followed by a colon (:)"

    USER="${FULL_USER##*:}"

Get DNS server address

    nmcli dev show | grep 'IP4.DNS'

Get custom IP addresses for existing interfaces

    ifconfig -a

Resolve IP address of domain name

    # Do not include protocol
    dig @<DNS_server_IP_address> google.com +short

Testing networking

    curl https://webpage.com

If DNS is not available

    curl --resolve webpage.com:443:1.2.3.4 https://webpage.com/path/ -I

#### Azure cli

Get access token

    az account get-access-token

If having a problem with incorrect audience, set receiver URL in resource

    az account get-access-token --resource https://graph.microsoft.com

### Windows

Open a bat script and keep output visible after it finishes
 
    cmd /k my_script.bat

**netstat**
count number of localhost connections

    netstat -n | findstr 127.0.0.1 | find /v /c ""

#### PowerShell

**Filter strings**

echo $Env:PATH | Select-String substring

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
    git add -A

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
    git reset --soft HEAD~5

    # Reset to commit and delete changes (destructive)
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

    # Get current branch name
    git rev-parse --abbrev-ref HEAD

```

### Misc

**Delete all branches with no upstream**

TODO test if fresh active branch is not deleted and if only local ones are dropped

    git branch -vv | grep ': gone]' | awk '{print $1}' | xargs git branch -D

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

**How to contribute on GitHub**

Create fork in GitHub, clone forked repository, add upstream with

    git remote add upstream original-repo-url

Update upstream repo in your IDE, create new branch, push changes to the fork, then create pull request in
GitHub, resolve comments and if it's accepted, delete branch from local repo.

**Delete last commit**

    # Warning: destructive and rewrites history
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

        -f      Point to dockerfile if not in current folder
        -t      Add tag
        -o      Output destination

**Run**

    docker run -d -p 80:80 docker/getting-started

        -d              Run in the background
        -p 80:80        Map port 80 of the host to port 80 in the container
        -i              Keep STDIN open even if not attached
        --expose        Expose a port or a range of ports
        -h              Container host name

**Download image**

    docker pull alpine

**Misc**

```shell
# List all your containers
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
# If you open http://localhost in browser, you see nginx welcome

# Stop container
docker container stop webserver

# Copy to container
docker cp source/path IMAGE_ID:/destination/path
```

### Dockerfile

```dockerfile
# This is comment ignored by docker.  # Inline comment also possible

# Starting container <image>[:tag]
FROM alpine:3.4

# Combine RUNs into one
RUN apk update && \
    apk add curl && \
    apk add vim && \

ENV PG_MAJOR=9.3  # Add environment variable
EXPOSE 80/tcp # Allow to access this port
CMD - Run this script after docker run
ENTRYPOINT - Similar to CMD - run this script after docker run - If user should not change the script (arguments)

 # (Preferred) Add directories/files to your image
COPY . .

# Add directories/files to your image
ADD
```

## Networking

### TCP, HTTP, REST API etc.

#### Headers

##### Cache-Control

- `public, max-age=31536000, immutable`
- `public, max-age=60, stale-while-revalidate=300`
- `private, max-age=300`
- `no-cache`
- `no-store`

##### ETag

Entity tag returned in response to represent a specific version of a resource.

##### If-None-Match

Request header containing a previously received ETag.  
If unchanged, server returns `304 Not Modified`; otherwise returns `2xx` with new content.

#### Swagger and OpenAPI

OpenAPI is specification and document format (JSON and YAML) that describes HTTP API

Swagger UI is web app that reads an OpenAPI document and renders interactive docs with "Try it out"

Open OpenAPI Swagger file

    docker run --rm -p 8080:8080 -e OAUTH_USE_PKCE=true -e 'SWAGGER_JSON=/spec.json' -v <PATH_TO_SPEC>:/spec.json swaggerapi/swagger-ui

You can also add -e OAUTH_CLIENT_ID=XXX to prefill client_id. Beware, that for Auth code flow you need to keep client_secret empty!

Name conventions:

For version 2: api_name.swagger.json
For version 3: api_name.openapi.json

#### Standard endpoints

Sometimes security definition is under

    <BASE_URL>/.well-known/openid-configuration

## AI

### Ollama

```bash
ollama-start

ollama pull phi3.5

ollama serve &
```

The api has two parts

- OpenAI compatible endpoints like
  http://localhost:11434/v1/completions
  http://localhost:11434/v1/chat/completions
  http://localhost:11434/v1/models
- Custom ollama API on http://localhost:11434/api/...

```python
import os

os.environ["OLLAMA_ORIGINS"] = "*"

from openai import OpenAI


# Point to local Ollama
client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama",  # Dummy key, not used
)

response = client.chat.completions.create(
    model="phi3.5", messages=[{"role": "user", "content": "What is the size of France in square miles?"}]
)

print(f"Response output: {response.choices[0].message.content}")
```

## Query languages

### PromQL

Get CPU usage for all nodes:

    node_cpu_seconds_total

Average CPU usage over 5 minutes:

    avg(rate(node_cpu_seconds_total[5m]))

## Misc

### OpenSSL

Show SSL certificate used for https page

    openssl s_client -showcerts -connect <git_url>:443 </dev/null 2>/dev/null |openssl x509 -outform PEM > gitlab.crt

Shows all chain of trust (all necessary CAs)

    openssl s_client -connect teams.microsoft.com:443 -proxy localhost:3128 -servername teams.microsoft.com -showcerts </dev/null 2>&1 | tee /tmp/teams_tls.txt

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
