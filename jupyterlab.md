# Using JupyterLab on [CoCalc](https://cocalc.com/)

Author: [William Stein](https://cocalc.com/about/team)

Video: https://youtu.be/LLtLFtD8qfo

https://CoCalc.com makes it very easy to run a hosted JupyterLab
instance in the cloud, either a lightweight instance on our shared cluster, or a high powered instance on a dedicated virtual machine.

## In a [CoCalc Project](https://doc.cocalc.com/project.html)

Start any project, click on "Explorer", then click on
the JupyterLab Button off to the right, and JupyterLab
will pop up in another browser tab in a few seconds.

![](.jupyterlab.md.upload/paste-0.9950633901922943)

<br/>

The default environment of a CoCalc project is a relatively
limited Docker container on a shared machine that is part
of our Kubernetes cluster.

If you add collaborators to your project, they can also use JupyterLab and will be able to access the same files.

**Note:** we do not currently enable realtime collaboration
in JupyterLab by default right now \(due to bugs\), but that
may change in the near future.

## On a [Compute Server](https://doc.cocalc.com/compute_server.html)

When you need (vastly!) more computational resources, GPU's or disk space, fire up a compute server. This augments your project with a remote dedicated computer. With one click, you can then launch JupyterLab running directly on the compute server.

To create a compute server, click "Servers" on the left:

![](.jupyterlab.md.upload/paste-0.19852115666612447)

Click "Create Compute Server..." to create your server.
There are a huge number of options regarding RAM, CPU's,
GPU's, etc.   To run JupyterLab, _**it's critical to enable DNS**_ so that your JupyterLab server has a proper SSL certificate and domain name:

![](.jupyterlab.md.upload/paste-0.1827772746663061)

Since you'll be running JupyterLab directly on the compute server, if you select a region geographically close to yourself, it will have much lower latency.  Here I've selected a \$0.06/hour spot instance with 8 vCPUs and 32 GB RAM on the US West coast, because I'm near Seattle at the moment:

![](.jupyterlab.md.upload/paste-0.30587783078997255)

Once your compute server is running, you
can launch JupyterLab either by clicking the "Jupyter"
button on the left, or by opening the "⋮" menu in the upper right.

![](.jupyterlab.md.upload/paste-0.6889429558547853)

After clicking you'll see a dialog for a few seconds while JupyterLab launches:

![](.jupyterlab.md.upload/paste-0.22983337264587278)

<br/>

Then your JupyterLab server will be up and running!

![](.jupyterlab.md.upload/paste-0.8669553911217733)

<br/>

You can share the full link including the token with anybody else who you would like to share access to this JupyterLab server \(and hence the files in your project\).  They do not need a CoCalc account.  People without the token are blocked:

![](.jupyterlab.md.upload/paste-0.1318418031619586)

Your compute server is a full Ubuntu 22.04 machine with network and root access, it can run Docker containers, Kubernetes, Dask, etc.  You can easily get a shell on it by clicking \+New \-\-&gt; Linux Terminal, then selecting your compute server in the upper left where it says "Server":

![](.jupyterlab.md.upload/paste-0.12449193345534337)

When you are ready to do something serious and extra support would save you time, contact us at [help@cocalc.com](mailto:help@cocalc.com)! 

When you're done, stop your compute server, which doesn't delete the disk, but still has a small cost.  You can also deprovision it, which deletes the disk and incurs no cost.

