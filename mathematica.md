# How to use the Mathematica Jupyter Kernel on [CoCalc.com](http://CoCalc.com)

_TIME: This tutorial takes less than 10 minutes to complete._

**UPDATE (May 31, 2024):** This works with Ubuntu 24.04 and Mathematica 14.0, which are the current versions with CoCalc and Mathematica.

It is finally easy to run Mathematica Jupyter notebooks on https://cocalc.com via a  Wolfram Engine license!  You only have to pay for the compute resources you use, which start at about \$0.02/hour.

**CAVEAT:** You should probably only use this if you are paying Mathematica customer.  I do not know how to buy a Wolfram Engine license.  If you use the free version, as explained below, then run your VM later, it'll will likely complain when you try again, as follows: _"The Wolfram Engine could not be activated using your Wolfram ID.  This may have occurred because WolframScript has already activated a different installation of the Wolfram Engine using your account's limited free activation keys. Only two keys are generated when you create a free Wolfram Engine license."_   It seems like Wolfram never implemented a way to **deactivate** a free Wolfram Engine license once you activate it, it never times out, and the license is tied to the exact VM you activate the license on, so this is completely unusable in a cloud context, unfortunately.

Click "Servers" in any CoCalc project:

<br/>

![](.mathematica.md.upload/paste-0.48922211489959966)

<br/>

Then create a new **Compute Server** with the Python image and make sure you create a _**disk that is at least 20GB in size!**_  You can easily expand the disk later at any time, without having to restart anything.

<br/>

![](.mathematica.md.upload/paste-0.19421151586224683)

<br/>

Start your compute server, open a Linux Terminal in your project, and select your compute server in the upper left menu so that the terminal is running on the compute server.

<br/>

![](.mathematica.md.upload/paste-0.3489692762116898)

<br/>

**Install some packages** into the compute server \(which is running Ubuntu 24.04\):

```sh
sudo apt-get update && sudo apt-get install -y libgl1 libasound2t64 libxkbcommon-x11-0 libegl1 wget avahi-daemon
```

Download and **install Wolfram Engine** \(check https://www.wolfram.com/engine/  for the latest version number\):

```sh
cd /tmp && wget -O math.sh 'https://account.wolfram.com/dl/WolframEngine?version=14.0&platform=Linux&downloadManager=false&includesDocumentation=false' && chmod +x math.sh && sudo ./math.sh
```

**Start Wolfram Engine** and sign in using your account:

```sh
(compute-server-49) /tmp$ wolframscript 
The Wolfram Engine requires one-time activation on this computer.

Visit https://wolfram.com/engine/free-license to get your free license.

Wolfram ID: wstein@sagemath.com
Password: 
Wolfram Engine activated. See https://www.wolfram.com/wolframscript/ for more information.
Wolfram Language 14.0 Engine for Linux x86 (64-bit)
Copyright 1988-2023 Wolfram Research, Inc.

In[1]:= FactorInteger[2024]                                                        

Out[1]= {{2, 3}, {11, 1}, {23, 1}}

In[6]:= Quit[];                                                                    
(compute-server-49) /tmp$ 
```

**Install the [Wolfram Jupyter kernel](https://github.com/WolframResearch/WolframLanguageForJupyter)**:

```sh
cd /tmp && git clone https://github.com/WolframResearch/WolframLanguageForJupyter.git && cd WolframLanguageForJupyter && ./configure-jupyter.wls add
```

Confirm Jupyter is installed:

```sh
(compute-server-843) /tmp/WolframLanguageForJupyter$ jupyter kernelspec list
Available kernels:
  wolframlanguage14    /home/user/.local/share/jupyter/kernels/wolframlanguage14
  python3              /usr/local/share/jupyter/kernels/python3
```

Test Jupyter in the terminal. This can take a long time the first time.  The speed can depend on the type of compute server you selected.

```sh
(compute-server-843) /tmp/WolframLanguageForJupyter$ jupyter console --kernel=wolframlanguage14
Jupyter console 6.6.3

Wolfram Language/Wolfram Engine Copyright 2019

Note: This Jupyter kernel was installed through the WolframScript install method. Accordingly, updates to a WolframLanguageForJupyter paclet will not affect this kernel.
In [1]: FactorInteger[2024]
Out[1]: {{2, 3}, {11, 1}, {23, 1}}
```

Finally **use a Jupyter notebook** by creaeting a new Jupyter notebook in your project, then selecting your compute server from the dropdown menu in the upper left.

![](.mathematica.md.upload/paste-0.918463772000452)

<br/>

You can also use AI to generate Mathematica code:

![](.mathematica.md.upload/paste-0.13814876093177664)

![](.mathematica.md.upload/paste-0.25250741490715156)

![](.mathematica.md.upload/paste-0.3677519927791366)
