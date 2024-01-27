---
title: Nix
description: 
published: true
date: 2024-01-27T12:25:41.996Z
tags: work
editor: markdown
dateCreated: 2024-01-27T12:25:41.996Z
---

# [install](https://nix.dev/install-nix)
```bash
	  curl -L https://nixos.org/nix/install | sh
```

# quick test using temporary shell
```bash
	  # install a package
	  nix-shell -p <package-name>
	  
	  # install a package and run a bash command
	  nix-shell -p <package-name> --run "<package-command>"
	  
	  # example with bash command
	  nix-shell -p git --run "git --version"
	  
	  # example with specific nix version
	  nix-shell -p git --run "git --version" -I nixpkgs=https://github.com/NixOS/nixpkgs/archive/2a601aafdc5605a5133a2ca506a34a3a73377247.tar.gz
```

# cleanup packages
```bash
	  nix-collect-garbage
```

# [create re-useable script](https://nix.dev/tutorials/first-steps/reproducible-scripts)
- Create a file named `nixpkgs-releases.sh` with the following content
```bash
		  #!/usr/bin/env nix-shell
		  #! nix-shell -i bash --pure
		  #! nix-shell -p bash cacert curl jq python3Packages.xmljson
		  #! nix-shell -I nixpkgs=https://github.com/NixOS/nixpkgs/archive/2a601aafdc5605a5133a2ca506a34a3a73377247.tar.gz
		  - curl https://github.com/NixOS/nixpkgs/releases.atom | xml2json | jq .
```
- The first line is a standard shebang.
The additional shebang lines are a Nix-specific construct.  
- We specify `bash` as the interpreter for the rest of the file with the `-i` option.
- We enable the `--pure` option to prevent the script from implicitly using programs that may already exist on the system that will run the script.
- With the `-p` option we specify the packages required for the script to run.
- The command `xml2json` is provided by the package `python3Packages.xmljson`, while `bash`, `jq`, and `curl` are provided by packages of the same name. `cacert` must be present for SSL authentication to work. Use [search.nixos.org](https://search.nixos.org/packages) to find packages providing the program you need.  
- The parameter of `-I` refers to a specific Git commit of the Nixpkgs repository.
		  This ensures that the script will always run with the exact same packages versions, everywhere.  
- Make the script executable:
```bash
		  chmod +x nixpkgs-releases.sh
```
- Run the script:
```bash
		  ./nixpkgs-releases.sh
```

# [create re-useable shell environment](https://nix.dev/tutorials/first-steps/declarative-shell)
- Create a file called `shell.nix` with these contents
```
let
  nixpkgs = fetchTarball "https://github.com/NixOS/nixpkgs/tarball/nixos-22.11";
  pkgs = import nixpkgs { config = {}; overlays = []; };
in  
pkgs.mkShell {
  packages = with pkgs; [
    git
    neovim
    nodejs
  ];
}
```
- Enter the environment by running `nix-shell` in the same directory as `shell.nix`
```bash
		  nix-shell
```
- `nix-shell` by default looks for a file called `shell.nix` in the current directory and builds a shell environment from the Nix expression in this file.
		  Packages defined in the packages attribute will be available in `$PATH`.  
- Export certain environment variables when you enter a shell environment. For example: Set your `GIT_EDITOR` to use the `nvim` from the shell environment:
```
pkgs.mkShell {
  packages = with pkgs; [
    git
    neovim
    nodejs
  ];
		  
  GIT_EDITOR = "${pkgs.neovim}/bin/nvim";
}
```
- Any attribute name passed to `mkShell` that is not reserved otherwise and has a value which can be coerced to a string will end up as an environment variable.
- Run some commands before entering the shell environment. These commands can be placed in the `shellHook` attribute provided to `mkShell`.
```
pkgs.mkShell {
  packages = with pkgs; [
    git
    neovim
    nodejs
  ];
		  
  GIT_EDITOR = "${pkgs.neovim}/bin/nvim";
		  
  shellHook = ''
    git status
  '';
}
```

- TODO: https://nix.dev/tutorials/nix-language
