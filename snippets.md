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
    - [Linux terminal](#linux-terminal)
      - [Bash](#bash)
      - [Commands - LS, man, curl etc.](#commands---ls-man-curl-etc)
    - [Windows](#windows)
      - [PowerShell](#powershell)
      - [Cmd](#cmd)
  - [Cloud](#cloud)
    - [Azure](#azure)
      - [Azure CLI](#azure-cli)
    - [GCP - Google Cloud Platform](#gcp---google-cloud-platform)
      - [gcloud CLI](#gcloud-cli)
      - [Spanner](#spanner)
  - [Git](#git)
    - [Glossary](#glossary)
    - [Git essentials](#git-essentials)
    - [Worktrees](#worktrees)
    - [Submodules](#submodules)
    - [Misc](#misc)
    - [.gitignore](#gitignore)
    - [Git hooks](#git-hooks)
  - [Docker](#docker)
    - [Installation](#installation)
    - [Commands](#commands)
    - [Dockerfile](#dockerfile)
  - [Networking](#networking)
    - [TCP, HTTP, REST API etc](#tcp-http-rest-api-etc)
      - [Headers](#headers)
        - [Cache-Control](#cache-control)
        - [ETag](#etag)
        - [If-None-Match](#if-none-match)
        - [Browser security headers](#browser-security-headers)
      - [NGINX](#nginx)
        - [Static file server directives](#static-file-server-directives)
      - [Swagger and OpenAPI](#swagger-and-openapi)
      - [Standard endpoints](#standard-endpoints)
  - [Databases](#databases)
    - [Graphs](#graphs)
  - [Security](#security)
    - [OAuth](#oauth)
      - [Vocabulary](#vocabulary)
      - [Common Entra ID configuration](#common-entra-id-configuration)
      - [ID token and access token](#id-token-and-access-token)
      - [Scopes, delegated permissions and application permissions](#scopes-delegated-permissions-and-application-permissions)
      - [Client credentials](#client-credentials)
      - [Authorization Code with PKCE (OIDC)](#authorization-code-with-pkce-oidc)
      - [Frontend + Backend example](#frontend--backend-example)
  - [AI](#ai)
    - [GitHub Copilot](#github-copilot)
    - [Ollama](#ollama)
    - [MCPs](#mcps)
      - [MCP Inspector](#mcp-inspector)
  - [Query languages](#query-languages)
    - [Spanner GQL](#spanner-gql)
      - [MATCH](#match)
    - [PromQL](#promql)
  - [Miscellaneous](#miscellaneous)
    - [OpenSSL](#openssl)
    - [LDAP](#ldap)

## OS

### Linux

#### Essentials

```bash
# Update all packages
sudo apt update
# Upgrade all packages
sudo apt full-upgrade
# Install a package
sudo apt install PACKAGE

# Bash startup
~/.bashrc
# Login startup
~/.profile
```

#### Do after every fresh install

```bash
# Install build tools and compilers
sudo apt install build-essential
```

#### Structure

```text
/dev        devices
/etc        system configuration
/home       user files
/opt        self-contained third-party applications
/proc       kernel and process information
/usr        distribution-managed programs and libraries
/usr/local  manually installed programs
/var        changing data such as logs
```

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

##### systemctl

```bash
# Restart or inspect a service
systemctl [--user] {status|start|restart|stop|enable|disable} SERVICE
# Show a service definition
systemctl [--user] cat SERVICE
# Reload unit files after editing one
systemctl --user daemon-reload
```

System units: `/usr/lib/systemd/system/`; user units: `~/.config/systemd/user/`. Overrides:
`/etc/systemd/system/` or `~/.config/systemd/user/`.

##### journalctl

```bash
# Logs from the current boot
journalctl -b
# Logs for a service
journalctl -u SERVICE
# Errors only
journalctl -p err
```

#### CA certificates

Put one PEM certificate per `.crt` file under `/usr/local/share/ca-certificates/`. Then run:

```bash
sudo install -m 644 certificate.crt /usr/local/share/ca-certificates/name.crt
sudo update-ca-certificates
```

Generated bundle: `/etc/ssl/certs/ca-certificates.crt`. Common variables: `NODE_EXTRA_CA_CERTS`,
`REQUESTS_CA_BUNDLE`, `SSL_CERT_FILE`.

## Terminals

### POSIX

```bash
# Map a branch name to an environment
if [ "$VAR" = develop ]; then
    TARGET=development
elif [ "$VAR" = main ]; then
    TARGET=production
else
    echo "Only main and develop branches can be released" >&2
    exit 1
fi
```

### Linux terminal

#### Bash

```bash
# Exit on errors, unset vars, or pipeline errors
set -euo pipefail

# Redirect stderr; redirect stdout and stderr (Bash)
2>/dev/null
&>/dev/null

# Restart a login shell; execute a script in the current shell
bash --login
. path

# Redirect stdout: overwrite; append
cat test > test1
cat test >> test1

# Remove the longest prefix ending in ':'
USER="${FULL_USER##*:}"

# Sync folders and remove stale files
staging_dir="$(mktemp -d)"
trap 'rm -rf -- "$staging_dir"' EXIT
rsync -a --delete --exclude='node_modules/' --exclude='.env' \
  "$staging_dir/" ./destination/
```

#### Commands - LS, man, curl etc.

```bash
# Fetch a web page; show response headers
curl https://example.com
curl -I https://example.com

# List files: long format, include hidden files, human-readable sizes
ls -lah

# Find files by name, case-insensitively
find . -type f -iname '*.yaml'

# Search recursively with line numbers; -F treats the pattern literally
grep -RInF -- 'PATTERN' .
# Find the literal '-f' option in help output
tar --help | grep -F -- '-f'

# Show filesystem and directory sizes in human-readable units
df -h
du -sh PATH

# Show processes
ps aux

# Show addresses and routes
ip -br address
ip route

# Show NetworkManager devices and DNS servers
nmcli device status
nmcli -f IP4.DNS device show

# Resolve a name; query a specific DNS server
dig +short example.com
dig @DNS_SERVER example.com +short
dig -x 1.2.3.4 +short

# Open a manual page; search manual page names and descriptions
man tar
man -k KEYWORD

In the pager, press `/PATTERN` to search, `n` for the next match, `N` for the previous match, and
`q` to quit.

# Prints content of file to console
cat example.png

# Shows content of file in terminal and allow scroll up and down
less example.png
```

### Windows

```cmd
# Keep a batch script window open
cmd /k my_script.bat
# Count localhost connections
netstat -n | findstr 127.0.0.1 | find /v /c ""
```

#### PowerShell

```powershell
# Filter a string
echo $Env:PATH | Select-String substring
# Invert the filter
echo $Env:PATH | Select-String substring -NotMatch
# Print a file
Get-Content file
# Print an environment variable
echo $Env:PATH
```

#### Cmd

```cmd
# Locate a command
where python
```

## Cloud

### Azure

#### Azure CLI

```bash
# Get an access token
az account get-access-token
# Get a token for a specific resource
az account get-access-token --resource https://graph.microsoft.com
```

### GCP - Google Cloud Platform

#### gcloud CLI

```bash
# Login. If you want to set it as default, add --update-adc
gcloud auth login

# View active config
gcloud config list

# Set project
gcloud config set project Name

# Set default region TODO: if working, remove  --region= from other commands
gcloud config set run/region europe-west1

# Deploy from source (builds container via Cloud Build)
gcloud run deploy SERVICE_NAME \
  --source . \
  --region europe-west1 \
  --allow-unauthenticated  # or --no-allow-unauthenticated

# Deploy from a container image
gcloud run deploy SERVICE_NAME \
  --image europe-west1-docker.pkg.dev/rbprj-100458/REPO/IMAGE:TAG \
  --region europe-west1

# Stream logs
gcloud run services logs read SERVICE_NAME --region=europe-west1 --limit=50

# Describe service
gcloud run services describe SERVICE_NAME --region=europe-west1

# Check your current permissions
gcloud projects get-iam-policy rbprj-100458 \
  --flatten="bindings[].members" \
  --filter="bindings.members:YOUR_EMAIL" \
  --format="table(bindings.role)"
```

#### Spanner

```bash
# List instances
gcloud spanner databases list --instance=INSTANCE_NAME

# List DBs
gcloud spanner databases list --instance=INSTANCE_NAME

# Run a single SQL query
gcloud spanner databases execute-sql DATABASE_NAME \
  --instance=INSTANCE_NAME \
  --sql="SELECT * FROM Document LIMIT 10"

# Run from a file
gcloud spanner databases execute-sql DATABASE_NAME \
  --instance=INSTANCE_NAME \
  --sql="$(cat query.sql)"

# Apply DDL changes (e.g. create table, add column)
gcloud spanner databases ddl update DATABASE_NAME \
  --instance=INSTANCE_NAME \
  --ddl="ALTER TABLE Document ADD COLUMN new_col STRING(MAX)"

# Show current DDL (all CREATE TABLE statements)
gcloud spanner databases ddl describe DATABASE_NAME \
  --instance=INSTANCE_NAME

# Create a new database
gcloud spanner databases create DATABASE_NAME \
  --instance=INSTANCE_NAME \
  --ddl-file=schema.sql

# Export database to GCS (creates a Dataflow job)
gcloud spanner databases export DATABASE_NAME \
  --instance=INSTANCE_NAME \
  --destination=gs://BUCKET_NAME/export/

# Import from GCS
gcloud spanner databases import DATABASE_NAME \
  --instance=INSTANCE_NAME \
  --source=gs://BUCKET_NAME/export/
```

## Git

### Glossary

- `origin`: remote URL shorthand
- `merge`: add another branch's changes with a new commit
- `rebase`: replay commits on another base; avoid rewriting shared branches
- `fast-forward`: move the branch pointer when no merge commit is needed

### Git essentials

```bash
# Create or clone a repository
git init
git clone REPOSITORY_URL

# Inspect, stage, commit, and review history
git status
git add -A
git commit -am "message"
git log --oneline --decorate

# Add a remote and push a branch
git remote add origin REPOSITORY_URL
git push -u origin BRANCH

# Create or switch branches and merge changes
git switch -c feature
git switch main
git merge feature

# Delete a local or remote branch
git branch -d feature
git push --delete origin feature

# Discard changes or unstage a file
git restore FILE
git restore --staged FILE
```

### Worktrees

```bash
# List worktrees
git worktree list
# Add a worktree for an existing branch
git worktree add ../repo-feature feature
# Create a branch and worktree from main
git worktree add -b feature ../repo-feature main
# Remove a clean worktree
git worktree remove ../repo-feature
# Remove stale worktree metadata
git worktree prune
# Prevent or allow pruning
git worktree lock ../repo-feature
git worktree unlock ../repo-feature
```

### Submodules

```bash
# Clone a repository with its submodules
git clone --recurse-submodules REPOSITORY_URL
# Add a submodule
git submodule add REPOSITORY_URL path/to/submodule
# Initialize submodules after cloning
git submodule update --init --recursive
# Show or update submodules at recorded commits
git submodule status
git submodule update --recursive
git submodule update --remote --merge --recursive

# Inspect and update a submodule
git -C path/to/submodule status
git -C path/to/submodule pull

# Record an updated submodule commit
git add path/to/submodule
git commit -m "Update submodule"
git submodule foreach --recursive git status
git submodule deinit -f path/to/submodule
git rm -f path/to/submodule
```

### Misc

```bash
# Delete local branches whose upstream was removed
git fetch --prune
git branch -vv | awk '/: gone]/{print $1}' | xargs -r git branch -d

# Stop tracking a file that is now ignored
git rm --cached -r -- path/to/file

# Fork workflow
git remote add upstream ORIGINAL_URL
git fetch upstream

# Delete a tag
git tag -d TAG
git push --delete origin TAG
```

### .gitignore

```gitignore
readme.txt       # Any matching path
/readme.txt      # Repository root only
output/          # Directory
*.java           # All Java files
abc/**           # Everything below abc
a/**/b           # b at any depth below a
!readme.txt      # Do not ignore this path
```

### Git hooks

```bash
# Use a custom hooks directory
git config core.hooksPath git_hooks_path
```

## Docker

### Installation

Install Docker using the official instructions:
[Linux](https://docs.docker.com/engine/install/ubuntu/) or
[Windows](https://docs.docker.com/desktop/install/windows-install/).

### Commands

```bash
# Show Docker and Compose versions
docker version
# Build an image
docker build -f Dockerfile -t IMAGE .
# Run a container and publish a port
docker run -d --name CONTAINER -p 80:80 IMAGE
# Start a Compose application
docker compose up -d --build --remove-orphans
# Download an image; list images and running containers
docker pull IMAGE
docker images
docker ps
# Open a shell in a running container
docker exec -it CONTAINER sh
# Stop and remove a container; remove an image
docker stop CONTAINER
docker rm CONTAINER
docker rmi IMAGE
docker cp source/path CONTAINER:/destination/path
```

### Dockerfile

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine:3.20
RUN apk add --no-cache curl
ENV PG_MAJOR=9.3
EXPOSE 80
COPY . .
CMD ["sh"]
```

## Networking

### TCP, HTTP, REST API etc

#### Headers

##### Cache-Control

`max-age` sets how long a response is fresh. `public` allows shared caches, such as a CDN, to store
it; `immutable` tells clients not to revalidate it while it is fresh. With NGINX, `expires 1y` sets
a one-year freshness lifetime (including `max-age`), while an explicit
`Cache-Control: public, immutable` adds the cache directives. A cache may still evict a response
early. Long-lived caching is best for assets whose filenames change when their contents change.

```http
Cache-Control: public, max-age=31536000, immutable
Cache-Control: public, max-age=60, stale-while-revalidate=300
Cache-Control: private, max-age=300
Cache-Control: no-cache
Cache-Control: no-store
```

##### ETag

An `ETag` identifies a response version; send it back as `If-None-Match`. An unchanged resource
returns `304 Not Modified`.

##### If-None-Match

```http
If-None-Match: "VERSION"
```

##### Browser security headers

- `X-Content-Type-Options: nosniff` tells browsers to use the declared `Content-Type` instead of
  guessing a different type from the response body.
- `X-Frame-Options: DENY` prevents the page from being displayed in a frame. CSP's `frame-ancestors`
  directive is the more flexible modern control.
- `Content-Security-Policy` restricts where a page may load scripts, styles, images, frames, and
  other resources. It can also control embedding and form submissions. Start with a restrictive
  policy and allow only sources the application needs.
- `Referrer-Policy: strict-origin-when-cross-origin` sends the full URL as the referrer for
  same-origin requests, only the origin for cross-origin HTTPS requests, and no referrer when
  navigating from HTTPS to HTTP.
- `Cross-Origin-Opener-Policy: same-origin` separates the page's browsing context from documents on
  other origins, limiting communication through window references such as `window.opener`.
- `Cross-Origin-Resource-Policy: same-origin` tells browsers to block other origins from loading the
  response as a cross-origin, no-CORS resource. This can affect resources intended for use by
  another origin.
- `Permissions-Policy` controls which browser features, such as camera, microphone, or geolocation,
  the page and embedded frames may use. For example, `camera=()` disables camera use;
  `microphone=(self)` allows it only for the same origin.

These headers are browser-enforced defenses, not substitutes for correct application behavior. For
example, CSP does not replace output escaping, and frame restrictions do not replace authorization
checks.

#### NGINX

##### Static file server directives

```nginx
# hides the NGINX version from generated error pages and the `Server` response header. It does not hide that the server is NGINX.


server_tokens off;


root /usr/share/nginx/html;


# `index` lists the file names NGINX tries when a request targets a directory. With `index index. html`, a request for `/` can serve `/usr/share/nginx/html/index.html`.
index index.html;
```

#### Swagger and OpenAPI

OpenAPI describes an HTTP API; Swagger UI renders it interactively.

```bash
docker run --rm -p 8080:8080 -e SWAGGER_JSON=/spec.json \
    -v "$PWD/spec.json:/spec.json" swaggerapi/swagger-ui
```

Use `.swagger.json` for OpenAPI 2 and `.openapi.json` for OpenAPI 3.

#### Standard endpoints

```text
/.well-known/openid-configuration
```

## Databases

### Graphs

RDF: Resource Description Framework

## Security

### OAuth

The client credentials grant is for service-to-service access; it does not sign in a user and does
not return an OIDC ID token. Use Authorization Code with PKCE when a user signs in. A real browser
application should use MSAL rather than manually implementing this protocol.

#### Vocabulary

- **Tenant:** the organization's Entra directory and identity boundary.
- **App registration:** the application's identity/configuration in Entra, including its client ID,
  redirect URIs, and permissions or exposed API scopes.
- **Enterprise application (service principal):** the tenant-local instance of an app registration;
  this is where admins can manage access, assignment, and consent in that tenant.
- **Client:** the application requesting a token. Angular is a public client; a server-side web app
  can be a confidential client.
- **Public client:** code runs on a device or in a browser and cannot keep a client secret private.
  Use PKCE, not a secret embedded in frontend code.
- **Confidential client:** server-side code can protect a credential, such as a certificate or
  secret. A secret belongs only on the server, never in Angular.
- **Scope:** a delegated permission requested on behalf of a signed-in user, for example
  `api://<API_CLIENT_ID>/access_as_user`. There is predefined one `.default` that you dont need to
  define and it means all configured scopes. You can have more scopes in one token applience, but
  all scopes needs to point to the same resource otherwise it's not valid.
- **Audience (`aud`):** the intended recipient of a token. An ID token is for the frontend client;
  an API access token is for the backend API.

#### Common Entra ID configuration

- **Supported account types / tenant:** choose which organization's or consumer accounts may sign
  in. A company-only app commonly uses a single-tenant setting.
- **Platform and redirect URI:** configure the URI where Entra returns the sign-in response. Choose
  **Single-page application (SPA)** for Angular; it is a public client and must not use a client
  secret. Choose **Web** for a server-side web app that can keep credentials on its server. The
  platform affects redirect and flow settings; it does not make browser code safe for secrets.
- **Expose an API:** on the backend/API registration, set its Application ID URI and define scopes
  such as `access_as_user`.
- **API permissions:** on the frontend registration, add the API's delegated scope so the client is
  allowed to request it.
- **Admin consent:** an administrator approves requested permissions for the organization, often to
  avoid each user consenting individually. Consent grants the permission; it does not by itself
  assign users to the app. If **Assignment required** is enabled on the enterprise application,
  users or groups must also be assigned.

#### ID token and access token

- **ID token:** proves to the frontend that the user signed in. It is intended for the frontend
  (`aud` is the frontend's client ID) and can contain identity claims such as a subject and display
  name. It is not an API authorization token.
- **Access token:** grants access to a particular API/resource. Its `aud` identifies that API and
  its scopes (often in `scp`) describe delegated permissions. Send it to the API as a bearer token;
  the API validates it. Access tokens are for the API, so frontend code should treat them as opaque.

#### Scopes, delegated permissions and application permissions

Scope defines what can be done. If someone applies to be able use scope, it's called permission.

Scope can be delegated and application. Application scope is applied through Client credential flow,
the delegated via Authorization flow.

Permissions allows you to do something in other app. If you apply for permission, it means my Entra
ID App can get token for another app (`aud` claim in a token).For example to get permission to read
personal messages from teams, you need to use delegated one as permission for application will never
be granted by user as it would mean, that application could read all personal chats of all users.

Beware, that if you add permission, it may not be respected until you get admin grant. It can be
true even for permissions that stated `Admin consent required: No`.

- **Delegated permission:** Is used in the name of the particular user. It has same permissions as
  user and user needs to approve permissions in pop up. The flow used for getting token with these
  permissions is Authorization flow (interactive sign in browser)
- **Application permission:** Is used as an application. The entity that have permissions or not is
  Service principal (Entra ID app). Some rights are not able to get.

#### Client credentials

Use this only from a confidential server-side client. Grant the app registration application
permissions to the target API and admin consent as required. Keep the client secret out of source
control and browser code.

```bash
# Get a client credentials token
curl --request POST "https://login.microsoftonline.com/<TENANT_ID>/oauth2/v2.0/token" \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=client_credentials' \
  --data-urlencode 'client_id=<CLIENT_ID>' \
  --data-urlencode 'client_secret=<CLIENT_SECRET>' \
  --data-urlencode 'scope=<API_APPLICATION_ID_URI>/.default'
```

#### Authorization Code with PKCE (OIDC)

This flow signs in a user. Register a public client redirect URI in Microsoft Entra ID; for this
manual curl example, use the **Mobile and desktop applications** platform and `http://localhost`.
The user must complete sign-in in a browser, so curl can prepare the `/authorize` request but cannot
perform the interactive login. Copy the returned `code` from the browser's redirected URL and verify
that the returned `state` matches the value generated below. The same `redirect_uri` and PKCE
verifier are required when exchanging the code. Do not add a client secret to a public client.

Set `TENANT_ID`, `CLIENT_ID`, and `API_SCOPE` in the shell first. `API_SCOPE` is a delegated scope
exposed by the API, for example `api://API_CLIENT_ID/access_as_user`.

```bash
REDIRECT_URI='http://localhost'
AUTH_ENDPOINT="https://login.microsoftonline.com/${TENANT_ID}/oauth2/v2.0/authorize"
TOKEN_ENDPOINT="https://login.microsoftonline.com/${TENANT_ID}/oauth2/v2.0/token"
SCOPE="openid profile email offline_access ${API_SCOPE}"
STATE="$(openssl rand -hex 16)"
NONCE="$(openssl rand -hex 16)"
CODE_VERIFIER="$(openssl rand -hex 32)"
CODE_CHALLENGE="$(printf '%s' "$CODE_VERIFIER" | openssl dgst -sha256 -binary | openssl base64 -A | tr '+/' '-_' | tr -d '=')"

# Build the authorization URL, then open it in a browser and sign in.
AUTH_URL="$(curl --silent --output /dev/null --write-out '%{url_effective}' \
  --get "$AUTH_ENDPOINT" \
  --data-urlencode "client_id=${CLIENT_ID}" \
  --data-urlencode 'response_type=code' \
  --data-urlencode "redirect_uri=${REDIRECT_URI}" \
  --data-urlencode 'response_mode=query' \
  --data-urlencode "scope=${SCOPE}" \
  --data-urlencode "state=${STATE}" \
  --data-urlencode "nonce=${NONCE}" \
  --data-urlencode "code_challenge=${CODE_CHALLENGE}" \
  --data-urlencode 'code_challenge_method=S256')"
printf 'Open this URL in a browser:\n%s\n' "$AUTH_URL"

# After sign-in, copy code from the redirect URL and confirm its state equals $STATE.
AUTH_CODE='CODE_COPIED_FROM_REDIRECT_URL'

# Exchange the one-time authorization code for tokens. No client secret for a public client.
curl --request POST "$TOKEN_ENDPOINT" \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode "client_id=${CLIENT_ID}" \
  --data-urlencode 'grant_type=authorization_code' \
  --data-urlencode "code=${AUTH_CODE}" \
  --data-urlencode "redirect_uri=${REDIRECT_URI}" \
  --data-urlencode "scope=${SCOPE}" \
  --data-urlencode "code_verifier=${CODE_VERIFIER}"
```

The `openid` scope requests an ID token (OIDC) (aud is the web page that provides logging); the
delegated `API_SCOPE` requests an access token for the API (aud is the API URL). In an Angular SPA,
register the redirect URI under the **Single-page application** platform and let MSAL handle the
authorization-code/PKCE exchange in the browser. A SPA redirect URI is not interchangeable with the
native-client redirect used in this curl walkthrough. See the
[authorization code flow documentation](https://learn.microsoft.com/entra/identity-platform/v2-oauth2-auth-code-flow).

#### Frontend + Backend example

Generate 2 app registrations in Entra ID. App Back is for backend. Expose a scope there (e.g.
`<api-client-id>/access_as_user`). App Front is for frontend where apply for permissions of scope
from A. In Back add Front as Authorized client, so it is not necessary to consent with permisssions.

To Front add redirect uri to your hosted page and to localhost for development.

In your frontend app you can only use app B. Apply for this scope
`openid profile api://<api-client-id>/access_as_user`. There will be two tokens available ID tokens
with aud pointing to SPA and access token with aud to your API (<api-client-id> that will be checked
in your API code).

## AI

### GitHub Copilot

```bash
gh skill install github/awesome-copilot documentation-writer
```

### Ollama

The API has two parts:

- OpenAI-compatible endpoints:
  - `http://localhost:11434/v1/completions`
  - `http://localhost:11434/v1/chat/completions`
  - `http://localhost:11434/v1/models`
- Custom Ollama API: `http://localhost:11434/api/...`

```bash
# Download a model
ollama pull MODEL
# Run a model
ollama run MODEL
# Start the server; OpenAI-compatible API is at localhost:11434/v1
ollama serve
```

```python
from openai import OpenAI

client = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")
response = client.chat.completions.create(
    model="MODEL",
    messages=[{"role": "user", "content": "Question"}],
)
print(response.choices[0].message.content)
```

### MCPs

#### MCP Inspector

```bash
ALLOWED_ORIGINS=http://127.0.0.1:6274 npx @modelcontextprotocol/inspector
```

## Query languages

### Spanner GQL

`GRAPH` selects the named graph to query. In a `MATCH` pattern, parentheses represent nodes and
brackets represent edges. For example:

```text
(account:Account)-[transfer:Transfers]->(destination:Account)
```

- `(account:Account)` matches a node labeled `Account`. `account` is an alias used later in the
  query to refer to this node.
- `[transfer:Transfers]` matches an edge labeled `Transfers`; `transfer` is its alias.
- `->` means the edge points from `account` to `destination`. Reversing the arrow would match the
  opposite direction.
- `(destination:Account)` matches the node at the other end, also labeled `Account`.

Together, the pattern finds outgoing `Transfers` edges from one `Account` node to another. The
aliases let the rest of the query read properties such as `account.id` or `transfer.amount`.
`@account_id` is a query parameter supplied by the client.

```sql
GRAPH FinGraph
MATCH (account:Account)-[transfer:Transfers]->(destination:Account)
WHERE account.id = @account_id
RETURN destination.id, transfer.amount
ORDER BY transfer.amount DESC
LIMIT 10;
```

#### MATCH

```sql
-- The MATCH clause doesn't need to be just `node - edge - node` but it can bu just node for example
MATCH (n:Material)

-- It can do multihop like
MATCH (a)-[e1]->(b)-[e2]->(c)

-- Nodes are in () brackets and edges are in square brackets and can be omited to match any edge.
MATCH (n)->(m)
```

### PromQL

```promql
# CPU usage for all nodes
node_cpu_seconds_total
# Average CPU usage over 5 minutes
avg(rate(node_cpu_seconds_total[5m]))
```

## Miscellaneous

### OpenSSL

```bash
# Show the SSL certificate used by an HTTPS page
openssl s_client -showcerts -connect git.example.com:443 \
  </dev/null 2>/dev/null \
    | openssl x509 -outform PEM > gitlab.crt
# Show the complete certificate chain through a proxy
openssl s_client -connect teams.microsoft.com:443 \
  -proxy localhost:3128 -servername teams.microsoft.com -showcerts \
  </dev/null 2>&1 | tee /tmp/teams_tls.txt
```

### LDAP

```python
from ldap3 import Connection, SAFE_SYNC, Server

# Connect to LDAP
server = Server("ldap.example.com", port=3268)
connection = Connection(
    server, "USER@EXAMPLE.COM", "PASSWORD", client_strategy=SAFE_SYNC,
    auto_bind=True, auto_referrals=False,
)

# Find a group by CN
status, result, response, _ = connection.search(
  "DC=bosch,DC=com", "(CN=Bj_PS_dux_streaming_c-schicht_editor)"
)

# Find a user by account name
status, result, response, _ = connection.search(
  "DC=bosch,DC=com", "(sAMAccountName=psx6fe)"
)

# Find records of a type, e.g. users
status, result, response, _ = connection.search(
  "DC=bosch,DC=com", "(objectClass=User)"
)

# Find all records matching an attribute
status, result, response, _ = connection.search(
  "DC=bosch,DC=com", "(sAMAccountName=*)"
)
```
