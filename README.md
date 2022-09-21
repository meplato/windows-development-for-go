# Windows Development environment for Go

This is just a short summary for Hipsters developing with Go
on Windows.

## Install package manager & Windows Terminal

First, install [Chocolatey](https://chocolatey.org/), a package manager for Windows,
as described [here](https://chocolatey.org/install).
Secondly, install [Windows terminal](https://github.com/microsoft/terminal).
Thirdly, install your first choco package, the latest powershell:
```
choco install powershell-core -y
```

## PowerShell settings

You should allow your PowerShell to execute scripts.
You're a developer--the terminal is your friend.

```
Set-ExecutionPolicy Unrestricted -Scope CurrentUser
```

## Install packages

Next, open the windows terminal as an administrator. Then install
all the tools we want as a Go programmer on Windows by running:

```
# Docker for Windows
choco install docker-desktop -y

# protoc (Compiler for protobuf)
choco install protoc -y

# Python, required for some npm packages
choco install python -y
choco install python2 -y
# IMPORTANT: make sure to permanently add the Python27 path to the path environment variable


# Go programming language
choco install go -y
choco install golang -y

# Run make on Windows
choco install make -y

# Git on Windows
choco install git -y --params="'/GitAndUnixToolsOnPath /NoAutoCrlf'"
choco install gh -y
choco install github-desktop -y
choco install poshgit -y
refreshenv

# NodeJS (v14.18.1 because mall uses this version) and tools
choco install nodejs --version=14.18.1 -y
choco install yarn -y
npm install -g node-gyp@6.0.1
npm install -g eslint@6.8.0

# Visual Studio Code (and Fira Code font)
choco install vscode -y
choco install FiraCode -y
```

From time to time, you might run [`choco outdated`](https://chocolatey.org/docs/commands-outdated) to see which packages have new versions and [`choco upgrade`](https://chocolatey.org/docs/commands-upgrade) accordingly.

## PHRASEAPP
To install it, just use choco: `choco install phraseapp -y` 

After that you need to [login](https://app.phrase.com/account/login) and then go to the [Access Tokens page](https://app.phrase.com/settings/oauth_access_tokens). Generate a new access token and copy it. 

*Important:* If you leave the page, you can never again view the generated access token. You only can create a new one.
And then create a new environment variable, with PowerShell: 
`$env:PHRASEAPP_ACCESS_TOKEN="YOUR_TOKEN_HERE"`

## Go configuration

Please open a new terminal window, _not running as administrator_.

Type `go env` to see the settings of your Go installation:

```
C:\Users\Marek> go version
go version go1.19.1 windows/amd64
C:\Users\Marek> go env
set GO111MODULE=
set GOARCH=amd64
set GOBIN=
set GOCACHE=C:\Users\Marek\AppData\Local\go-build
set GOENV=C:\Users\Marek\AppData\Roaming\go\env
set GOEXE=.exe
set GOEXPERIMENT=
set GOFLAGS=
set GOHOSTARCH=amd64
set GOHOSTOS=windows
set GOINSECURE=
set GOMODCACHE=C:\Users\Marek\go\pkg\mod
set GONOPROXY=github.com/meplato/*,meplato.com/*,meplato.cloud/*
set GONOSUMDB=github.com/meplato/*,meplato.com/*,meplato.cloud/*
set GOOS=windows
set GOPATH=C:\Users\Marek\go
set GOPRIVATE=github.com/meplato/*,meplato.com/*,meplato.cloud/*
set GOPROXY=https://proxy.golang.org,direct
set GOROOT=C:\Program Files\Go
set GOSUMDB=sum.golang.org
set GOTMPDIR=
set GOTOOLDIR=C:\Program Files\Go\pkg\tool\windows_amd64
set GOVCS=
set GOVERSION=go1.19.1
set GCCGO=gccgo
set GOAMD64=v1
set AR=ar
set CC=gcc
set CXX=g++
set CGO_ENABLED=1
set GOMOD=NUL
set GOWORK=
set CGO_CFLAGS=-g -O2
set CGO_CPPFLAGS=
set CGO_CXXFLAGS=-g -O2
set CGO_FFLAGS=-g -O2
set CGO_LDFLAGS=-g -O2
set PKG_CONFIG=pkg-config
set GOGCCFLAGS=-m64 -mthreads -fno-caret-diagnostics -Qunused-arguments -Wl,--no-gc-sections -fmessage-length=0 -fdebug-prefix-map=C:\Users\Marek\AppData\Local\Temp\go-build3035694521=/tmp/go-build -gno-record-gcc-switches
```

## Visual Studio Code

You can start Visual Studio Code as `code` from the command line
(PowerShell, cmd.exe, Hyper etc.).

```
code
```

Once VSCode is ready, review its [settings](https://code.visualstudio.com/docs/getstarted/settings). Notice that it has an integrated settings editor; but often it's much easier to simply edit the `settings.json` manually. On Windows, it is located at `%APPDATA%\Code\User\settings.json`.

Here's how my `settings.json` looks like:

```
{
    "terminal.integrated.shell.windows": "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
    "go.lintTool": "golangci-lint",
    "go.lintFlags": ["--fast"],
    "telemetry.enableTelemetry": false,
    "telemetry.enableCrashReporter": false,
    "editor.fontFamily": "Fira Code",
    "editor.fontLigatures": true,
    "window.zoomLevel": 0
}
```

Using [Fira Code](https://github.com/tonsky/FiraCode) is, of course, completely optional. But I like that font for it has ligatures.

I also use PowerShell as the integrated terminal, but you can use anything that works for you.

The lint tool can be installed from the integrated terminal (Terminal -> New Terminal):

```
GO111MODULE=on go get github.com/golangci/golangci-lint/cmd/golangci-lint@v1.21.0
```

## Git configuration

Make sure to disable automatic conversion of LF to CRLF (and vice versa) (see [here](https://help.github.com/en/articles/dealing-with-line-endings) for details).

```
git config --global core.autocrlf true
```

Next, setup your credentials. They will be used e.g. when pushing a branch
over to GitHub:

```
git config --global user.email xxx@meplato.de
git config --global user.name "Firstname Lastname"
```

You might also want to set up some aliases:

```
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.pushf "push --force-with-lease"
```

If you want to use Visual Studio Code for
[editing your git commit and diff messages](https://code.visualstudio.com/docs/editor/versioncontrol#_vs-code-as-git-editor),
you can do so by:

```
git config --global core.editor "code --wait"
```
