# Windows Development environment for Go

This is just a short summary for Hipsters developing with Go
on Windows.

## Install package manager

First, install [Chocolatey](https://chocolatey.org/), a package manager for Windows,
as described [here](https://chocolatey.org/install).

## PowerShell settings

You should allow your PowerShell to execute scripts.
You're a developer--the terminal is your friend.

```
Set-ExecutionPolicy Unrestricted -Scope CurrentUser
```

## Windows settings

You might want to enable HyperV for Windows.

```
choco install Microsoft-Hyper-V-All -y -source windowsFeatures
```

## Install packages

Next, open PowerShell as an administrator. Then install
all the tools we want as a Go programmer on Windows by
copying over the [`packages.config`](https://github.com/meplato/windows-development-for-go/blob/master/packages.config)
file from this repository. Then run:

```
# Docker for Windows
choco install docker-for-windows -y

# Go programming language
choco install golang -y

# Run make on Windows
choco install make -y

# Hyper is a terminal like iterm2 on macOS
choco install hyper -y

# Git on Windows
choco install git.install -y --params="'/GitAndUnixToolsOnPath /NoAutoCrlf'"
choco install poshgit -y
refreshenv

# Update path (might not be necessary)
# $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")

choco install Git-Credential-Manager-for-Windows -y
choco install github -y

# NodeJS (LTS) and tools
choco install nodejs-lts -y
npm install -g npm-windows-upgrade
npm install -g windows-build-tools
npm install -g node-gyp
npm install -g yarn

# Visual Studio Code (and Fira Code font)
choco install visualstudiocode -y
choco install firacode -y

# Ruby (e.g. for generating markdown pages with GitHub wiki)
choco install ruby -y
choco install ruby.devkit -y

# Various other tools
choco install 7zip.install -y
choco install sysinternals -y
```

From time to time, you might run [`choco outdated`](https://chocolatey.org/docs/commands-outdated) to see which packages have new versions and [`choco upgrade`](https://chocolatey.org/docs/commands-upgrade) accordingly.

## Go configuration

Please open a new PowerShell window, _not running as administrator_.

Type `go env` to see the settings of your Go installation:

```
C:\Users\oliver> go version
go version go1.12.4 windows/amd64
C:\Users\oliver> go env
set GOARCH=amd64
set GOBIN=
set GOCACHE=C:\Users\oliver\AppData\Local\go-build
set GOEXE=.exe
set GOFLAGS=
set GOHOSTARCH=amd64
set GOHOSTOS=windows
set GOOS=windows
set GOPATH=C:\Users\oliver\go
set GOPROXY=
set GORACE=
set GOROOT=C:\Go
set GOTMPDIR=
set GOTOOLDIR=C:\Go\pkg\tool\windows_amd64
set GCCGO=gccgo
set CC=gcc
set CXX=g++
set CGO_ENABLED=1
set GOMOD=
set CGO_CFLAGS=-g -O2
set CGO_CPPFLAGS=
set CGO_CXXFLAGS=-g -O2
set CGO_FFLAGS=-g -O2
set CGO_LDFLAGS=-g -O2
set PKG_CONFIG=pkg-config
set GOGCCFLAGS=-m64 -mthreads -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=C:\Users\oliver\AppData\Local\Temp\go-build404466978=/tmp/go-build -gno-record-gcc-switches
```

Especially important is the `GOPATH` variable. Prior to the introduction of
[Go modules](https://github.com/golang/go/wiki/Modules), the `GOPATH` variable
specifies your Go workspace, i.e. where your source code and projects live.

With [Go modules](https://github.com/golang/go/wiki/Modules) enabled,
`GOPATH` becomes obsolete and is deprecated from Go 1.13 and later.

To enable Go modules for Go versions prior to Go 1.13, you need to set the
`GO111MODULE` environment variable to `on`. With PowerShell, do:

```
$env:GO111MODULE="on"
Get-ChildItem Env:
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
GO111MODULE=on go get github.com/golangci/golangci-lint/cmd/golangci-lint@v1.16.0
```

## Git configuration

Make sure to disable automatic conversion of LF to CRLF (and vice versa).

```
git config --global core.autocrlf false
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
