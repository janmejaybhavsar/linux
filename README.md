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

CMPE 283 Assignment 2 – Instrumentation via hypercall
	Name: Janmejay Bhavsar(SJSU ID: 015931344)

The following steps were followed for completing the assignment:
1)	Navigate to the directory and update the files: /arch/x86/kvm/cupid.c and /arch/x86/kvm/vmx/vmx.c to include the code to handle the exits for nodes 0x4fffffff and 0x4ffffffe.
2)	Next step is to build the kvm module and install the kernel using the following commands: 
$ sudo make -j 16 modules
$ sudo make INSTALL_MOD_STRIP=1 modules_install
$ sudo make install
Since I had previously made the kernel for assignment it took me a much lesser time to set it up again in this assignment
3)	After I found that there was existing kvm and kvm_intel modules using the command: $ lsmod | grep kvm and to remove that I used the command: $ sudo rmmod kvm_intel and $ sudo rmmod kvm(follow the order to remove the modules).
4)	Next step is to load the current kvm module which contains the code we modified in those files by using the commands: $ sudo modprobe kvm and $ sudo modprobe kvm_install(follow the ordering to load the modules).
5)	After that I verified if the modules are loaded successfully by again using the command: $ lsmod | grep kvm
This will successfully confirm that our module was loaded into the kernel with no errors in the code.
6)	Now to verify if our code runs correctly I had to install an Ubuntu VM instance inside the current VM by installing the Virtual Manager by using the following commands:
$ sudo apt update
$ sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager
After that I verified if the libcirt daemon is active or not by using the command: $ sudo systemctl is-active libvirtd
7)	Next step is to add user permissions for the new processes using the following commands:
$ sudo usermod -aG kvm $USER
$ sudo usermod -aG libvirt $USER
$ newgrp libvirt
8)	After adding the permissions start the virtual manager using the command: $ virt-manager and after that boot up using an existing iso of Ubuntu and install Ubuntu in the new VM.
9)	After installation open terminal and type the commands:
$ sudo apt-get update -y
$ sudo apt-get install -y cpuid
This will install the cpuid package and let us directly input the value for which we need to check the exit outputs for.
10)The next step is to run the commands: 
$ cpuid -l 0x4fffffff and $ cpuid -l 0x4ffffffe 
This will display all the values in the registers in the other VM. 
11)After running those commands in the new VM head back to the
existing VM and enter the command dmesg to check the output of the kernel.
Following are the screenshots of both VMs and the output displayed on the kernal message log: 
![image](https://user-images.githubusercontent.com/89321629/164387647-3fb187bc-b4fc-4d3a-88e1-c26200458031.png)

 
This the new instance of VM in which cpuid command is run with 0x4fffffff leaf node.
![image](https://user-images.githubusercontent.com/89321629/164387670-79451cee-01c8-43c3-96bf-7cdd7e73c22a.png)

 
This is the current instance of VM which displays the kernal ouput for the total number of exits performed for the above mentioned leaf node
![image](https://user-images.githubusercontent.com/89321629/164387692-7c1f8b32-47e0-41e2-a6d9-81231c19e570.png)

 
This the new instance of VM in which cpuid command is run with 0x4ffffffe leaf node.
![image](https://user-images.githubusercontent.com/89321629/164387726-82bd6b7d-fd4f-4f5a-8d9f-3ef04d5e6f7a.png)

 
This is the current instance of VM which displays the kernal ouput for the total time spent in processing all the exits for the above mentioned leaf node.
12)The final step in this assignment is to commit the 2 files which were modified and push them onto the git repository by using the following commands:
$ git add /arch/x86/kvm/cpuid.c /arch/x86/kvm/vmx/vmx.c
$ git commit
$ git push
