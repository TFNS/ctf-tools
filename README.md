# ctf-tools
[![Build Status](https://travis-ci.org/zardus/ctf-tools.svg?branch=master)](https://travis-ci.org/zardus/ctf-tools)
[![IRC](https://img.shields.io/badge/freenode-%23ctf--tools-green.svg)](http://webchat.freenode.net/?channels=#ctf-tools)

This is a collection of setup scripts to create an install of various security research tools.
Of course, this isn't a hard problem, but it's really nice to have them in one place that's easily deployable to new machines and so forth.
The install-scripts for these tools are checked regularly, the results can be found on [the build status page](_buildstatus/index.md).

Installers for the following tools are included:

| Category | Source | Tool | Description |
|----------|--------|------|-------------|
| stego | apt | [pngtools](https://launchpad.net/ubuntu/+source/pngtools) | PNG's analysis tool. | <!--deb-tool-->
| stego | Directory | [sound-visualizer](http://www.sonicvisualiser.org/) | Audio file visualization. | <!--tool--><!--failing-->
| stego | Directory | [steganabara](http://www.caesum.com/handbook/stego.htm) | Another image stenography solver. | <!--tool--><!--test-->
| stego | Directory | [stegdetect](http://www.outguess.org/) | Stenography detection/breaking tool. | <!--tool--><!--test-->
| stego | Docker | [stego-toolkit](https://github.com/DominicBreuker/stego-toolkit) | A docker image with dozens of steg tools. | <!--tool--><!--no-test-->
| stego | Directory | [stegsolve](http://www.caesum.com/handbook/stego.htm) | Image stenography solver. | <!--tool--><!--test-->
| stego | Directory | [stegosaurus](https://github.com/AngelKitty/stegosaurus) | A steganography tool for embedding arbitrary payloads in Python bytecode (pyc or pyo) files. | <!--tool--><!--no-test-->
| stego | Directory | [zsteg](https://github.com/zed-0xff/zsteg) | detect stegano-hidden data in PNG & BMP. | <!--tool--><!--no-test-->

## Usage

To use, do:

```bash
# set up the path
/path/to/ctf-tools/bin/manage-tools setup
source ~/.bashrc

# list the available tools
manage-tools list

# install gdb, allowing it to try to sudo install dependencies
manage-tools -s install gdb

# install pwntools, but don't let it sudo install dependencies
manage-tools install pwntools

# install qemu, but use "nice" to avoid degrading performance during compilation
manage-tools -n install qemu

# uninstall gdb
manage-tools uninstall gdb

# uninstall all tools
manage-tools uninstall all

# search for a tool
manage-tools search preload
```

Where possible, the tools keep the installs very self-contained (i.e., in to tool/ directory), and most uninstalls are just calls to `git clean` (**NOTE**, this is **NOT** careful; everything under the tool directory, including whatever you were working on, is blown away during an uninstall).
One exception to this are python tools, which are installed using the `pip`
package manager if possible. A `ctftools` virtualenv is created during the
`manage-tools setup` command and can be accessed using the command
`workon ctftools`.

## Help!

Something not working?
I didn't write (almost) any of these tools, but hit up [#ctf-tools on freenode](http://webchat.freenode.net/?channels=#ctf-tools) if you're desperate.
Maybe some kind soul will help!

## Docker (version 1.7+)

By popular demand, a Dockerfile has been included.
You can build a docker image with:

```bash
git clone https://github.com/zardus/ctf-tools
cd ctf-tools
docker build -t ctf-tools .
```

And run it with:

```bash
docker run -it ctf-tools
```

The built image will have ctf-tools cloned and ready to go, but you will still need to install the tools themselves (see above).

Alternatively, you can also pull ctf-tools (with some tools preinstalled) from dockerhub:

```bash
docker run -it zardus/ctf-tools
```

## Vagrant

You can build a Vagrant VM with:

```bash
wget https://raw.githubusercontent.com/zardus/ctf-tools/master/Vagrantfile
vagrant plugin install vagrant-vbguest
vagrant up
```

And connect to it via:

```bash
vagrant ssh
```

## Kali Linux

Kali Linux (Sana and Rolling), due to manually setting certain libraries to not use the latest version available (sometimes being out of date by years) causes some tools to not install at all, or fail in strange ways. AFL and Panda comes to mind, in fact any tool that uses QEMU 2.30 will probably fail during compilation under Kali.
Overriding these libraries breaks other tools included in Kali so your only solution is to either live with some of Kali's tools being broken, or running another distribution separately such as Ubuntu.

Most tools aren't affected though.

## Adding Tools

To add a tool (say, named *toolname*), do the following:

1. Create a `toolname` directory.
2. Create an `install` script.
3. (optional) if special uninstall steps are required, create an `uninstall` script.

### Install Scripts

The install script will be run with `$PWD` being `toolname`. It should install the tool into this directory, in as contained a manner as possible.
Ideally, full uninstallation should be possible with a `git clean`.

The install script should create a `bin` directory and put its executables there.
These executables will be automatically linked into the main `bin` directory for the repo.
They could be launched from any directory, so don't make assumptions about the location of `$0`!

## License

The individual tools are all licensed under their own licenses.
As for ctf-tools itself, it is licensed under BSD 2-Clause License.
If you find it useful, star it on github (https://github.com/zardus/ctf-tools).

Good luck!

# See Also

There's a curated list of CTF tools, but without installers, here: https://github.com/apsdehal/aWEsoMe-cTf.

There's a Vagrant config with a lot of the bigger frameworks here: https://github.com/thebarbershopper/epictreasure.
