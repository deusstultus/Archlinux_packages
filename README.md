# Archlinux_packages
Arch linux packages tree

This repository built using subtrees to preserve history without the overhead of cloning the whole tree.

Usage for duplicating concept:

###Set Remotes  
First package from remote: `git remote add arch-community <upstream URL> -t packages/<package name>`  
Subsequent packages: `git remote set-url --add arch-community packages/<package name>

###Import subtree  
```
git checkout <package ref>
git subtree split -P trunk -b packages/<package name>
git checkout master
git subtree add -P <package name> packages/<package name>
```
