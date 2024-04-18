# Running On-Prem Compute Servers on CoCalc

You can add the power of a VM running on your own server to a
https://CoCalc.com project.

VIDEO: https://youtu.be/NkNx6tx3nu0

Open a project on https://cocalc.com, then create a Jupyter notebook with the Python kernel and do some basic computations.  You can then add a compute server to your project and instead run the notebook temporarily on the compute server.   E.g., in the video I add an on-prem compute server running on my Macbook Pro laptop to a CoCalc (https://cocalc.com) project, and using the compute server via a Jupyter notebook and a terminal.  This involves creating an Ubuntu 22.04 virtual machine via multipass, and pasting a line of code into the VM to connect it to CoCalc.

![](.onprem.md.upload/paste-0.2299110202569199)

After using a compute server running on my laptop, I create another compute server running on Lambda cloud (https://lambdalabs.com/).  This involves renting a powerful server with an H100 GPU, waiting a few minutes for it to boot up, then pasting in a line of code.  The compute server gets configured, starts up, and we are able to confirm that the H100 is available.  We then type "conda install -y pytorch" to install pytorch, and use Claude3 to run a demo involving the GPU and train a toy model.

![](.onprem.md.upload/paste-0.4177807966967708)

<br/>
<br/>

![](.onprem.md.upload/paste-0.24040074716849102)

<br/>
<br/>

![](.onprem.md.upload/paste-0.7363532414129568)

