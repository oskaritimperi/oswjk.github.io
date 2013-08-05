---
layout: post
title: Windows XP and Vagrant
---

# {{ page.title }}

Getting Windows XP and Vagrant to play together needs some manual
labour.

General steps are:

- install [WinRM](https://github.com/WinRb/WinRM) (on host)
- install [vagrant-windows](https://github.com/WinRb/vagrant-windows)
- install Windows XP SP3
- install the latest .NET Framework 2.0
- install [Windows Management Framework](http://support.microsoft.com/kb/968929)
- try to make `winrm quickconfig` to work on the guest
- install symlink driver
- create a vagrant base box

## Installing *vagrant-windows*

This should be a simple thing:

    vagrant plugin install vagrant-windows

If your vagrant version is >=1.2.0, and vagrant-windows hasn't yet
released support for vagrant-1.2, then you need to manually do the
installation:

    > git clone https://github.com/WinRb/vagrant-windows.git
    > cd vagrant-windows
    > bundler install
    > rake build
    > vagrant install plugin pkg/vagrant-windows-1.2.0.gem

This is atleast what I had to do. YMMV.

## Make `winrm quickconfig` working

Using the `quickconfig` subcommand you can get the WinRM interface
working quickly. It might work for you the first time, but atleast I
got some *Access Denied* errors. To make it work I had to google for
the error and I found a few different tips you could try:

- try to run the `winrm` command from an Administrator prompt (make sure that
the administrator account password is not empty!)
- run `Set-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Control\Lsa forceguest 0` or
  the same command with `1` at the end
- try [this](http://www.shirmanov.com/2011/04/winrm-access-is-denied-on-local.html) tip
- `winrm set winrm/config/client/auth @{Basic="true"}`

If the tips here do not work, you should be able to find more by
googling.

## Symbolic links

Directions can be found [here](http://schinagl.priv.at/nt/hardlinkshellext/hardlinkshellext.html#symboliclinksforwindowsxp).

After installing, you can create symbolic links to network shares
and so on with the tool `ln.exe`:

    C:\symlink> ln.exe -s "\\remote\share" share_from_remote

## Create a vagrant base box

    vagrant package --base YourWindowsXpVMNameInVirtualBox --output YourWindowsXpVMNameInVirtualBox.box
    vagrant box add YourWindowsXpVMNameInVirtualBox YourWindowsXpVMNameInVirtualBox.box

