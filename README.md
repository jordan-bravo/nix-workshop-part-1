# Nix Workshop Part 1: The Nix Package Manager
Notes for part 1-of-2 of my introductory Nix workshop

Link to the recording: https://youtu.be/glQoiK5DOZY

# Overview

## What you'll learn today:
- What is Nix and what you can do with it.
- How to get started doing it.
- How to find info to learn more (this is important)

# What is Nix?
- Package manager & build tool
- Configured using the Nix language
- NixOS is a Linux distro built from the ground up with the Nix package manager & build tool

## What you'll learn in part 2:
- How is using NixOS different than using Nix on MacOS/Linux?
- How to install and configure NixOS
- Help with your configuration.

# Why use Nix?

We'll focus on three use cases
- Developer environments
- Configure every aspect of your computer across any platform
- "Unbreakable" system administration (NixOS)

# Tradeoffs
- there's a lot to learn and it takes time (for example, functional languages syntax is unfamiliar to many.
- documentation is scattered and disorganized.
- increased storage requirements (not an issue for modern desktops/laptops but could be an issue on resource-constrained systems)

## Where does nix get its packages?
- `nixpkgs`, pronounced "nix packages", is main repo of the project.
- it's the largest in the world by far, with over 100,000 packages.
- https://repology.org/repositories/graphs

