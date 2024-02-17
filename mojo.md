# How to use Mojo on CoCalc via a Compute Server

You can easily install and use Mojo on https://cocalc.com as follows. 

## Install Mojo

Create any [compute server](https://doc.cocalc.com/compute_server.html) by clicking "Servers", then "Create Compute Server", then:

- Switch to a Spot instance for a big discount.
- Disable the Nvidia GPU -- it's of no use yet with Mojo.
- Use a Python image and 10GB disk (other images will also work).

Start the compute server.  Create a Linux terminal on
the compute server by:

- Click +New --> Linux Terminal
- Set the terminal to run on your new compute server by clicking on the "Project" dropdown and selecting your compute server:

![](.mojo.md.upload/paste-0.6508116145416152)

- Go to https://developer.modular.com/download, sign in if necessary, and see the page with instructions about how to install mojo.   In step 2 off to the right you'll see a step that looks like this:

```
curl https://get.modular.com | sh - && \
modular auth mut_d81762...b4c04
```

where the string `mut_d81762...b4c04` is your Mojo auth token. 
Paste the following in your terminal, replacing `mut_d81762...b4c04` with your personal Mojo auth token.  In about 2 minutes, mojo should be installed
into your compute server, along with a Mojo Jupyter kernel.

```sh
export MUT=mut_d81762...b4c04

sudo apt update && sudo apt install -y python3.10-venv && curl https://get.modular.com | sh - && modular auth $MUT && modular install mojo
```

It looks like this in CoCalc:

![](.mojo.md.upload/paste-0.6292145085370329)

... then ...

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
            let q = a // b
            let r = a % b
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
let np = Python.import_module("numpy")
let ar = np.arange(15).reshape(3, 5)
print(ar)
print(np.sin(ar))
```

In a Jupyter notebook it would look like this:

![](.mojo.md.upload/paste-0.1630590775139753)

## AI Integration

CoCalc has extensive AI integration, but the models from OpenAI and Google don't seem to know how to write Mojo yet, so they are mostly not useful to generate code.  The explain functionality does seem to work pretty well with GPT4 though:

![](.mojo.md.upload/paste-0.09121604589971533)

