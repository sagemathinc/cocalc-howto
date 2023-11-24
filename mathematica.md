# How to use the Mathematica Jupyter Kernel on [CoCalc.com](http://CoCalc.com)

It is finally easy to run Mathematica Jupyter notebooks on https://cocalc.com via the free Wolfram Engine!  You only have to pay for the compute resources you use, which start at about \$0.02/hour.

Click "Servers" in any CoCalc project:

<br/>

![](.mathematica.md.upload/paste-0.48922211489959966)

<br/>

Then create a new **Compute Server** with the Python image and make sure you create a disk that is at least 15GB bigger than the minimum, e.g., 30GB would be a good choice:

<br/>

![](.mathematica.md.upload/paste-0.19421151586224683)

<br/>

Start your compute server, open a Linux Terminal in your project, and select your compute server in the upper left menu so that the terminal is running on the compute server.

<br/>

![](.mathematica.md.upload/paste-0.3489692762116898)

<br/>

**Install some packages** into the compute server:

```sh
sudo apt-get update && sudo apt-get install -y libgl1-mesa-glx libasound2 libxkbcommon-x11-0 libegl1 wget avahi-daemon
```

Download and **install Wolfram Engine** \(check https://www.wolfram.com/engine/  for the latest version number\):

```sh
cd /tmp && wget -O math.sh 'https://account.wolfram.com/dl/WolframEngine?version=13.3&platform=Linux&downloadManager=false&includesDocumentation=false' && chmod +x math.sh && sudo ./math.sh < /dev/null
```

**Start Wolfram Engine** and sign in using your account:

```sh
(compute-server-49) /tmp$ wolframscript 
The Wolfram Engine requires one-time activation on this computer.

Visit https://wolfram.com/engine/free-license to get your free license.

Wolfram ID: wstein@sagemath.com
Password: 
Wolfram Engine activated. See https://www.wolfram.com/wolframscript/ for more information.
Wolfram Language 13.3.0 Engine for Linux x86 (64-bit)
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
(compute-server-49) /tmp/WolframLanguageForJupyter$ jupyter kernelspec list
Available kernels:
  wolframlanguage13.3    /home/user/.local/share/jupyter/kernels/wolframlanguage13.3
  python3                /usr/local/share/jupyter/kernels/python3
(compute-server-49) /tmp/WolframLanguageForJupyter$ 
```

Test Jupyter in the terminal. This can take a long time the first time.  The speed can depend on the type of compute server you selected.

```sh
(compute-server-49) /tmp/WolframLanguageForJupyter$ jupyter console --kernel=wolframlanguage13.3

Jupyter console 6.6.3

Wolfram Language/Wolfram Engine Copyright 2019

Note: This Jupyter kernel was installed through the WolframScript install method. Accordingly, updates to a WolframLanguageForJupyter paclet will not affect this kernel.
In [1]: 

In [1]: FactorInteger[2025]
Out[1]: {{3, 4}, {5, 2}}

In [2]: Quit[]
Shutting down kernel
(compute-server-49) /tmp/WolframLanguageForJupyter$ 
```

Finally **use a Jupyter notebook** by creaeting a new Jupyter notebook in your project, then selecting your compute server from the dropdown menu in the upper left.

![](.mathematica.md.upload/paste-0.918463772000452)

