repo init -u https://source.codeaurora.org/external/imx/imx-manifest  -b imx-linux-zeus -m imx-5.4.70-2.3.5.xml
repo sync
DISTRO=fsl-imx-xwayland MACHINE=imx8mnevk source imx-setup-release.sh -b build-xwayland
bitbake imx-image-multimedia

error message: 

WARNING: gnu-config-native-20190501+gitAUTOINC+b98424c249-r0 do_fetch: Failed to fetch URL git://git.savannah.gnu.org/config.git, attempting MIRRORS if available

ERROR: gnu-config-native-20190501+gitAUTOINC+b98424c249-r0 do_unpack: Unpack failure for URL: 'git://git.savannah.gnu.org/config.git'. 
No up to date source found: clone directory not available or not up to date: /home/imxharvey/bsp5.4.70-2.3.0/downloads//git2/git.savannah.gnu.org.config.git; 
shallow clone not enabled

ERROR: Logfile of failure stored in: /home/imxharvey/bsp5.4.70-2.3.0/build-xwayland/tmp/work/x86_64-linux/gnu-config-native/20190501+gitAUTOINC+b98424c249-r0/temp/log.do_unpack.35429

ERROR: Task (virtual:native:/home/imxharvey/bsp5.4.70-2.3.0/sources/poky/meta/recipes-devtools/gnu-config/gnu-config_git.bb:do_unpack) failed with exit code '1'

first try: to to .gitconfig to set a predefined source for download as:
url.https://git.savannah.gnu.org/git/config.git/.insteadof=git://git.savannah.gnu.org/config.git
but still get the same error message as above.

Then I change the predefined source as https://git.savannah.gnu.org/gitweb/?p=config.git.insteadof=git://git.savannah.gnu.org/config.git" but, it seems there are no much difference from error message

RROR: gnu-config-native-20190501+gitAUTOINC+b98424c249-r0 do_unpack: Unpack failure for URL: 'git://git.savannah.gnu.org/config.git'. No up to date source found: clone directory not available or not up to date: /home/imxharvey/bsp-5.4.70-2.3.0/downloads//git2/git.savannah.gnu.org.config.git; shallow clone not enabled
ERROR: Logfile of failure stored in: /home/imxharvey/bsp-5.4.70-2.3.0/build-xwayland/tmp/work/x86_64-linux/gnu-config-native/20190501+gitAUTOINC+b98424c249-r0/temp/log.do_unpack.44218
ERROR: Task (virtual:native:/home/imxharvey/bsp-5.4.70-2.3.0/sources/poky/meta/recipes-devtools/gnu-config/gnu-config_git.bb:do_unpack) failed with exit code '1'


Receiving objects: 100% (158102/158102), 14.93 MiB | 32.53 MiB/s, done.
Resolving deltas: 100% (103740/103740), done.
Invalid clone.bundle file; ignoring.
Fetching: 33% (4/12) meta-freescale-3rdparty
fatal: unable to update url base from redirection:
  asked for: http://git.openembedded.org/meta-python2/info/refs?service=git-upload-pack
   redirect: http://cn-sha02-fw01-trust-nxn.nxn.nxp.com:6080/php/browser_challenge.php?vsys=1&rule=6&url=http://git.openembedded.org%2fmeta-python2%2finfo%2frefs%3fservice%3dgit-upload-pack

meta-python2:
fatal: unable to update url base from redirection:
  asked for: http://git.openembedded.org/meta-python2/info/refs?service=git-upload-pack
   redirect: http://cn-sha02-fw01-trust-nxn.nxn.nxp.com:6080/php/browser_challenge.php?vsys=1&rule=6&url=http://git.openembedded.org%2fmeta-python2%2finfo%2frefs%3fservice%3dgit-upload-pack
meta-python2: sleeping 4.0 seconds before retrying
fatal: unable to update url base from redirection:
  asked for: http://git.openembedded.org/meta-python2/info/refs?service=git-upload-pack
   redirect: http://cn-sha02-fw01-trust-nxn.nxn.nxp.com:6080/php/browser_challenge.php?vsys=1&rule=6&url=http://git.openembedded.org%2fmeta-python2%2finfo%2frefs%3fservice%3dgit-upload-pack

meta-python2:
fatal: unable to update url base from redirection:
  asked for: http://git.openembedded.org/meta-python2/info/refs?service=git-upload-pack
   redirect: http://cn-sha02-fw01-trust-nxn.nxn.nxp.com:6080/php/browser_challenge.php?vsys=1&rule=6&url=http://git.openembedded.org%2fmeta-python2%2finfo%2frefs%3fservice%3dgit-upload-pack
error: Cannot fetch meta-python2 from git://git.openembedded.org/meta-python2
Fetching: 100% (12/12), done in 56.413s
Garbage collecting: 100% (12/12), done in 0.112s

fatal: unable to update url base from redirection:
  asked for: http://git.openembedded.org/meta-python2/info/refs?service=git-upload-pack
   redirect: http://cn-sha02-fw01-trust-nxn.nxn.nxp.com:6080/php/browser_challenge.php?vsys=1&rule=6&url=http://git.openembedded.org%2fmeta-python2%2finfo%2frefs%3fservice%3dgit-upload-pack

meta-python2:
fatal: unable to update url base from redirection:
  asked for: http://git.openembedded.org/meta-python2/info/refs?service=git-upload-pack
   redirect: http://cn-sha02-fw01-trust-nxn.nxn.nxp.com:6080/php/browser_challenge.php?vsys=1&rule=6&url=http://git.openembedded.org%2fmeta-python2%2finfo%2frefs%3fservice%3dgit-upload-pack
meta-python2: sleeping 4.0 seconds before retrying
fatal: unable to update url base from redirection:
  asked for: http://git.openembedded.org/meta-python2/info/refs?service=git-upload-pack
   redirect: http://cn-sha02-fw01-trust-nxn.nxn.nxp.com:6080/php/browser_challenge.php?vsys=1&rule=6&url=http://git.openembedded.org%2fmeta-python2%2finfo%2frefs%3fservice%3dgit-upload-pack

meta-python2:
fatal: unable to update url base from redirection:
  asked for: http://git.openembedded.org/meta-python2/info/refs?service=git-upload-pack
   redirect: http://cn-sha02-fw01-trust-nxn.nxn.nxp.com:6080/php/browser_challenge.php?vsys=1&rule=6&url=http://git.openembedded.org%2fmeta-python2%2finfo%2frefs%3fservice%3dgit-upload-pack
error: Cannot fetch meta-python2 from git://git.openembedded.org/meta-python2
Fetching: 100% (1/1), done in 1m24.556s
Garbage collecting: 100% (1/1), done in 0.005s
fatal: failed to unpack tree object HEAD
error.GitError: Cannot checkout meta-python2: Cannot initialize work tree for meta-python2
error: Cannot checkout meta-python2
Checking out: 100% (12/12), done in 0.957s

error: Unable to fully sync the tree.
error: Downloading network changes failed.
error: Checking out local projects failed.
Failing repos:
sources/meta-python2
Try re-running with "-j1 --fail-fast" to exit at the first error.
