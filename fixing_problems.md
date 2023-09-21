

## Kernels taking up too much space
Inspired heavily by this stack overflow post: https://askubuntu.com/a/1038748

Finding which kernels are installed: `dpkg --list | grep linux-image`

Finding which kernel is currently being used: `uname -a`

Removing an old kernel: `sudo apt purge linux-image-5.4.0-136-generic`

If the disk is full, I fot an error message like:
```
You might want to run 'apt --fix-broken install' to correct these.
The following packages have unmet dependencies:
 linux-image-generic : Depends: linux-image-5.4.0-163-generic but it is not going to be installed
 linux-modules-extra-5.4.0-136-generic : Depends: linux-image-5.4.0-136-generic but it is not going to be installed or
                                                  linux-image-unsigned-5.4.0-136-generic but it is not going to be installed
 linux-modules-extra-5.4.0-163-generic : Depends: linux-image-5.4.0-163-generic but it is not going to be installed or
                                                  linux-image-unsigned-5.4.0-163-generic but it is not going to be installed
E: Unmet dependencies. Try 'apt --fix-broken install' with no packages (or specify a solution).
```
In this case, there is insufficient space in the boot partition to remove a kernel (ironic).
To remove space, we want to manually remove kernel files. Naturally, *be* **extremely** *careful with this step*.

Switch to root: `sudo su`

Navigate to /boot: `cd /boot/`

Delete a couple old kernels that aren't being used: `rm -f *5.4.0-137*`

Fix dependency errors: `apt isntall -f`

Update apt to prep remove `apt update`

Finish removing the kernels manually deleted `apt autoremove`

Install new kernels `apt upgrade`

The stack overflow post suggests running `sudo apt autoremove` to prevent this issue from occuring again, but I'm on a hunt for a better solution.



Then continue to purge old files and update as normal
