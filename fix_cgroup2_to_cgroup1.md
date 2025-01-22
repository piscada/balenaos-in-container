To downgrade from cgroup2 to cgroup1 on Debian 12 (Bookworm), you need to adjust your kernel boot parameters. Here's a step-by-step guide:

1. Verify Current cgroup Version

Run the following command to check if cgroup2 is currently enabled:

mount | grep cgroup

If you see cgroup2 listed, your system is running cgroup2.

2. Modify GRUB Configuration

You need to add a kernel parameter to disable cgroup2 and use cgroup1.

Open GRUB Configuration:

sudo nano /etc/default/grub

Add Kernel Parameter:

Find the line starting with GRUB_CMDLINE_LINUX and add the following:

systemd.unified_cgroup_hierarchy=0

For example:

GRUB_CMDLINE_LINUX="quiet splash systemd.unified_cgroup_hierarchy=0"

3. Update GRUB

Apply the changes to GRUB by running:

sudo update-grub

4. Reboot the System

Reboot your machine to apply the changes:

sudo reboot

5. Verify cgroup1 is Enabled

After the system reboots, verify that cgroup1 is being used:

mount | grep cgroup

You should see something like:

cgroup on /sys/fs/cgroup type tmpfs (rw,mode=755)

6. Test Docker Compatibility

Restart the Docker service and ensure it's functioning correctly:

sudo systemctl restart docker
docker info | grep -i cgroup

You should now see cgroupfs under Cgroup Driver.

Notes:

Downgrading to cgroup1 might have implications for other services or applications relying on cgroup2. Test your environment thoroughly after making this change.

If this doesn't resolve your issue, additional debugging of kernel configuration or Docker settings may be required.
