
## How i compiled linux kernel to source

### Step 1:
Install essentials for compiling linux from source

```
sudo apt-get install build-essential libncurses-dev bison flex libssl-dev libelf-dev
```

### Step 2:
Clone a recent stable linux kernel repository and checked out linux-5.12.y branch

```
git clone git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git linux_stable
```
```
cd linux_stable

git branch -a | grep linux-5

git checkout linux-5.12.y
```

### Step 3:
Add a .config file into the kernel root directory. in my case i copied my PC current .config file
so you want customize your kernel using

```sudo cp /boot/.config {path to kernel root directory.config} ```


### Step 4:
Customize your kernel
many ways to customize your kernel, using any of the following commands you can configure driver as module(to be loaded and unloaded at run time) or as builtin (to be loaded at boot time). drivers built into the kernel will increase the kernel image and the boot up time.

```
1. make menuconfig        - 
2. make oldconfig         - 
3. make localconfig       - 
4. make localmodconfig	  - 
5. make defconfg	        - This command creates a configuration based on the defaults for your architecture:
6. make xconfig	          - requires qt5 installation. you can install it using sudo apt-get install qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools
```

Use make oldconfig to apply any changes made to the conf file outside the terminal
You should always run this before building a kernel.

### TIPs:
for the source to compile successfully on my machine i had to disable few things
```
scripts/config --disable SYSTEM_TRUSTED_KEYS
scripts/config --disable SYSTEM_REVOCATION_KEYS
```

for faster compile disable debug. this can make your compile twice as faster
```
scripts/config --disable DEBUG_INFO
```


### Step 5:
compile the source using any of the following comands. uses host architecture as the target architecture
```
make all		  - builds the kernel for the host architecture
make			    - this will also invoke the "make all" command
make -j2 all	- 
make -j2 		  - this specifies the number of threads to run concurrently for the compilation process
time make		  - will time your compilation process
```
This will compile for the host architecture by default.

If you want to compile for a different architecture, arm cortex A8 for instance

``time make -j4 ARCH=arm CROSS_COMPILE=arm-cortex_a8-linux-gnueabihf- zImage``

And for x86_64 architecture which is my host architecture. leaving out 'ARCH=x86_64' will make it use the host architecture as target architecture
```
time make -j4 ARCH=x86_64
```

After compilation, the image is located here
``Kernel: arch/x86/boot/bzImage``

**Step 6:**
install the kernel and update grub using
```
sudo su -c "make modules_install install"
```


## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/paulnwoko/My-Linux-learning-journey/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/paulnwoko/My-Linux-learning-journey/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
