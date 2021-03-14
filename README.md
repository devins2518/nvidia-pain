# nvidia-pain

Nvidia on X11 sucks... kinda. It's very finicky to work with but if you get a working config, it 
should stay fine (hopefully). A very common issue is after using `nvidia-xconfig`, your X11 config 
will turn into a ticking timebomb that will unexpectedly break for seemingly no reason. You might
find yourself one day with an extremely slow system that doesn't seem to be as blazing fast as
before. You kill picom or whatever your compositor is and the issue is suddenly fixed! There are
a few steps you can take to try and debug this issue, but it is still generally just a pain to get
working again.

### 1. Actually confirming it's an issue with NVIDIA
Kill your compositor and open a terminal or switch to a TTY (using ctrl+alt+f{number}) so you can 
run some commands. Run `glxinfo | grep OpenGl`. You might need to install glxinfo to run that
command. If it outputs something like `OpenGL vendor string: NVIDIA Corporation` or `OpenGL 
renderer string: GeForce GTX 1660 SUPER/PCIe/SSE2`, then your problem is not with the GPU or
drivers (at least as it relates to this guide). If theres something about `Mesa` or `SGI`, then 
you're using CPU rendering which is why your performance is so bad.

### 2. Installing the DKMS
You'll want to install the **D**ynamic **K**ernel **M**odule **S**upport package for your NVIDIA 
driver. On arch based distros, this can be done by doing `sudo pacman -S nvidia-dkms`. Other distros
will have a similar process. What this does is install a package that can replace the binary blob
in your kernel to always match your current driver version so that there aren't any kernel/driver incompatibilities.

### 3. Fixing your /etc/X11 config
1. First cd into `/etc/X11`. If you run `ls` you'll see a few files and folders.
2. Make a backup of `xorg.conf` and `xorg.conf.d` by running `cp xorg.conf xorg.conf.old; mv 
xorg.conf.d xorg.conf.d.old`.
3. Open up `xorg.conf` and remove everything that isn't necessary. My [xorg.conf](xorg.conf) has
been linked to use as an example. My monitor runs at 144hz so you might need to do something 
different in the `Section "Screen"` area. 
### DO NOT BLINDLY COPY AND PASTE, READ THROUGH WHAT YOU ARE DOING, WHY YOU ARE DOING IT, AND WHAT EFFECT IT WILL HAVE. USE [ONLINE DOCUMENTATION](https://linux.die.net/man/5/xorg.conf) TO HELP YOU THROUGH THIS.
