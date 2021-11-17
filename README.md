Simple application to read current clocks of ATi Radeon cards (xf86-video-ati, xf86-video-amdgpu).

# xf86-video-ati and xf86-video-amdgpu driver

Install and run radeon-profile-daemon (https://github.com/spezifisch/radeon-profile-daemon) for using this app as normal user. Otherwise app need to be run with root privileges for changing power profiles (and clocks readings sometimes). You can add `username ALL = NOPASSWD: /usr/bin/radeon-profile` to your `/etc/sudoers`. Here is tip for run app as normal user but involves change permissions to system files: http://bit.ly/1dvQMhS

# Fork

This fork of https://github.com/marazmista/radeon-profile includes:

* changed socket path for improved security from https://github.com/marazmista/radeon-profile/pull/246
* suspend fix from https://github.com/marazmista/radeon-profile/pull/236
* debian package build scripts based on [upstream launchpad PPA](https://launchpad.net/~radeon-profile/+archive/ubuntu/stable)

Tested with Ubuntu 20.04.

# Build and install Debian/Ubuntu package

Prerequisite packages:

* debuild
* devscripts
* equivs

Install build dependencies:

```
sudo mk-build-deps --install
```

*Note:* If you don't want to sign the package (when only installing it locally), add the following to `~/.devscripts`:

```
DEBUILD_DPKG_BUILDPACKAGE_OPTS="-us -uc -ui"
```

Build the package:

```
debuild
```

**Dependency note:** The forked [radeon-profile-daemon](https://github.com/spezifisch/radeon-profile-daemon) package needs to be installed before installing this package (radeon-profile) as it's a dependency for this package.

Then install with:

```
sudo dpkg -i ../radeon-profile_*.deb
```

# Functionality

* Monitoring of basic GPU parameters (frequencies, voltages, usage, temperature, fan speed)
* DPM profiles and power levels
* Fan control (HD 7000+), definition of multiple custom curves or fixed speed
* Overclocking (amdgpu) (Wattman, Overdrive, PowerPlay etc)
* Per app profiles/Event definitions (i.e. change fan profile when temp above defined or set DPM to high when selected binary executed)
* Define binaries to run with set of environment variablees (i.e. GALLIUM_HUD, MESA, LIBGL etc)

# Dependencies

* Qt 5.6<= (qt5-base and qt5-charts) (On Debian/Ubuntu: `qt5-default libqt5charts5-dev`)
* libxrandr
* libdrm (for amdgpu, 2.4.79<=, more recent, the better)
* recent kernel (for amdgpu 4.12<=, more recent, the better)
* radeon card

For full functionality:
* glxinfo - info about OpenGL, mesa
* xdriinfo - driver info
* xrandr - connected displays

# Build

```
git clone https://github.com/spezifisch/radeon-profile.git
cd radeon-profile/radeon-profile
qmake
make 
```

The resulting binary is `./target/radeon-profile`

For Ubuntu 17.04, qt5-charts isn't available:
* Use `qtchooser -l` to list available profiles
* Use `qmake -qt=[profile from qtchooser]` to specify Qt root or download and install a Qt bundle from https://www.qt.io/download-open-source/#section-2
* Make a `qt5opt.conf` in `/usr/lib/x86_64-linux-gnu/qtchooser/` containing:

```
/opt/Qt5.9.1/5.9.1/gcc_64/bin
/opt/Qt5.9.1/5.9.1
```

# Installation

**Note:** These instructions are for the upstream repository, not for this fork.

### Ubuntu 
Available for Ubuntu from PPA [stable](https://launchpad.net/~radeon-profile/+archive/ubuntu/stable) and [git develop](https://launchpad.net/~radeon-profile/+archive/ubuntu/radeon-profile) repository

Add in terminal commands:

* For git ppa: 
```
sudo add-apt-repository ppa:radeon-profile/radeon-profile
```
* For stable ppa: 
```
sudo add-apt-repository ppa:radeon-profile/stable
```
* Then run commands:
```
sudo apt update
sudo apt install radeon-profile
```

### Arch Linux
* AUR package: https://aur.archlinux.org/packages/radeon-profile-git/
* System daemon AUR package: https://aur.archlinux.org/packages/radeon-profile-daemon-git/

# Links

* System daemon: https://github.com/spezifisch/radeon-profile-daemon
* Sort of official thread: http://phoronix.com/forums/showthread.php?83602-radeon-profile-tool-for-changing-profiles-and-monitoring-some-GPU-parameters
* Icon: http://proicons.deviantart.com/art/Graphics-Cards-Icons-H1-Pack-161178339

# Screenshot

Main screen
![Main screen](https://i.imgur.com/Z880p47.png)

[More screenshots](http://imgur.com/a/DMRr9)
