# How to remove Snap from Ubuntu 22.04 LTS

## Disable services and uninstall snapd packages

Stop snapd (snap daemon) services:

`$ sudo systemctl disable snapd.service`

`$ sudo systemctl disable snapd.socket`

`$ sudo systemctl disable snapd.seeded.service`

List all the snaps packages installed with the following command:

`$ snap list`

Remove snap packages. It’s best to do so one-by-one, rather than all in one apt remove line. So something like:

`$ sudo snap remove firefox`

`$ sudo snap remove snap-store`

`$ sudo snap remove gtk-common-themes`

`$ sudo snap remove gnome-3-38-2004`

`$ sudo snap remove core18`

`$ sudo snap remove snapd-desktop-integration`


For those who get the error in Ubuntu 22.04:

        $ sudo snap remove --purge firefox
        error: cannot perform the following tasks:
        - Remove data for snap "firefox" (1943) (unlinkat /var/snap/firefox/common/host-hunspell/en_ZA.dic: read-only file system)

Verify that indeed /var/snap/firefox/common/host-hunspell is mounted as an ext4 file system using:

`$ sudo lsblk -fe7 -o+ro`

`$ sudo systemctl stop var-snap-firefox-common-host\\x2dhunspell.mount`

`$ sudo systemctl disable var-snap-firefox-common-host\\x2dhunspell.mount`

        Removed /etc/systemd/system/default.target.wants/var-snap-firefox-common-host\x2dhunspell.mount.
        Removed /etc/systemd/system/multi-user.target.wants/var-snap-firefox-common-host\x2dhunspell.mount.

Delete any snap data cache:

`$ sudo rm -rf /var/cache/snapd/`

Then purge or remove completely snapd using the following command:

`$ sudo apt autoremove --purge snapd`

Optionally delete any files previously created in home directory with the following command:

`$ rm -rf ~/snap`

---
# How to remove Snap from Ubuntu 24.04 LTS

## Disable services and uninstall snapd packages

Stop snapd (snap daemon) services:

`$ sudo systemctl disable snapd.service`
`$ sudo systemctl disable snapd.socket`
`$ sudo systemctl disable snapd.seeded.service`

List all the snaps packages installed with the following command:

`$ snap list`

Remove snap packages. It’s best to do so one-by-one, rather than all in one apt remove line. So something like:

`$ sudo snap remove snap-store`
`$ sudo snap remove gnome-42-2204`
`$ sudo snap remove ubuntu-budgie-welcome` (***specific for ubuntu budgie***)
`$ sudo snap remove snapd-desktop-integration` (***specific for ubuntu budgie***)
`$ sudo snap remove core22`
`$ sudo snap remove bare` (***specific for ubuntu budgie***)
`$ sudo snap remove snapd`
`$ sudo apt autoremove --purge snapd`
`$ sudo rm -rf /var/cache/snapd/`

---
## Reinstalling Firefox with apt

If you want to reinstall Firefox with apt, you may get the following error:

    firefox : PreDepends: snapd but it is not going to be installed

You can get around that by blocking Ubuntu from pulling the snap version of Firefox by pinning it.

First, create a new file:

`$ sudo vim /etc/apt/preferences.d/firefox-no-snap`

Next, add these lines to that new file:

        Package: firefox*
        Pin: release o=Ubuntu*
        Pin-Priority: -1

Save it. Then add the Mozilla team Ubuntu PPA for Firefox:

`$ sudo add-apt-repository ppa:mozillateam/ppa`

Finally, ‘apt update’ and ‘apt install’ the latest Firefox version:

`$ sudo apt update`

`$ sudo apt install firefox`
