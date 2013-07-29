Title: OpenWRT for ZipIt Z2 handhelds
CSS: markdown.css

# OpenWRT for ZipIt Z2

This is an unofficial port of the [OpenWRT](http://www.openwrt.org/) Linux distribution for the [ZipIt Z2](http://linux.zipitwireless.com) handheld.

<p class="centered"><img src="zipit-openwrt.jpg" alt="ZipIt Z2 running OpenWRT" /></p>

**Note that this is an unofficial port of "OpenWRT trunk" at this stage.**

# News
* 23 April 2013 - Nightly builds re-enabled again (had stopped in November.) Added some notes about alternative installations, package sources.

* 18 March 2012 - Package snapshots should be back & current now, after being incomplete/old for a few weeks.

* 29 June 2012 - "ZipIt" packages feed has been moved to openwrt-zipit-packages (no longer forked from Qi Hardware.) Development can be found [on github](https://github.com/openwrt-zipit).


# Downloads

Two images you can install to an SD card (see instructions, below):

* [Nightly build for SD card, with console](http://chainxor.org/openwrt-zipit/snapshot/openwrt-pxa-zipitz2-rootfs.tar.gz)

* [Mozzwald has a recent build with the gmenu2x launcher](http://mozzwald.homelinux.net/zipit/index.php?dir=openwrt%2Fpxa)

<p class="centered"><img src="gmenu2x.png" alt="Gmenu2x launcher, courtesy slug_" /></p>

See below for instructions.

# Internal Install

There is also this image for installation to the internal flash:

[slug_'s recent build with gmenu2x launcher, for installing to internal flash](http://blog.engine12.com/?p=546). 

The tool [z2uFlashstock](http://mozzwald.homelinux.net/node/174) will install this build (and uboot bootloader) onto a stock ZipIt Z2.  You can also use it to update the internal jffs on a uboot Zipit.

# Packages

Packages are built nightly on this site, [directory here](http://chainxor.org/openwrt-zipit/snapshot/packages). The nightly build comes with [opkg](http://wiki.openwrt.org/doc/techref/opkg) configured to install packages automatically from this repository.

Mozzwald's image with gmenu2x is configured to use his [package repository](http://mozzwald.homelinux.net/zipit/openwrt/pxa/packages). There may be packages available in this repository which have failed in the nightly build.

If you wish to switch between the package repositories used by *opkg update*, you can edit the file /etc/opkg.conf on the ZipIt. URLs are

* *http://chainxor.org/openwrt-zipit/snapshot/packages* (nightlies)

* *http://mozzwald.homelinux.net/zipit/openwrt/pxa/packages* (mozzwald's.)


## Pros

* Very lightweight, OpenWRT is designed for low end devices.

* Nearly 3000 [prebuilt binary packages](snapshot/packages) available via 'opkg' package manager.

* The 4.52b base install includes wifi support, ssh server, ZipIt-friendly console. By trimming it's possible to get down to 2Mb or less for a base system.

* A good base for building a more feature-heavy ZipIt installation.


## Cons

* Not as many packages as Debian [z2sid](http://mozzwald.com/z2sid). In particular, not as many graphical packages (some have been ported from the [Ben Nanonote OpenWRT port](https://github.com/projectgus/qi-openwrt-packages).)

* OpenWRT is officially a router firmware (although it runs on lots of other devices), so some aspects are more router-friendly than handheld-friendly.

## Requirements

* A ZipIt Z2 and a spare MiniSD card.

* uboot already installed. If your ZipIt is stock, I recommend downloading the latest version of [FlashStock](http://russelldavis.org/2011/02/13/flashstock-v0-1/) from [Mozzwald's downloads page](http://mozzwald.com/download). Or try [z2uFlashstock](http://mozzwald.homelinux.net/node/174).

## Installing to an SD card

* Set up your miniSD card with a single ext2 partition.

* Download the [latest build snapshot tarball](snapshot/openwrt-pxa-zipitz2-rootfs.tar.gz).

* Untar the tarball as root onto the SD card (be sure to run 'sync' when done.)

* Edit the file etc/config/wireless on the miniSD card and follow the prompts to preconfigure the wireless (easier than doing it on the device.)

* Eject the SD, then insert it into the ZipIt and boot. Press "Enter" to start a new console as root.


## Getting Started

* After you boot, use 'passwd' to set a root password. **This is important: until you do this, passwordless telnet will be available to the ZipIt**.

* After you set a root password, you will be able to ssh to the ZipIt.

* Use "opkg update" to refresh the list of [packages](snapshot/packages). Use "opkg install *packagename*" to install one.

* For tons more information, see the [OpenWRT Wiki](http://wiki.openwrt.org/).

## Useful Things

Because this is an early image, most things will need some experimentation to get right. Here are some things to get you started, though:

### Wifi & Network Configuration

Edit the files /etc/config/network & /etc/config/wireless to set up a wifi client, there are comments to guide you.

For a complete reference, check the [OpenWRT wireless network docs](http://wiki.openwrt.org/doc/uci/wireless).

### Menu/Launcher

Anarsoul has ported gmenu2x. *opkg install gmenu2x* to use. The ZipIt will automatically boot into gmenu2x if it is installed.

### Audio
*opkg install kmod-sound-zipit-z2 alsa-utils* to get Linux audio support.

To hear audio, run 'alsamixer' then scroll right until you see "Left Mixer" then "Right Mixer" and press 'm' to unmute them. It'd be nice if someone makes a little alsactl script to automate this, we can put it in the kmod-sound-zipitz2 package. :)

To exit alsamixer, type Ctrl-C (the ... & C keys.)

One command line audio player is moc. *opkg install moc* then *mocp* to run.

### Email
[mutt](http://www.mutt.org) is a great console email client. *opkg install mutt msmtp* will give you mutt w/ secure IMAP & POP
 support, and you can [configure msmtp](http://msmtp.sourceforge.net/doc/mutt+msmtp.txt) as a secure sendmail for SMTP.

### Mouse support

*kmod install mouse-emul* and you can press 'Option' to toggle using the keypad as a mouse.


## Source Code & Getting Involved

* OpenWRT for ZipIt development all on [github](https://github.com/openwrt-zipit).

* The [openwrt-zipit](https://github.com/openwrt-zipit/openwrt-zipit) repository has the base system. [Anarsoul has written a guide to getting started with building from source](http://anarsoul.blogspot.com.au/2012/02/openwrt-for-zipit-z2.html) (he's using his git repositories, you can either use these or replace them.) The [OpenWRT build documentation](http://wiki.openwrt.org/about/toolchain) is also very complete.

* [openwrt-zipit-packages](https://github.com/openwrt-zipit/openwrt-zipit-packages) is a ZipIt-specific fork of new user interface packages. Thank to [mozzwald](http://mozzwald.com) for starting this repository. Many packages were originally ported by Qi Hardware for the Ben Nanonote.

* Contributors are extremely welcome, just fork on github if you want to get hacking!

If you're wondering what is left to do, there are still several of the Qi Hardware packages that don't build for ZipIt. Or take a look at the [Issues List](https://github.com/projectgus/openwrt-zipit/issues) for OpenWRT-ZipIt. Or, port your favourite program!

Please send us Pull Requests or patches via github so changes can be incorporated.

## About the nightly builds

Should run once a day and attempt to build all packages in the openwrt-zipit repositories. Logs and the build script [are uploaded after each build](http://chainxor.org/openwrt-zipit/snapshot/)


## Credits

Worked on by many [#zipit](http://webchat.freenode.net/?channels=zipit) folks - projectgus, anarsoul, mozzwald, deeice & slug_, and many others.

Big thanks to the OpenWRT developers for the original distribution.

## I Found A Bug

If it pertains to the core OS, please [report it via the github Issues for openwrt-zipit](https://github.com/openwrt-zipit/openwrt-zipit/issues).

If it pertains to a package taken from openwrt-zipit-packages, please [report it via that project's Issues page](https://github.com/openwrt-zipit/openwrt-zipit-packages/issues).

If it is another program from the core OpenWRT distribution, you can either report it on github or report it back to the OpenWRT team. Bear in mind that we are grabbing packages from OpenWRT development versions, so it is likely packages will break from time to time. Also please remember that ZipIt is not a supported configuration for OpenWRT, so they may not be familiar with your configuration.


<!--- Piwik -->
<script type="text/javascript">
var pkBaseURL = (("https:" == document.location.protocol) ? "https://stats.projectgus.com/" : "http://stats.projectgus.com/");
document.write(unescape("%3Cscript src='" + pkBaseURL + "piwik.js' type='text/javascript'%3E%3C/script%3E"));
</script>
<script type="text/javascript">
try {
var piwikTracker = Piwik.getTracker(pkBaseURL + "piwik.php", 1);
piwikTracker.trackPageView();
piwikTracker.enableLinkTracking();
} catch( err ) {}
</script>
<noscript><p><img src="http://stats.projectgus.com/piwik.php?idsite=1" style="border:0" alt="" /></p>
</noscript>
<!--- End Piwik Tracking Code -->
