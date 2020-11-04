#Step in details
1.	Clone the repository for linux
2.	Run command “sudo bash” to login to root mode bash
3.	Run command “apt-get install build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache bison flex libelf-dev” to install essential packages for building kernel
4.	Run command “uname -a”. Copy the kernel version.
Output: Linux ubuntu 5.4.0-52-generic #57-Ubuntu SMP Thu Oct 15 10:57:00 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
5.	Run command “cp /boot/config-4.15.0-112-generic ./.config”
6.	Run command “make oldconfig” (Holding down enter to have default settings)
7.	Run the following commands in sequence:
a.	make
b.	make modules
c.	make install
d.	make modules_install
8.	Run command “reboot”
9.	Run command “uname -a”. Check the kernel version.
Output: Linux ubuntu 5.10.0-rc2+ #1 SMP Tue Nov 3 11:46:16 PST 2020 x86_64 x86_64 x86_64 GNU/Linux
10.	Run command “sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils” to install KVM
11.	Run command “kvm -version” to check.
Output: QEMU emulator version 4.2.1 (Debian 1:4.2-3ubuntu6.8)
Copyright (c) 2003-2019 Fabrice Bellard and the QEMU Project developers 
12.	Run command “sudo apt-get install virt-manager” to install virt-manager
13.	Modiy “~/linux/arch/x86/kvm/cpuid.c” from the linux repository. Add leaf function at int kvm_emulate_cpuid(struct kvm_vcpu *vcpu) method


		if(eax == 0x4FFFFFFF){
			pr_info("CMPE 283 Leaf function for eax == 0x4FFFFFFF");
			kvm_cpuid(vcpu, &eax, &ebx, &ecx, &edx, false);

			eax = atomic64_read(&number_of_exits);
			pr_info("The total number of exits (all types) in eax=%u", eax);

			ebx = (atomic64_read(&total_time) >> 32);
			pr_info("The high 32 bits of the total time spent processing all exits in ebx=%u", ebx);

			ecx = ((atomic64_read(&total_time) >> 32) << 32);
			pr_info("The low 32 bits of the total time spent processing all exits in ecx=%u", ecx);}
14.	Modify “~/linux/arch/x86/kvm/vmx/vmx.c” from the linux repository. Define the counter to measure the numbers of exits and total time in processor cycles.
/*CMPE 283 Assignment 2*/
static void meassureTotalTime(int startTime){
	atomic64_add(rdtsc() - startTime, &total_time);
}
15.	Do step 7 again
16.	Run tests
Output: CPUID(0x4FFFFFFF), exits=31294, cycles spent in exit=1056348271


#Questions
1. Does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? 
2. Approximately how many exits does a full VM boot entail?





