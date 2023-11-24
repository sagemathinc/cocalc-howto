# Run Your Own SageMathCell Server

This tutorial guides you through installing SageMathCell on freshly released CoCalc compute servers. If you follow it, you will get a server close to public https://sagecell.sagemath.org but with possibility to tweak parameters, installed software, and access to it.

As of November 24, 2023, SageMathCell deployment relies on LXC containers and BTRFS filesystem, so we will work outside of the usual Docker container that is closely intergrated with CoCalc. This is likely to change in the future.

## Create and Add SSH Key

In order to escape from the Docker container and become root on the actual compute server, we need to create an SSH key inside of the CoCalc project and [add it to the list of project keys](https://doc.cocalc.com/project-settings.html?highlight=ssh+key#configuring-ssh-keys-for-a-single-project).

## Create a Compute Server

The most important parameter is the disk size; 150GB should be sufficient and with this size the more affordable "Standard \(HDD\) disk" should work fine. The basic "Python" image will work for us as well. As far as CPU and RAM goes, pick what you want \- it may make sense to get a more powerful machine while you build your server and then drop it down to something like 4 vCPU's and 16GB RAM later:

![](.SageMathCell.md.upload/paste-0.394184525012494)

You probably want to create a domain name for your server:

![](.SageMathCell.md.upload/paste-0.3480906054389139)

## Become root on the Compute Server

Start a terminal running on your new server and enter

```sh
ssh root@localhost
```

If your SSH key was set up correctly, you will be logged into the compute server VM outside of any Docker container:

![](.SageMathCell.md.upload/paste-0.09347849627811611)

Update software sources, install upgrades, and prerequisites for SageMathCell:

```sh
apt update && DEBIAN_FRONTEND=noninteractive apt-get install -y haproxy lxc python3-lxc python3-psutil rsyslog-relp
```

## Create BTRFS for LXC

These commands create a 100GB sparse file for LXC containers to live in. Adjust the size if you need to. Watch for the output of the `losetup` command and use it for `mkfs.btrfs`. Then edit `fstab`:

```sh
truncate -s 100G varliblxc.img
losetup -fP --show varliblxc.img
mkfs.btrfs /dev/loop5
```

We add a line to `/etc/fstab` to automatically mount our new btrfs filesystem:

```sh
echo "/root/varliblxc.img /var/lib/lxc    btrfs   loop,noatime,compress=lzo       0       0" >> /etc/fstab
```

Reboot to make sure that everything is working correctly. \(At the moment there is no way to recover if you mess up the booting process; you woudd have to start from scratch or make a support request to [help@cocalc.com.](mailto:help@cocalc.com)\)

```sh
reboot -h now
```

## Create a Self-Signed SSL Certificate

For example, follow [this guide](https://tecadmin.net/step-by-step-guide-to-creating-self-signed-ssl-certificates/). Combine your private key and certificate into a single file and put them into the HAProxy folder with appropriate permissions:

```sh
openssl genrsa -out mysagecell.key 2048
openssl req -new -key mysagecell.key -out mysagecell.csr
openssl x509 -req -days 365 -in mysagecell.csr -signkey mysagecell.key -out mysagecell.crt
cat mysagecell.crt mysagecell.key > mysagecell.pem
chmod 0600 mysagecell.pem
mkdir /etc/haproxy/cert
cp mysagecell.crt /etc/haproxy/cert/
```

## Management Script

Download SageMathCell management script, change its permissions and execute. The first run is fast. Then exit and login again:

```sh
wget https://raw.githubusercontent.com/sagemath/sagecell/master/contrib/vm/container_manager.py
chmod a+x container_manager.py
./container_manager.py
exit
```

Edit `container_manager.py` to suit your needs, e.g., add extra packages. At the very least, you need to configure access on port 443 using the certificate you have created. To do this, find `HAProxy_section` string and add to it

```raw
    bind *:443 ssl crt /etc/haproxy/cert/mysagecell.pem
    option forwardfor
    http-request add-header X-Proto https if { ssl_fc }
```

so you get

![](.SageMathCell.md.upload/paste-0.43725874620434624)

<br/>

_**NOTE:**_ The above is the first time in this entire tutorial where you have to explciitly edit a file.  One way to edit this file is using the command line terminal and some editor such as vim, joe, emacs, etc.  Another way is to copy the file to /home/user, click "Sync Files", edit the file in your project by clicking on it and editing it, click "Sync Files" again, then copy it back:

```sh
cp container_manager.py /home/user/
# ... edit the file in cocalc after clicking "Sync Files"
# click "Sync Files" again
cp /home/user/container_manager.py .
```

Now run the script to build and deploy the containers, which should take **HOURS** as it installs a large number of Ubuntu packages and builds SageMath from source:

```sh
time ./container_manager.py --deploy
```

Once this is done, your server should be up and running. You may consider reducing resources of your compute server to lower the cost.

## Slow Start Up Warning

At the moment booting up a machine with SageMathCell installed as above takes about 7 MINUTES. This is due to deliberate delays introduced by the management script which CoCalc is waiting on to consider the server ready. Please be patient when booting up!  You can also ssh to root@ip\-address\-of\-server just a few seconds after the server starts running.

## Spot Provisioning Issues

If you are running your server on a spot instance, it may be randomly killed during peak usage times.  Currently CoCalc does not attempt to automatically restart your server if it is killed in this way, but that is on our roadmap.   Thus right now if you want your server to be up all of the time, select Standard instead of Spot under "Provisioning".  You can switch this setting back and forth any time the server is off.

