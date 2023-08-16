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




### Device Driver vs Kernel Modules:
While every device driver is a kernel module, not all kernel modules are device drivers. Kernel modules serve various purposes, including:
1. Device Drivers
2. File Systems
3. System Calls
4. Network drivers (e.g., those implementing TCP/IP)
5. TTY line disciplines (for terminal devices)

### Advantages:
1. Memory efficient as they can be loaded/unloaded on demand.
2. No need to reboot after every modification.
3. Bugs in modules don't necessarily halt the system.
4. Easier debugging and maintenance.
5. Simplified management for multiple machines.

### Disadvantages:
1. Consumes more memory due to module management.
2. Modules load late, so essential features must be in the base kernel.
3. Static kernels prevent run-time modifications, enhancing security.

### Configuration:
To support modules, the kernel should have the `CONFIG_MODULES=y` option enabled.

### Types:
1. **In-Source Tree:** Present within the Linux Kernel Source Code.
2. **Out-of-Tree:** Absent from the Linux Kernel Source Code, though they can eventually become in-tree modules.

### Basic Commands:
- **List Modules:** `lsmod` (Info derived from `/proc/modules`).
- **Module Information:** `modinfo` provides detailed information about a module.

---

With this foundation, you're ready to delve deeper into the fascinating world of kernel programming.
