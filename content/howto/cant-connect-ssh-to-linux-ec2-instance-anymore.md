+++
author = "Marko Zivanovic"
title = "Can’t connect/SSH to Linux EC2 instance anymore"
date = "2021-01-17"
tags = [
    "aws", "cloud", "ubuntu"
]
categories = [
    "howto", "aws"
]
+++


Chances are that you’re here because you did something stupid. You locked yourself out of your EC2 instance somehow. Maybe you’ve played with the firewall settings on the machine, maybe you don’t know what you’ve done. Been there, done that. Luckily, there’s a workaround.

In my case, I’ve messed up <a href="https://en.wikipedia.org/wiki/Uncomplicated_Firewall" target="_blank">UFW</a> config on my Ubuntu instance. If you messed up some other configuration, you might have to edit a different file, but the basics of workaround are the same – attaching a disk from the instance you locked yourself out to the recovery instance you’ll create.

Here are the steps:

1. Create a new recovery instance (the same as the one you messed up)
2. Stop the problematic instance (DO NOT TERMINATE!)
3. Detach the volume (problematic volume) from the original instance
4. Attach it to the recovery instance as **/dev/sdf**
5. SSH to the created recovery instance
6. Run **$ sudo lsblk** to display the attached volumes and confirm the name of the problem volume. It usually begins with **/dev/xvdf**
7. Mount problem volume: **$ sudo mount /dev/xvdf1 /mnt**
8. Open the UFW configuration file: **$ sudo vim /mnt/etc/ufwufw.conf**
9.  Change **ENABLED=yes** to **ENABLED=no** and save
10. Unmount volume: **$ sudo umount /mnt**
11. Detach the problematic volume from the recovery instance and re-attach it to the original instance as **/dev/sda1**.
12. Start the original instance and you should be able to log back in.
