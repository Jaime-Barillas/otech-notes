# What is an Operating System

+ Software that controls hardware to perform various operations.
+ Can be a collection of software.
+ Can be simple or complex.

Modern OS's contain several complex functions:

+ Manage processes.
+ Manage memory.
+ Interact with hardware.
+ Manage multiple concurrently running software.

# OS "Modes"

OS software can run in different modes:

+ User mode - Apps and the UI run in this mode. Applications must request the
  OS to perform hardware actions on their behalf.
+ Kernel mode - The OS runs in this mode. Also called "Supervisor Mode". Code
  that runs in kernel mode has direct access to hardware allowing it to perform
  any operations on the hardware.

# Some Functions of the OS

+ Read/Write Disk/Memory.
+ Pure computations.
+ Network communication.
+ Media playback.
+ I/O peripheral communication.
+ General OS security.

# The OS Manages Resources

+ Hardware.
+ Software.
+ The file system.
+ In memory data.

Modern OS's are capable of managing multitasking and multiprocessing resources.

# Common OS Hardware

## CPU

> The brain of the computer

Fetches instructions and data from memory to execute and operate on them. CPU
family specific instructions exist.

## Memory

General storage for the computer. Ranges from fast to quick:

+ Hardware registers.
+ CPU local cache.
+ Main Memory (RAM).
+ SSD's.
+ HDD's.
+ Optical disks.
+ Magnetic tapes.

## I/O Devices

+ Monitors.
+ Keyboards+
+ Mice.
+ USB flash drives.
+ etc.

# The Layers of an OS

1. [App] User and other system programs.
2. [OS] The UI.
3. [OS] System calls - How apps interact with the hardware.
4. [OS] OS services - Ongoing OS processes, such as, I/O operations.
6. Hardware.

---

# OS Architectures

OS's are, generally, very large programs. Here are a few ways they are
structured:

## The Simple Structure

+ Not divided into modules
+ Interfaces and levels of functionality are not well separated.

## The Layered Structure

+ Divided into a number of layers. Each is built ontop of the last.
+ Hardware is layer 0.
+ The user interface is the highest layer.
+ Layers are defined such that each only uses the functions services of the
  next lower layer.

## Microkernels

+ Moves as many services from the kernel into user space.
+ Communication between user modules typically uses message passing.
+ Easy to extend.
+ Easy to port.
+ Often more reliable.
+ Often more secure.
+ There is performance overhead when the various user space modules communicate
  with their kernel space dependencies.

## Modules

Many OS's implement loadable kernel modules.

+ Typically uses an Object-Oriented approach.
+ Core components are separate.
+ Modules communicate over known interfaces.
+ Loaded as needed.
+ Similar to layers, but more flexible.

## A Hybrid Approach

Most modern OS's use a hybrid approach to address various concerns, such as,
security, preformance, usability, etc.

# Linux Kernel Modules

Each kernel module usually has an initializing function specified with the
`module_init()` function (macro?). Similarly there is a deinitializing function
specified with the `module_exit()` function.
Here is a simple kernel module:
```c
#include <linux/init.h>
#include <linux/module.h>
#include <linux/kernel.h>

MODULE_LICENSE("GPL"); // Needed.

static int __init init_kernel_mod(void) {
    printk(KERN_ALERT "Initializing kernel mod!\n");
    return 0;
}

static void __exit exit_kernel_mod(void) {
    printk(KERN_ALERT "Releasing kernel mod resources!\n");
}

module_init(init_kernel_mod);
module_exit(exit_kernel_mod);
```

And the associated Makefile:

```makefile
CONFIG_MODULE_SIG=n
obj-m += my_kernel_mod.o

default:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

Review the Linux kernel module development documentation for details.

## Relevant CLI Commands

+ `lsmod` - List currently loaded mods.
+ `modinfo <mod-name>` - Prints info about a specific kernel module. 
+ `rmmod <mod-name>` - Removes a currently loaded module.
+ `insmod <mod-name>` - Loads a new module (no dependencies on unloaded modules
  allowed).
+ `modprobe <mod-name>` - Loads a new module and its dependencies.
