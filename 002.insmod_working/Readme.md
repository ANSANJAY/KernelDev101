What happens when we do `insmod` on a module 
============================================

## What is a kernel module?

A kernel module is a segment of kernel code that can be dynamically added to the running kernel upon loading. Similarly, it can be removed when its functionality is no longer needed.

## `insmod` process

When we utilize `insmod` on a module, the system undergoes several steps:

1. **Initial Call**: The process starts with a call to `init_module()`. This intimates the kernel about a pending module load request and subsequently transfers control to the kernel .

2. **In-Kernel Operations**: Once within the kernel, the `sys_init_module()` function gets activated. This function:

   - First verifies the permissions of the user trying to load the module.
   
   - Upon successful verification, the `load_module` function is invoked. This function:
     
     - Allocates temporary memory and transfers the ELF module from the user space to the kernel memory utilizing `copy_from_user`.
     
     - Validates the integrity of the ELF file ensuring it meets the expected structure and standards.
     
     - Based on the interpretation of the ELF file, offsets are generated within the allocated temporary memory. These offsets are termed as "convenience variables".
     
     - Arguments specified by the user for the module are also transferred to the kernel memory.
     
     - The process then moves to symbol resolution.
     
     - Upon completion, `load_module` yields a reference pointing to the kernel module.
  
   - This reference, provided by `load_module`, is integrated into a doubly linked list that chronicles all loaded modules in the system.
   
   - Lastly, the `module_init` function present within the module code is triggered.

   
What Happens if We Return -1 in the Init Function of a Kernel Module
====================================================================

The init function of a kernel module, typically denoted as `init_module` or `module_init`, is the function that gets called when you insert the module into the kernel (using `insmod`). It is used to set up any resources that the module needs, such as allocating memory, initializing data structures, registering device drivers, etc.

Returning a value from the init function indicates the success or failure of the module initialization. In the Linux kernel, a return value of 0 indicates success, while a non-zero value indicates failure.

If you return -1 in the init function of a kernel module:

1. **Kernel Module Insertion**: The kernel will not insert the module. The module's functionality will not be available.
  
2. **Resource Cleanup**: Any resources that were allocated or set up in the init function up to the point of failure may need to be manually released or cleaned up, to prevent memory leaks or other resource issues.
  
3. **Kernel Logging**: The failure will typically be logged in the kernel log. When you run `dmesg`, you might see a message like `module_name: init_module failed`. The specific error message will depend on the implementation of your init function and any logging you have included.
  
4. **Dependencies**: Any subsequent modules that depend on the failed module may also fail to load or function correctly.

Regarding loading the module:
- **Loading Attempt**: You can still attempt to load the module using the `insmod` command.
  
- **Result**: If the initialization function returns -1, the module will not be loaded into the kernel's address space. `insmod` will typically return an error message to the effect of "Operation not permitted" or a similar message indicating failure.

It is generally a good practice to provide detailed error messages in your init function, to make it easier to diagnose the cause of any failures. You can use the `printk` function with an appropriate log level (e.g., `KERN_ERR`) to log error messages.
