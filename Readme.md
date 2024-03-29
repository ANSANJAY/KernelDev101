# Basics of Kernel Programming

This README provides a concise introduction to the basics of kernel programming, covering the fundamentals of device drivers, kernel modules, and more.

## What is a Device Driver?

A device driver, often simply referred to as a 'driver,' is a software component that facilitates interaction between the computer's OS and a hardware device.

```
+----------+     +----------+     +----------+
|   User   |     |  Kernel  |     | Hardware |
+----------+     +----------+     +----------+
     ^                ^                ^
     |                |                |
User Interface   Kernel Interface   Hardware Interface
```

A device driver essentially serves as the mediator with:
- One side communicating with the kernel
- One side interfacing with the hardware
- One side interacting with the user

## What is a Kernel Module?

Traditionally, to add functionalities to the kernel, it had to be recompiled and the system rebooted. Kernel Modules change this by allowing code to be dynamically loaded and unloaded from the kernel upon demand.

### Terminology:
- **Loadable Kernel Modules (LKM):** Another name for Kernel Modules.
- **Extension:** `.ko` which stands for Kernel Object.

### Standard Location:
By default, modules are stored in `/lib/modules/<kernel version>` on the root file system.

![](/001.Basics/images/1.png)

![](/001.Basics/images/Screenshot%20from%202023-08-15%2020-50-48.png)


## Device Driver vs Kernel Modules:
While every device driver is a kernel module, not all kernel modules are device drivers. Kernel modules serve various purposes, including:
1. Device Drivers
2. File Systems
3. System Calls
4. Network drivers (e.g., those implementing TCP/IP)
5. TTY line disciplines (for terminal devices)

## Advantages:
1. Memory efficient as they can be loaded/unloaded on demand.
2. No need to reboot after every modification.
3. Bugs in modules don't necessarily halt the system.
4. Easier debugging and maintenance.
5. Simplified management for multiple machines.

## Disadvantages:
1. Consumes more memory due to module management.
  - Module management consumes unpageable kernel memory.
  - A basic kernel with a number of modules loaded will consume more memory than a equivalent kernel module with the drivers compiled into the kernel image itself.

2. Modules load late in the boot process , so essential features must be in the base kernel.
 - so all core functionality should be in the base kernel.

3. Static kernels prevent run-time modifications, enhancing security.

## Configuration:
To support modules, the kernel should have the `CONFIG_MODULES=y` option enabled.

![](images/Screenshot%20from%202023-08-15%2021-10-32.png)



### Types:
1. **In-Source Tree:** Present within the Linux Kernel Source Code.
2. **Out-of-Tree:** Absent from the Linux Kernel Source Code, though they can eventually become in-tree modules.

### Basic Commands:
- **List Modules:** `lsmod` (Info derived from `/sys/modules`).

![](/images/Screenshot%20from%202023-08-15%2020-45-45.png)


- gives infor on size of module.
- used by field explains the dependency 

- **Module Information:** `modinfo` provides detailed information about a module.

![Module information for module ip6_tables](/images/Screenshot%20from%202023-08-15%2021-14-47.png)


![Module information for module cec](/images/Screenshot%20from%202023-08-15%2021-16-51.png)

Note - 

- parm  : acceptable parameter for the module
- intree : It's an intree module

---

With this foundation, you're ready to delve deeper into the fascinating world of kernel programming.
