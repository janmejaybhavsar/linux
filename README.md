CMPE 283 Assignment 1- To Discover VMX Features

Name: Janmejay Bhavsar (SJSU ID: 015931344)

The following steps were followed for completing the assignment:
1) Download and install VMWare Workstation 16 Player from https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html
2) Download and install Ubuntu from here https://ubuntu.com/download/desktop and allocate 8 GB RAM and at least 200 GB Storage Space.
3) Fork the master branch of Linux from Torvalds’s Repo https://github.com/torvalds/linux
4) Install git on Ubuntu by typing the following command in the terminal:
$ sudo apt install git
5) After installation set the global config of git using the following commands:
$ git config --global user.name "<your_github_username>"
$ git config --global user.email "<your_github_email_address>"
6) Clone the forked Linux git repo onto the home directory of the system by navigating to the forked git repo and then clicking on code and copy the HTTPS URL and then type the following command:
$ git clone https://github.com/janmejaybhavsar/linux.git
7) During the compilation of the cloned repo below are some of the commands which resolved the errors which I had encountered:
$ sudo apt-get install libelf-dev
$ sudo apt-get install gcc
$ sudo apt-get install flex
$ sudo apt-get install bison
$ sudo apt-get install libncurses5-dev
$ sudo apt-get install libssl-dev
$ sudo apt install dwarves
$ sudo apt-get install -y zstd
8) Next step is to build the kernel by navigating to the Linux directory type the following command:
$ make menuconfig
This command will display a pop up to load a specific kernel configuration, I did nothing here and just clicked on Exit.
9) Check the current kernel version by typing the following command:
$ uname -a
10) I encountered a kernel version mismatch, so I executed the following command to avoid that:
$ cp /boot/config-5.13.0-39-generic .config
11) Next execute the commands:
$ make oldconfig
$ make prepare
12) I encountered some errors related to certificates which I resolved using the below commands:
$ scripts/config --disable SYSTEM_TRUSTED_KEYS
$ scripts/config --disable SYSTEM_REVOCATION_KEYS
13) After resolving all the errors, the next step is to make modules using the command:
$ make -j 8 modules
14) After completion of that make the kernel using the command:
$ make -j 8
15) Install the modules using the following command:
$ sudo make INSTALL_MOD_STRIP=1 modules_install
16) To install the new kernel, type the following command:
$ sudo make install
17) After installation I rebooted the system using the command:
$ sudo reboot
This will apply the changes made to the kernel
18) After the reboot is complete, I verified my kernel version using the command:
$ uname -a
My kernel version changed from 5.13.0-39-generic to 5.18.0-rc2 #2 SMP PREEMP_DYNAMIC Tue Apr 12 16:58:34 PDT 2022 x86_64 x86_64 x86_64 GNU Linux
This confirms that the kernel was installed correctly.
19) After this I copied the Makefile and the cmpe283-1.c file into the linux/cmpe283/ directory so that I can commit my changes in my repo.
20) Next step is to use the make command in the cmpe283 directory to generate a kernel object I encountered a Module license error while doing this which I resolved by adding the following code in the cmpe283-1.c file:
MODULE_LICENSE("GPL v2");
21) Verify the creation of the kernel object file using the following command:
$ ls |grep *.ko
22) Now to run the file I inserted the cmpe283-1.ko file into the kernel using the following command:
$ sudo insmod cmpe283-1.ko
23) To verify if it was inserted correctly use the command:
$ lsmod | grep cmpe283
It will display cmpe283_1 if inserted correctly
24) Use the command:
$ dmesg
This will print out the VMX features which I added into the file onto the terminal console.
25) Next, I removed the file from the kernel by typing the following command:
$ sudo rmmod cmpe283_1
This will print the Assignment 1 end print message after executing the dmesg command again.

Following is the Output of my file:

 ![image](https://user-images.githubusercontent.com/89321629/163303816-cadc5ab8-d485-462a-a9a0-405b03bcda97.png)

![image](https://user-images.githubusercontent.com/89321629/163303829-6eeddcd4-cbb4-47f3-a318-0b37cf494667.png)



 



After removing the kernel object file:

![image](https://user-images.githubusercontent.com/89321629/163303836-16898b25-2a9d-46a0-b571-c16a36db8a83.png)


 

26) The final step is to add, commit and push the cmpe283-1.c and Makefile onto the forked git repository using the following commands:
$ git add cmpe283-1.c Makefile
$ git commit
$ git push
