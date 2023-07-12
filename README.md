# Vega Protocol Core Development environment

This repo provides a Nix flake that creates a development environment with the Go version and packages required for development of the Vega Core project. This will
allow you to have the appropriate versions required for developing the Vega Core project without having to make changes to your existing Go versions or libraries
that you might use in other projects.

## Pre-requisites

### Nix

Download and install the `nix` [package manager](https://nixos.org/download.html) for your system. The use of flakes requires you to enable flakes
and the nix command under experimental-features in your nix configuration.

```text
# /etc/nix/nix.conf or ~/.config/nix/nix.conf

experimental-features = nix-command flakes
```

### Direnv

[direnv](https://direnv.net) is a shell extension that allows you to augment your shell and allows you to set different environments for different folders.

By using `direnv` and this flake, you can have a development environment with the correct versions of Go and Go dependencies installed whenever you enter
the folder you keep your Vega Protocol project files.

Install `direnv` using the [installation instructions](https://direnv.net/docs/installation.html) and make sure you [hook](https://direnv.net/docs/hook.html)
to your shell.

You should open a new shell after you have set up the hook for it to enable `direnv`.

## Recommended use

To use this flake, create a folder for your vega development environment. When you have completed setup, navigating to this folder will automatically activate
the Vega development shell.

```bash
mkdir -p /path/to/vega-dev-shell
```

Create a `.envrc` file under your `/path/to/vega-dev-shell` and tell it to use the environment specified in this flake:

```bash
echo "use flake github:guoguojin/vega-dev-shell" > /path/to/vega-dev-shell/.envrc
```

Navigate to your `/path/to/vega-dev-shell`. It will complain that `.envrc` is blocked and you should run `direnv allow` to approve its content. Run `direnv allow`
as instructed and nix will start to download and install the packages and setup the environment variables required for the Vega development shell. By default, this
will install the recommended version of Go along with the versions of libraries specified in the `scripts/gettools.sh` script of the Vega project, except it will do
it in a separate GOPATH leaving your existing Go installation and GOPATH intact.

It may take a few minutes the first time it loads your environment to install everything required, but after that it should start up your development environment
very quickly as soon as you have navigated to your `/path/to/vega-dev-shell`.

Your terminal should show something similar to this:

```text
direnv: loading /path/to/vega-dev-shell/.envrc
direnv: using flake .
Setting XDG_CACHE to /home/<your-user-name>/.cache
Vega development shell
Go version go version go1.20.5 linux/amd64
GOROOT=/nix/store/i3ab37h47xmd0zh75708gj57hah7v7f4-go-1.20.5/share/go
GOPATH=/home/tanq/.cache/dev-shell/vega/go
```

To make sure the development environment has activated properly, you can check which Go binary you are using and the GOPATH for the environment:

```sh
[tanq@nightshade repos]$ which go
/nix/store/i3ab37h47xmd0zh75708gj57hah7v7f4-go-1.20.5/bin/go
[tanq@nightshade repos]$ echo $GOPATH
/home/tanq/.cache/dev-shell/vega/go
```

If you navigate out of this folder, you should see:

```
direnv: unloading
```

Clone the [Vega Core](https://github.com/vegaprotocol/vega.git) project under `/path/to/vega-dev-shell`.

You should now be ready to use this Vega development environment. If you use (n)vim or Emacs then you should be all set. If you use an IDE or VS Code
then there may be a few other steps you need to take to configure your code editor.

## Integrate with Jetbrains IDEs

To use this development environment with a Jetbrain IDE (e.g. GoLand or IntelliJ with Go plugin) you can edit the settings for GOROOT and Project GOPATH
to the paths that are logged out when you enter the Vega development shell. This will make the Jetbrains IDE use the Go version specified in GOROOT
and the appropriate applications that have been installed to the GOPATH. If you start a terminal, the terminal should pick up the development environment
automatically.

## Integrate with VS Code

If you start `code` from the terminal when in the Vega development shell directly, the `GOROOT` and `GOPATH` environment variables should be picked up by
VS Code automatically and you shouldn't need to do anything further.

If you start VS Code from anywhere outside the Vega development shell, you can update the settings.json to point GOROOT and GOPATH to the appropriate
paths that are created by the development shell.
