# Turbocharge Your Research: A Quick Guide to High-Performance Computing on CoCalc

Video: [https://youtu.be/qxF7a246t_I](https://youtu.be/qxF7a246t_I)

Description: Discover the power of High Performance Computing (HPC) with CoCalc, offering an integrated environment to harness high-performance compute servers effectively. This tutorial guides you through using CoCalc's HPC capabilities, featuring tools like Slurm, the Intel HPC Toolkit, Clang, and GFortran. Learn how to select and configure the appropriate compute server through a detailed step-by-step process, write and compile Fortran code, and execute parallel processes with ease. Suitable for both beginners exploring cost-effective options and seasoned researchers needing robust computing power, this demo provides practical insights into setting up a virtual machine on Google Cloud, optimizing costs, and streamlining your HPC tasks using familiar interfaces such as VS Code and JupyterLab.

## Tutorial

CoCalc compute servers have a High Performance Computing (HPC) image, which includes [Slurm](https://slurm.schedmd.com/documentation.html), the [Intel HPC Toolkit](https://www.intel.com/content/www/us/en/developer/tools/oneapi/hpc-toolkit.html), clang, gfortran, and other compilers.

To use it, click the "HPC/Fortran" template:

<img src=".hpc.md.upload/paste-0.9350733032007519"   width="1163.69px"  height="311.367px"  style="object-fit:cover"/>

Or select the HPC image on Google Cloud:

![](.hpc.md.upload/paste-0.9337147533814754)

The HPC template by default uses the h3\-standard\-88 instance type which has 88 CPU's, 352GB of RAM, and each vCPU is a true CPU. This machine type is [highly optimized for HPC tasks](https://cloud.google.com/blog/products/compute/new-h3-vm-instances-are-optimized-for-hpc). It is also not cheap, so if you're on a budget or just exporing, select any other machine type. E.g., if you type "cpu:8" into th Machine Type box, you'll see all options sorted by price with 8 vCPU, which should be enough for a quick tutorial:

![](.hpc.md.upload/paste-0.2158496366052265)

I'm selecting n2d\-highcpu\-8, and also up higher, switching the Provisioning to Spot, since we're just doing a tutorial:

![](.hpc.md.upload/paste-0.1967839577338566)Now this will only cost \$0.09/hour! You can also switch the region to us\-east5, and it's even cheaper at \$0.07/hour!

![](.hpc.md.upload/paste-0.45581151610886206)I'm also enabling DNS so that we can easily use VS Code and JupyterLab directly on the compute server:

![](.hpc.md.upload/paste-0.17121313011329597)

Optional: It can be nice for HPC work to avoid using a networked filesystem at all. Thus we'll edit code and compile it in a directory that only exists on the compute server. Add the directory "hpc\-demo" to the "Fast Local Directories":

![](.hpc.md.upload/paste-0.6383851634183966)

Then hit enter:

![](.hpc.md.upload/paste-0.7741545336953282)

Finally, let's start it up, which should take about 2 minutes:

![](.hpc.md.upload/paste-0.4191389901632556)

And here it goes:

![](.hpc.md.upload/paste-0.013320187666732153)

You can click "Serial" on the left to watch the boot process if you want:<img src=".hpc.md.upload/paste-0.9097651587483226"   width="1163.69px"  height="636.391px"  style="object-fit:cover"/>

Once it is fully running and all the progress bars are at 100%, let's write some [Fortran code](./binary_tree.f90):

<img src=".hpc.md.upload/paste-0.2468504460870462"   width="1163.69px"  height="518.594px"  style="object-fit:cover"/>In the File exploer set the server in the upper left to our compute server:

![](.hpc.md.upload/paste-0.9434392354074206)

See

![](.hpc.md.upload/paste-0.8108319371351016)

Then navigate to hpc\-demo, and click \+New \-\-&gt; `binary_tree.f90` and paste this [Fortran code](./binary_tree.f90) in:

![](.hpc.md.upload/paste-0.1440316649686466)

To compile our code, click "\+New \-\-&gt; Linux Terminal", and set the terminal to run on the compute server:

![](.hpc.md.upload/paste-0.6591113231074563)

Now use the Intel Fortran Compiler by typing `ifx binary_tree.f90` :

![](.hpc.md.upload/paste-0.4582907557149931)And run the code:

<img src=".hpc.md.upload/paste-0.679849644308703"   width="891.281px"  height="741.352px"  style="object-fit:cover"/>

This compute server has all the [standard Intel HPC tools](https://www.intel.com/content/www/us/en/developer/tools/oneapi/hpc-toolkit.html) installed, and also Clang and GNU compilers. We can also use gfortran to compile the same code by typing `gfortran binary_tree.f90` :

![](.hpc.md.upload/paste-0.04038237469447803)

Also, the [Slurm Workload Manager](https://slurm.schedmd.com/) is installed and configured to run as many processes in parallel as vCPU's that your compute server has. Let's run our Fortran program 8 times via `srun -n 8 ./a.out`

Below we verify that it indeed did run 8 times.

![](.hpc.md.upload/paste-0.3991690217532058)

As of May 2024 CoCalc doesn't explicitly support clustering. However, Google Cloud VM's offer over 400 vCPU per machine, so for a large number of applications using a cluster isn't necessary. **Spin up a VM for \$5\-\$10/hour, run it for a day, and get your HPC research done now!**

Finally, if you're more used to VS Code than CoCalc as environment, it's easy to run directly on the compute server, by clicking the VS Code button to the left of the compute server or via the menu in the upper right. Similar for JupyterLab.
