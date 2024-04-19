# Use Mojo on CoCalc

VIDEO: https://youtu.be/qTYECdPGkJs 

You can easily install and use [Mojo](https://www.modular.com/max/mojo) on https://cocalc.com as explained in this new tutorial, which should take about 10 minutes to follow.

Mojo is a new programming language that "combines the usability of Python with the performance of C, unlocking unparalleled programmability of AI hardware and extensibility of AI models."

In [the tutorial](https://github.com/sagemathinc/cocalc-howto/blob/main/mojo.md), you will create a compute server, install Mojo in two minutes, use the Mojo Jupyter kernel, edit and run .mojo files and call Python code from Mojo.    Along the way you can try out any of the new features of Mojo and go through [the Mojo intro manual](https://docs.modular.com/mojo/manual/) to find out what this new programming language really feels like.

## Install Mojo

Create any [compute server](https://doc.cocalc.com/compute_server.html) by clicking "Servers", then "Create Compute Server", then:

- Switch to a Spot instance for a big discount.
- Disable the Nvidia GPU -- it's of no use yet with Mojo.
- Use a Python image and 10GB disk (other images will also work).

Start the compute server.  Create a Linux terminal on
the compute server by:

- Click +New --> Linux Terminal
- Set the terminal to run on your new compute server by clicking on the "Project" dropdown and selecting your compute server:

<img src=".mojo.md.upload/paste-0.6508116145416152"   width="699.723px"  height="178.608px"  style="object-fit:cover"/>

- Go to https://developer.modular.com/download, sign in if necessary, select the Linux instructions, and follow the directions exactly!  It should just work.

```sh
curl -s https://get.modular.com | sh -

modular auth
```

This sets up the Mojo package manager.  Next, install mojo:

```sh
modular install mojo
```

It looks like this in CoCalc:

![](.mojo.md.upload/paste-0.14566807107277535)

Finally, at the end it will suggest that you copy/paste the following code to add mojo to your PATH, which you should do:

```sh
BASHRC=$( [ -f "$HOME/.bash_profile" ] && echo "$HOME/.bash_profile" || echo "$HOME/.bashrc" )
echo 'export MODULAR_HOME="/home/user/.modular"' >> "$BASHRC"
echo 'export PATH="/home/user/.modular/pkg/packages.modular.com_mojo/bin:$PATH"' >> "$BASHRC"
source "$BASHRC"
```

## Using the Mojo Jupyter kernel

Create a new Jupyter notebook, select your compute server from the server dropdown, and then select the Mojo kernel.  If it isn't immediately visible, you can click "Kernel \-\-&gt; Refresh Kernel List".  You can now use Mojo:

![](.mojo.md.upload/paste-0.20356707742432523)

Right now it seems to take about 4s to compile anything.  Using a faster compute server \(e.g., with 32 cpus\) doesn't seem to make any difference.

## Use Mojo Files

You can edit and run .mojo files as follows.

First create a file that ends in `.mojo` and edit it by click \+New, then type e.g., "gcd.mojo". 

Get a terminal next to the mojo file by click Code \-\-&gt; New Frame \-\-&gt; Terminal.  For both the code and the terminal, click the yellow "Server" icon and set things so you are working on the compute server instead of the project:

![](.mojo.md.upload/paste-0.2648543849777911)  

Once you move the code and terminal to the compute server things will look like this:

![](.mojo.md.upload/paste-0.5324591448842453)

Here's the gcd code if you want to try it:

```py
def gcd(a:Int, b:Int) -> Int:
    var c : Int = 0
    while b:
        c = a % b
        a = b
        b = c
    return a

struct XGCD:
    var x: Int
    var y: Int
    var g: Int

    def __init__(inout self, a: Int, b: Int):
        var prevx : Int = 1;
        var prevy : Int = 0;
        self.x = 0;
        self.y = 1;

        while b:
            var q = a // b
            var r = a % b
            self.x, prevx = prevx - q * self.x, self.x
            self.y, prevy = prevy - q * self.y, self.y
            a, b = b, r
        self.x = prevx
        self.y = prevy
        self.g = a

    fn dump(self):
        print(self.x, self.y, self.g)

def xgcd(a:Int, b:Int) -> XGCD:
    return XGCD(a,b)

def main():
    xgcd(15,35).dump()
```

## Use Python from Mojo

You can use any python libraries from Mojo, as explained in [their docs](https://docs.modular.com/mojo/stdlib/python/python.html).
If you need to install python libraries, use `sudo pip install ...` in a terminal on the compute server.

Here is an example:

```py
from python import Python
var np = Python.import_module("numpy")
var ar = np.arange(15).reshape(3, 5)
print(ar)
print(np.sin(ar))
```

In a Jupyter notebook it would look like this:

![](.mojo.md.upload/paste-0.1630590775139753)

## AI Integration

CoCalc has extensive AI integration, but the models from OpenAI and Google don't seem to know how to write Mojo yet, so they are mostly not useful to generate code.  The explain functionality does seem to work pretty well with GPT4 though:

![](.mojo.md.upload/paste-0.09121604589971533)

### Mixtral

It turns out [the Dolphin\-Mixtral model](https://ollama.com/library/dolphin-mixtral) has some training on writing Mojo code.  You can use the OpenWebUI GPU compute server image (with two L4 GPU's) and install this model, and then ask it to write code for you.  Unfortunately, this isn't yet integrated with CoCalc's jupyter notebooks. 

![](.mojo.md.upload/paste-0.5856450432007869)

