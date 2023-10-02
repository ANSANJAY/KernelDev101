Linux dmesg command Tutorial 
=============================

What does dmesg command do?
--------------------------

The Kernel keeps all the logs in a ring buffer. This is done to avoid the boot logs from getting lost until the syslog daemon starts, collects them, and stores them in /var/log/dmesg. If we don't store them in the ring buffer, we will lose the boot up logs.

The `dmesg` command is used to control or print the kernel ring buffer. By default, it prints messages from the kernel ring buffer onto the console.

Important dmesg commands:
-------------------------

1. **Clear Ring buffer**: 

   - `$dmesg -c` - Will clear the ring buffer after printing.
   - `$dmesg -C` - Will clear the ring buffer but does not print on the console.

2. **Don't Print Timestamps**: 

   - `$dmesg -t` - Will not print timestamps.

3. **Restrict dmesg command to list of levels**:

   - `$dmesg -l err,warn` - Will print only error and warn messages.

4. **Print human readable timestamps**:

   - `$dmesg -T` - Will print timestamps in a readable format. Note: The timestamp could be inaccurate.

5. **Display the log level in the output**:

   - `$dmesg -x` - Will add the log level to the output.

6. **Combining options**:

   - `$dmesg -Tx` - Will print both human readable time and log level.

![](./../images/Screenshot%20from%202023-10-01%2022-23-20.png)
