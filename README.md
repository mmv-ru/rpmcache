rpmcache
========

This script is intended to delete the rpm and delta files in /var/cache/zypp/packages if you keep these files locally. If you don't know what it means, then you don't need it, as RPM files caching is disable by default for all repositories.

The usage is simple (however that script was a piece of tiny and tricky bash programming). For those who might wonder, the magic key was sort -rV, that performs a sort of version numbers within text. Without this option, this script would have been significantly longer and more complicated. I wrote it a while ago but now that my file server is getting full, I had to test it and ended up rewriting it from scratch (as usual). It seems to be working .. but you'll use it - as always - at your own risk.

The script won't delete files until you set **DEBUG to 0** or comment out this line (in red in the code above). When DEBUG is set to 1, the script will only display the rm commands it would execute. By default, the script will keep the 2 latest versions of the packages. You can change this default by giving the number of versions you want to keep as argument. Only the packages for the local architecture (x86_64 or i[56]86) can be deleted as well as the "noarch" packages, and only the packages for the openSUSE version on which the script is executed. If you keep the packages on a file server and symlink /var/cache/zypp/packages (so do I), you'll have to run the script on each version and on each architecture.

Usage:

* **rpmcache -l --list** : list all the rpm packages
* **rpmcache -d --delete [N]** : list the packages that would be deleted while keeping the N latest versions of the rpms.
* **rpmcache -k --keep [N]** : list the packages that would be kept in cache while keeping the N latest versions of the rpms.
* **rpmcache -o --orphans** : delete rpms of non installed packages only.

Only one option can be used (sorry , I could have combined -d and -o).
These options only display files and won't delete anything (no matter the state of the DEBUG flag).
Obviously, merging the output of rpmcache --delete and rpmcache --keep should match the (sorted) output of rpmcache --list.

Now the destructive options (if DEBUG is commented out or set to 0):
* **rpmcache -D**: delete the rpms but preserve the 2 latest versions.
* **rpmcache -D 1** : delete the rpms but preserve the latest version.
* **rpmcache -D 3** : delete the rpms but preserve the 3 latest versions.
* **rpmcache -D 0** : delete all the rpms ( preserving 0 latest version).
* **rpmcache -D N** : delete the rpms but preserve the N latest versions.
* **rpmcache -O** : delete "orphan" rpms, rpms still in cache for packages that are not installed (most likely because they have been deinstalled).

Delta files - if they are any - will be deleted in any case.

This script is not dangerous. It won't harm your system. However the options -D and -O will delete files that you might not be able to get from anywhere. So these options are "destructive". Use them with caution and after you make sure they'll do what you want (that's what the listing options are for).

to do:

One could add a list of packages that should never be deleted. Some people like to keep older versions of chromium or the nvidia driver for example, and it has be proven to be helpful in the past. I don't need it yet. If you happen to need it now, you're welcome to add this feature. 
