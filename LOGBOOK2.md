# Trabalho realizado nas Semanas #2 e #3

## Identificação

CVE-2022-0847, also known as Dirty Pipe, is an injection vulnerability that became possible with the new pipe buffer structure in the Linux Kernel introduced in version 5.8. This vulnerability allows an unprivileged local user to use this flaw to write in read-only files, and as a result, escalate their privileges on a system.

## Catalogação

In 29-04-2021 a costumer sent a support ticket complaining that the access logs he downloaded could not be decompressed

In 19-02-2022 Max Kellermann discovered that the file corruption problem identified as Linux kernel bug turned out to be an exploitable vulnerability.

In 20-02-2022 a bug report of the exploit and patch was sent to the Linux kernel security team.

In 23-03-2022 it was released Linux stable versions with Max Kellermann bug fix (5.16.11, 5.15.25, 5.10.102).

In 07-03-2022 the topic was disclosed to the public.

## Exploit

This exploit makes use of the _splice()_ which is a system call that moves data between a file descriptor and a pipe, without requiring the data to cross the user-mode/kernel-mode address space boundary, which results in better performance.

To start the exploit, you need to read the target file so that it gets cached in the page cache.
Then create a pipe with the PIPE_BUF_FLAG_CAN_MERGE flag set.
Next, use the _splice()_ system call to make the pipe point to the location of the page cache where the desired data of the file is cached.
Finally, you can write the arbitrary data into the pipe. This data will overwrite the cached file page and because PIPE_BUF_FLAG_CAN_MERGE is set, it ultimately overwrites the file on the disk, thus completing the exploit.

One public example of automation can be found [here](https://github.com/Arinerron/CVE-2022-0847-DirtyPipe-Exploit). It changes the root password to “aaron” by modifying the /etc/passwd file. It also creates a backup and after exiting your shell it will restore the backup.

## Ataques

This exploit was used to grant temporary root access for Android. This only worked on Google Pixel 6 and certain versions Samsung Galaxy S22. On Google Pixel 6, the Dirty Pipe exploit was finally patched on May 2022 security update.