![repo-sizes](https://github.com/jordan-bravo/nix-workshop-part-1/assets/62706808/5cd40559-f9c0-43d5-ae3c-1893fd02d629)


## What about stuff not in nixpkgs?
- anyone can package software for nix.
- anyone can submit their package to nixpkgs.

# Interactive demo

# What you'll learn


## Lesson 1: installing nix package manager
- install nix from here: https://zero-to-nix.com/start/install
```
❯ curl --proto '=https' --tlsv1.2 -sSf -L https://install.determinate.systems/nix | sh -s -- install
```
- Open a new terminal session and the nix executable should be in your $PATH. To verify that:
```shell
❯ nix --version
```
This should print the version information for Nix.

## Lesson 2: Run a program without installing it
```
❯ nix run nixpkgs#cowsay -- ATL BitLab FTW
```





```
❯ nix run nixpkgs#python39
```

## Lesson 666: Install packages imperatively (DANGER)
##### What's the difference between imperative vs. declarative?

![declarative-comparison](https://github.com/jordan-bravo/nix-workshop-part-1/assets/62706808/0afa282b-f4c1-47eb-adf0-d00f8c0a1693)

![declarative-comparison-analogy](https://github.com/jordan-bravo/nix-workshop-part-1/assets/62706808/c7679e24-5cf3-4b8b-923e-d35e65f72249)

##### How to install packages imperatively (USE SPARINGLY)
```
❯ nix profile install nixpkgs#hello
```

```
❯ nix profile list
# other stuff omitted
Index:              1
Flake attribute:    legacyPackages.x86_64-linux.hello
# other stuff omitted
```

```
❯ nix profile remove 1
removing 'flake:nixpkgs#legacyPackages.x86_64-linux.hello'
```

```
❯ nix profile list
# nothing is listed
```


## Lesson 3: Install packages declaratively (This is the way)
- The Nix ecosystem has a program called home-manager that allows you to declarative manage your home directory, apps, and configs.
- Initialize home-manager:
```
nix run home-manager/master -- init --switch
```
- It will have created a `home-manager` directory in `~/.config`
- Open up `~/.config/home-manager/home.nix`
- Locate the section with `home.packges`.  It's a list with package names separated by spaces (or new lines).
- A good tool for searching the nixpkgs repo is https://search.nixos.org. Add/remove desired packages. Prefix package names with `pkgs.` 
- Example of packages? 
	- pkgs.curlie
	- pkgs.audacity	```
curlie https://jsonplaceholder.typicode.com/posts

- Save the file then run
```
❯ home-manager switch
```
- Nix evaluates and builds your configuration.  Now you have the packages and settings you specified.
#### Tip: reclaim storage space by clearing out inactive packages with `nix-collect-garbage`
## Lesson 4: Using dev environments (flakes)
- In the repo you want to turn into a flake, you'll create a file called `flake.nix`.
- For learning purposes, use this template:  https://github.com/jordan-bravo/nix-flake-template
- Fork it or download the `flake.nix` file.  The `README.md` might be helpful too.
- Edit the `flake.nix` as desired and save.  Now run:
```
❯ nix develop
```
- When finished, type `exit`

## Lesson 5: How to learn more about Nix

### Advice:  Use the new way, avoid the old way
- This is an opinionated workshop: it guides you into a particular way of doing thing.
- In Nix, there is an old way of doing things and a new way of doing things.  The new way is called Nix Flakes.
- Why are there two ways?
	- Hard to get consensus in an open source project, and therefore flakes have had the "experimental" flag for many years, but they're actually stable.
	- Some OG nixers are dragging their feet.
	- Lots of momentum and support for the new way.
- Analogous to Bitcoin, hard to get consensus, e.g. SegWit, Lightning, Taproot, these things are considered controversial by some.
- How to tell the difference between old way and new way with Nix flakes?
- New commands (with flakes) have a space between the word `nix` and the command
	- E.g. `nix run`, `nix develop`, `nix flake`, etc.
- Old commands are hyphenated:
	- E.g. `nix-shell`, `nix-env`, etc.
	- Some notable exceptions such as `nix-collect-garbage`

## Resources
- Install Nix from here: https://zero-to-nix.com/start/install
- Search here for packages available in nixpkgs: https://search.nixos.org/
- Search here for config options in home-manager:  https://mipmip.github.io/home-manager-option-search/
- Home-manager manual: https://nix-community.github.io/home-manager/
- Get help in 
    - Official Matrix chat: https://matrix.to/#/#nix:nixos.org
    - Unofficial Discord chat: https://discord.com/invite/RbvHtGa
- Search here for older versions of packages:  https://www.nixhub.io/
- Watch this talk on why Nix Flakes are the way forward: https://youtu.be/wZBiRv3ixhU
NixOS:
- Read this to learn, focuses on NixOS: https://nixos-and-flakes.thiscute.world
- Watch this to learn how to configure Nix (with Home-Manager) and/or NixOS: https://youtu.be/a67Sv4Mbxmc

Recording of this Nix workshop (Part 1): https://www.youtube.com/watch?v=glQoiK5DOZY

## Bonus: Lesson 6: What's going on under the hood?
- How does Nix maintain multiple versions of packages on our system and activate the one we specify?
- Nix creates a top-level directory on your system called `/nix`, and within that directory is a directory at `/nix/store`.  This is called the "Nix store" and it's where Nix stores everything.
- Take a look in this directory and list the contents (be patient, there's a lot of content and it might take a few moments to load):
```
❯ ls -l /nix/store
```
You'll see something like this (names will differ):
```
.r--r--r-- root root  2.8 KB Wed Dec 31 19:00:01 1969 zzs2771mlvh5yy8a6a1m2nld5nzy1zzs-nss-3.94.tar.gz.drv
.r--r--r-- root root  2.8 KB Wed Dec 31 19:00:01 1969 zzvadvfdyg749b5agfhk24w613zdkzgn-opencore-amr-0.1.6.tar.gz.drv
dr-xr-xr-x root root  4.0 KB Wed Dec 31 19:00:01 1969 zzwip5arfv7r4jvv9rdv8rkixqxpd5fi-vimplugin-cmp-nvim-lsp-2023-12-10
dr-xr-xr-x root root  4.0 KB Wed Dec 31 19:00:01 1969 zzwsixlsccw8vdy6wb9qqzj1fmyjis3n-net-tools-2.10
```

Notice a few things:
1. All the files and directories start with a long string of characters.  The leading string is a hash of all of the inputs of everything required to build a package.  If a single bit changes, the hash changes, and therefor it would have a new name and path.  This is how Nix can keep multiple versions of a package side-by-side without name collisions.
2. All the dates have been set to Dec 31 19:00:01 1969.  Nix deliberately sets this mock timestamp.  This is because when software is compiled, every input changes the output, including the operating system's current time.  If Nix didn't keep the date fixed, then the output would change based on the time of compilation, which would impede reproducability.
Assuming `cowsay` is installed (see previous lessons on installing), look at the following:
```
❯ which cowsay
# /home/jordan/.nix-profile/bin/cowsay
```
Nix adds the directory `~/.nix-profile/bin` to your shell's `PATH` variable, so that any executable binaries in that directory are available on the command line.  Take a look in that directory:
```
❯ ls -l ~/.nix-profile/bin
```

You'll see a bunch of files (you'll likely have different ones)
```
lrwxrwxrwx root root 83 B Wed Dec 31 19:00:01 1969 python3.11-config ⇒ /nix/store/c6dp2m0bjggi61z65rm9gy29va8gc38i-home-manager-path/bin/python3.11-config
lrwxrwxrwx root root 77 B Wed Dec 31 19:00:01 1969 qbittorrent ⇒ /nix/store/c6dp2m0bjggi61z65rm9gy29va8gc38i-home-manager-path/bin/qbittorrent
lrwxrwxrwx root root 70 B Wed Dec 31 19:00:01 1969 qvlc ⇒ /nix/store/c6dp2m0bjggi61z65rm9gy29va8gc38i-home-manager-path/bin/qvlc
lrwxrwxrwx root root 70 B Wed Dec 31 19:00:01 1969 racc ⇒ /nix/store/c6dp2m0bjggi61z65rm9gy29va8gc38i-home-manager-path/bin/racc
lrwxrwxrwx root root 70 B Wed Dec 31 19:00:01 1969 rake ⇒ /nix/store/c6dp2m0bjggi61z65rm9gy29va8gc38i-home-manager-path/bin/rake
```

We can see that the files in `~/.nix-profile/bin` are symbolic links, a.k.a. symlinks that point to derivations in the Nix store located in `/nix/store`.  When we tell Nix to install something, it downloads and builds it in the Nix store, then adds a symlink to it in our `~/.nix-profile/bin` directory, which has been added to `PATH`, and that is how our system knows where to find it when we type the name of our program such as `cowsay`.
