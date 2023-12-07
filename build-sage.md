# Building SageMath from Source in 18 Minutes on CoCalc using a 60 CPU machine with 240GB RAM

Video: https://youtu.be/b8e8qq-KWbA

You can now quickly build the latest devel version of [SageMath](https://sagemath.org) from source in 18 minutes on https://cocalc.com using a [compute server](https://doc.cocalc.com/compute_server.html).

## Create a Server

First, create a powerful compute server like this AMD Epyc beast, with 60 CPU's and 240GB of RAM, which costs $0.42/hour:

![](.build-sage.md.upload/paste-0.764245154316137)

<br/>

Make sure to:

- select the Python image
- have at least 30GB of disk space
- configure a fast non-sync'd data directory:

![](.build-sage.md.upload/paste-0.3826843758977656)

This fast data directory is important not only for speed, but because it isn't synchronized back to our project. The Sage build will use up over 10GB of disk space, and wouldn't fit in a CoCalc project (with significant upgrades), so we want it to live only on the remote compute server.

<br/>

## Open a Terminal

Once your compute server is running, create a terminal by clicking "+ New", then "Linux Terminal":

![](.build-sage.md.upload/paste-0.8660784834578961)

and set your terminal to run on the compute server:

![](.build-sage.md.upload/paste-0.5419513923911645)

## Clone the Sage Source Code

Change into your fast local data directory `sage` and clone the latest version
of Sage via `git clone https://github.com/sagemath/sage`:

![](.build-sage.md.upload/paste-0.7917766072809207)

## Configure and Build Sage

After getting the sage source code, configure and start the build. If you know about tmux (or screen), I recommend running this in tmux, since there will be a MASSIVE amount of output flying by.

```sh
(compute-server-150) ~/sage$ # optional: run tmux or screen
(compute-server-150) ~/sage$ cd sage
(compute-server-150) ~/sage/sage$ sudo apt-get install m4
(compute-server-150) ~/sage/sage$ make build
(compute-server-150) ~/sage/sage$ ./configure
(compute-server-150) ~/sage/sage$ time MAKE="make -j60" make build
...
18 minutes later:
...
Sage build/upgrade complete!
(compute-server-150) ~/sage/sage$ ./sage

```

## Change Your Compute Server to be Less Powerful

Now that you're done building Sage, you probably don't need
a 60 CPU monster computer. Click on the bar at the top, stop
the server, edit it, and start it again.

![](.build-sage.md.upload/paste-0.8336088964376351)

![](.build-sage.md.upload/paste-0.30719843104047406)

![](.build-sage.md.upload/paste-0.7634204521539059)

## Install the Sage Jupyter Kernel

As explained [here](https://doc.sagemath.org/html/en/installation/launching.html), the following command installs the Sage Jupyter kernel. This command will give an error at the end because we didn't build the docs (which takes a long time and doesn't leverage our huge parallel machine). However, it still works and installs the kernel:

```sh
(compute-server-150) ~/sage/sage$ jupyter kernelspec install --user $(./sage -sh -c 'ls -d $SAGE_VENV/share/jupyter/kernels/sagemath') --name sagemath-dev
```

That said, if you want to install the kernel manually, do this:

```sh
(compute-server-150) ~/sage/sage$ cp -r /home/user/sage/sage/local/var/lib/sage/venv-python3.11.1/share/jupyter/kernels/sagemath ~/.local/share/jupyter/kernels/
(compute-server-150) ~/sage/sage$ jupyter kernelspec list
Available kernels:
  sagemath        /home/user/.local/share/jupyter/kernels/sagemath
  sagemath-dev    /home/user/.local/share/jupyter/kernels/sagemath-dev
  python3         /usr/local/share/jupyter/kernels/python3
```

Once you do this, you can open a Jupyter notebook by clicking "+New" then "Jupyter Notebook". Just like with a terminal, set the notebook to run on your compute server. Refresh the available kernels, and select your sagemath kernel. You can then use your new copy of Sage from Jupyter!

## Conclusion

I hope you found this useful and will use bleading edge versions of Sage for amazing mathematical research, and contribute back.

Challenge: If you somehow build Sage from source on CoCalc in less than 17 minutes, please let me know (Ideas: use a more poweful machine, use a ramdisk, etc.)!  wstein@cocalc.com
