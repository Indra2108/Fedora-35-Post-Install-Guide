# Fedora 36 Post Install Guide
Things to do after installing Fedora 36

## Faster Updates
* `sudo nano /etc/dnf/dnf.conf` 
* Copy and replace the text with the following:
```
[main] 
gpgcheck=1 
installonly_limit=3 
clean_requirements_on_remove=True 
best=False 
skip_if_unavailable=True 
fastestmirror=1 
max_parallel_downloads=10 
deltarpm=true 
```

## RPM Fusion release
* Fedora has disabled the repositories for non-free .rpm software by default. Follow this if you use non-free software like discord and some multimedia codecs etc. As a general rule of thumb its advised to do this unless you absolutely don't want any non-free software on your system.
* `sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm`
* also while you're at it, install app-stream metadata by
* `sudo dnf groupupdate core`

## Update 
* `sudo dnf -y upgrade --refresh`
* Reboot

## Media Codecs
* Install these to get proper video playback.
```
sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin`

sudo dnf groupupdate sound-and-video
```

* Source: https://docs.fedoraproject.org/en-US/quick-docs/assembly_installing-plugins-for-playing-movies-and-music/
```
sudo dnf install gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel

sudo dnf install lame\* --exclude=lame-devel

sudo dnf group upgrade --with-optional Multimedia
```

## Firefox H/W Video Acceleration
* Helps decrease load on the CPU when watching videos on youtube by alloting the rendering to the dGPU/iGPU. Quite helpful in increasing battery backup on laptops.
* Change the following setting in about:config
* `media.ffmpeg.vaapi.enabled  true`
* `layers.acceleration.force-enabled : true`

## Update Flatpak
* `flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`
* `flatpak update`

## Set Hostname
* `hostnamectl set-hostname YOUR_HOSTNAME`

## Speed Boost
* Install Grub Customizer to implement these tweaks by
* `sudo dnf install grub-customizer` 

### Disable Mitigations 
* Increases performance in multithreaded systems. The more core count you have the greater the performance gain. Not advised for host systems on some networks for increased security vulnerabilities, using it on daily driver systems won't fetch any problems. 5-30% performance gain varying upon systems.
* Add `mitigations=off` in Kernel Parameters under General Settings in Grub Customizer and click save.

### Zswap (for systems with <16 gigs of RAM)
* Acts as virtual memory. Useful for sytems with <16 gigs of ram.
* Add `zswap.enabled=1` in Kernel Parameters under General Settings in Grub Customizer and click save.
