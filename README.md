# Specifics of Nix package manager on Mac OS X 10.10 (Darwin)

## Run

```
curl https://nixos.org/nix/install | sh
source $HOME/.nix-profile/etc/profile.d/nix.sh
cd ~
git clone https://github.com/copumpkin/nixpkgs.git
cd ~/.nix-defexpr
rm -rf channels
ln -s ~/nixpkgs nixpkgs
```

Notice that I am using Daniel Peebles' fork of https://github.com/NixOS/nixpkgs.  This is because the current head of NixOS' nixpkgs is broken for darwin (Mac OS X). Daniel's `pure-darwin` fork contains all the necessary patches to build on Darwin correctly.  This is the current state as of 6th June 2015.  When a merge back into NixOS' nixpkgs happens, we can then switch to using that our machine.

## Check

```
nix-env -q
```

We should see these listed:

```
cacert-20140715
nix-1.8
```

Also, our `$NIX_PATH` should reflect that the `nixpkgs` path is available.

```
$ echo $NIX_PATH
nixpkgs=/Users/calvin/.nix-defexpr/channels/nixpkgs
```

## Use binary caches?

```
sudo mkdir /etc/nix
sudo vim /etc/nix/nix.conf
```

Add in the following lines in `nix.conf`:

```
binary-caches = http://zalora-public-nix-cache.s3-website-ap-southeast-1.amazonaws.com/ http://cache.nixos.org/ http://hydra.nixos.org/
```

## Let's install something

```
$ nix-env -i hello
```

## Haskell

To search for haskell packages, we use the `-A` flag.  Like this:

```
$ nix-env -qaP -A nixpkgs.haskellPackages | grep cabal-install
nixpkgs.haskellPackages.cabal-install                                cabal-install-1.22.4.0
nixpkgs.haskellPackages.cabal-install-bundle                         cabal-install-bundle-1.18.0.2.1
nixpkgs.haskellPackages.cabal-install-ghc72                          cabal-install-ghc72-0.10.4
nixpkgs.haskellPackages.cabal-install-ghc74                          cabal-install-ghc74-0.10.4
```

Simply querying with `nix-env -qaP` will not yield any result because there are too many sub-packages to be included into the top level search path. So, we use the `-A` flag to specify the attribute path where we begin our search and pipe the results of all the packages in that sub-attribute path for filtering by `grep`.

```
$ nix-env -qaP -A nixpkgs.haskellPackages | grep cabal2nix
nixpkgs.haskellPackages.cabal2nix                                    cabal2nix-20150531
```

`cabal2nix` and `cabal-install` are the programs we need in our nix user environment. So let's get them installed first:

```
$ nix-env -iA nixpkgs.haskellPackages.cabal-install
...
$ nix-env -iA nixpkgs.haskellPackages.cabal2nix
...

```

Once done, let's review what we have in our Mac OS X's nix user environment:

```
$ nix-env -q
cabal-install-1.22.3.0
cabal2nix-20150531
cacert-20140715
hello-2.10
nix-1.8
```


