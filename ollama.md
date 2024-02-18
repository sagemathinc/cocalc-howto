# Efficiently Running and Managing Large Language Models using a GPU on CoCalc.com with Ollama: A Tutorial

## Feb 18, 2024

To use OpenWebUI \(formerly ollama\-webui\) in CoCalc is now very easy! 

Step 1. Create a compute server with a GPU, and select the **OpenWebUI \(Ollama\)** image:

<img src=".ollama.md.upload/paste-0.8380651939698733"   width="736.99px"  height="336.40599999999995px"  style="object-fit:cover"/>

Step 2: Use it as explained in the blue box below the image.

\(I'll provide more details and tips here later, along with a new video.\)

---

**\*\* Everything below is basically outdated! \*\***

## Feb 6, 2024

In order to install the absolutely latest version of ollama\-webui with ollama, please do the following:

Step 1. Make a compute server with a GPU and use the "CUDA Development" image.

Step 2. Create a new Linux Terminal in CoCalc, and set the computer server where it runs to your new server.

Step 3. Clone the ollama\-webui GitHub repository:

```sh
git clone https://github.com/ollama-webui/ollama-webui
```

Step 4.  Install lspci

```sh
sudo apt install pciutils
```

Step 5.  Setup the Docker images for ollama and ollama\-webui \-\- this will take about 5 minutes

```sh
cd ollama-webui
./run-compose.sh --enable-gpu --webui[port=80]

# Press return after you see the "Current Setup" display!
```

You may have to press enter a few times to get it going.  If you setup a compute server with two GPU's, include the option `--enable-gpu[count=1]` .

You will see this output:

```sh
(compute-server-927) ~/ollama-webui$ ./run-compose.sh --enable-gpu --webui[port=80]
Enabling GPU with 1 GPUs

Current Setup:
   GPU Driver: nvidia
   GPU Count: 1
   WebAPI Port: Not Enabled
   Data Folder: Using ollama volume
   WebUI Port: 443

Do you want to proceed with current setup? (Y/n): 


[+] Running 19/19
 ✔ ollama-webui 14 layers [⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled            45.2s 
   ✔ c57ee5000d61 Pull complete                                               0.5s 
   ✔ be0f2e005f57 Pull complete                                               0.3s 
   ✔ eab129fe7d73 Pull complete                                               0.5s 
   ✔ dd24933c9a93 Pull complete                                               0.6s 
   ✔ 3fcbdacf3969 Pull complete                                               0.7s 
   ✔ f2c1aef8c7fd Pull complete                                               0.7s 
   ✔ 2290acfa02c4 Pull complete                                               0.8s 
   ✔ ae0c48d3727f Pull complete                                               3.3s 
   ✔ 186b1fe8276c Pull complete                                               2.9s 
   ✔ 2b65a57a2cbc Pull complete                                               3.0s 
   ✔ 4a7e14be505e Pull complete                                               3.0s 
   ✔ affcdaf37b75 Pull complete                                               3.8s 
   ✔ 784f83807364 Pull complete                                               3.3s 
   ✔ 254bce9c0454 Pull complete                                               3.4s 
 ✔ ollama 3 layers [⣿⣿⣿]      0B/0B      Pulled                               7.0s 
   ✔ 57c139bbda7e Pull complete                                               0.3s 
   ✔ efa866b73628 Pull complete                                               0.3s 
   ✔ b930de02cbec Pull complete                                               2.7s 
[+] Running 5/5
 ✔ Network ollama-webui_default        Crea...                                0.1s 
 ✔ Volume "ollama-webui_ollama"        Crea...                                0.0s 
 ✔ Volume "ollama-webui_ollama-webui"  Created                                0.0s 
 ✔ Container ollama                    Started                                4.8s 
 ✔ Container ollama-webui              Started                                0.2s 

Compose project started successfully.
```

You can type `docker ps -a` in your terminal to see that some ollama containers are running!

Step 6.  Connect to your server and make an admin account.  The top of your server config will look like this:

![](.ollama.md.upload/paste-0.8772460509571076)

Copy and paste the ip address, with http:// in front of it \(not https!\).

You now have the latest ollama\-webui running and you can install any models, etc.  Here's my first test just now:

<img src=".ollama.md.upload/paste-0.1151416617247496"   width="592.77px"  height="492.562px"  style="object-fit:cover"/>

**IMPORTANT:** this setup does not have any SSL cert, so your password is sent in the clear.   Please do not consider this secure.  An older version is described below that is secure, and we will also create an image very soon that has SSL cert stuff built in \(like our older ones below\)! 

## Older directions, videos, etc.

---

**Videos:** 

- complete demo -- https://youtu.be/1xA6pXMZFIU 
- demo of latest version using a multimodal model -- https://youtu.be/X5fbG-QAxDs

UPDATES:

- Dec 16, 2023 \-\- Mixtral works extremely well with 2xL4's, but you need to explicitly update ollama in a terminal by typing `curl https://ollama.ai/install.sh | sh` then restarting your server \(or kill the running olama server and do `ollama serve` \).  It takes about 5\-10 minutes to download the weights, and uses about 30GB additional disk space.  Inference is very fast on a 2xL4's, for \$1/hour.
- Dec 14, 2023 \-\- updated ollama and ollama\-webui to support multimodal models!<img src=".ollama.md.upload/paste-0.5831256738722361"   width="536.544px"  height="405.84400000000005px"  style="object-fit:cover"/>

---

This tutorial is designed to guide you through running Large Language Models seamlessly on [CoCalc.com](http://CoCalc.com) using [Ollama](https://ollama.ai/). We will cover the step\-by\-step process to set up a compute server, navigate Ollama's extensive library of models, and manage server resources for optimised performance. With **prices starting at \$0.13/hour**, we aim to provide an accessible and cost\-effective solution for language model experimentation.

![](.ollama.md.upload/paste-0.27876508292174473)

## Create a Compute Server with a sufficiently large GPU

You can [browse a large number of models](https://ollama.ai/library). Once you have a sense of what you want to run, create a compute server with a GPU that has enough RAM. You typically want the instance and the GPU itself to have at least the following amount of RAM.  Also, see the guidelines regarding disk space below.

- 3b models \- anything
- 7b models require about 8GB of RAM
- 13b models require about 16GB of RAM  \(example: a single L4 GPU\)
- 30b models require about 32GB of RAM \(example: 2 x L4 GPU's\)
- 70b models require about 64GB of RAM   \(example: 4 x L4 GPU's\)

Currently, CoCalc provides T4, L4, and A100 GPU's. The T4 GPU's have 16GB RAM each, the L4's have 24GB RAM each, and there are A100's with either 40GB or 80GB RAM.
If you need say 64GB of RAM, you could use 4 L4's or a single A100 80GB or two A100 40GB's. For example, here I've configured a compute server with 4 x L4's in Taiwan for \$1.63/hour. The price is low because this is a spot instance \-\- it might not be available or might get killed \-\- but for experimenting with ollama, a spot instance can be a great choice!

When you're browsing [the page about a model](https://ollama.ai/library/phind-codellama), click on ["Tags"](https://ollama.ai/library/phind-codellama/tags) to see exactly what you can download.  Under each tag there is a size, e.g., "19GB". Make sure your free disk space is bigger than this.   If you run out of disk space while downloading a model, at any point you can click "Edit" on your compute server, then increase the disk space, and in a few seconds your disk will be larger. You can type `df -h /` in the terminal to see free disk space.

**WARNING:** Though you can change the number of GPU's after you run the compute server and stop it, in many cases **it won't boot** due to subtle changes in the underlying hardware of the VM.   If you do that, change the number of GPU's back to what it was before and the server will work again.   It's easy to make as many compute servers as you want at the same time in any project, but you have to download models separately to each one \(there is no cost to download models\).   

## Quickstart \-\- use the official Ollama image

THE REST OF THIS TUTORIAL WAS BEFORE THE NEW OLLAMA Image \-\- now choose that image instead of CUDA:

![](.ollama.md.upload/paste-0.04944021143349042)

Once the compute server starts up, open the URL and you can use ollama\-webui.  Open a terminal and select your ollama image, and you can check on disk space, look at logs in /var/log/supervisor, or do everything else described below.  You should definitely read the rest of the tutorial below because though ollama\-webui is a nice frontend, it provides much less insight into what is going on regarding downloading or running models than the terminal. In particular:

```sh
(compute-server-109) ~/ollama$ ollama run orca-mini  # see progress as model gets pull
# once it works, refresh the webui and use it there.

(compute-server-109) ~/ollama$ htop   # see process

(compute-server-109) ~/ollama$ tail -f /var/log/supervisor/ollama-std*    # see logs
```

**Using spot L4 GPU's is also a very good choice, so things are pretty fast even with larger models.**

---

# From Scratch Tutorial

Also, use "CUDA Toolkit" as the image, since ollama is self contained software, and you won't be using PyTorch and Tensorflow.  This does give you the ability to also easily build any version of ollama from source.

![](.ollama.md.upload/paste-0.5447573942131758)

Once you create your GPU instance, it will start running. It can take a
couple of minutes to boot up, especially if it is large.
If it immediately stops, this is due to it being a spot instance.
If this happens, try changing the region and try again (the instance
must be deprovisioned to change the region), or switch to a Standard
instance, which costs more.   

## Install Ollama

Open a separate terminal (+New --> Linux Terminal),
then in the upper left server selector, select your 
compute server:

![](.ollama.md.upload/paste-0.33685539193524705)

You may need to click "Sync Files" after selecting your compute
server.

Next paste the following into the terminal to install the latest
version of ollama:

```sh
sudo apt install -y lshw && curl https://ollama.ai/install.sh | sh
```

The installation should take only a few seconds:

![](.ollama.md.upload/paste-0.8231679313741955)

## Try it out!

You can now use all of the ollama commands and everything 
should just work, unless you run out of disk space or
GPU RAM. To get going, type `ollama serve` in your terminal

```sh
ollama serve 
```

Then create another terminal, set it to run on the same compute server,
and you can then use any ollama commands.

![](.ollama.md.upload/paste-0.74623693340403)

Above, I used the split terminal button in the upper right, then set the new
terminal to run on the compute server, and ran [the mistral model](https://ollama.ai/library/mistral):

```sh
ollama run mistral
```

Once the model is downloaded, inference should be extremely fast, and look like this:

![](.ollama.md.upload/paste-0.590820171896622)

## Run a LARGE language model!

Since I configured this compute server with 4 x L4 GPU's, I have 96GB GPU RAM, so can run any of the models.  However, there isn't enough disk space to download a large model:

```sh
(compute-server-59) ~/ollama$ df -h /
Filesystem      Size  Used Avail Use% Mounted on
overlay          39G   38G  1.5G  97% /
(compute-server-59) ~/ollama$ 
```

To increase the disk space, click the bar or label to the right of "Sync Files" at the top of your terminal, click "Edit", scroll down in the model, and add disk space.  I will add 100GB, so I have plenty of space to use the [70B LLAMA2 model](https://ollama.ai/library/llama2):

![](.ollama.md.upload/paste-0.5043058417602853)

Click the bar again to hide the details display.  Confirm in the terminal that you have the extra disk space:

```sh
(compute-server-59) ~/ollama$ df -h /
Filesystem      Size  Used Avail Use% Mounted on
overlay         136G   38G   99G  28% /
```

OK, let's grab the 70B parameter llama2!

```sh
ollama run llama2:70b
```

<br/>

![](.ollama.md.upload/paste-0.3741336509938804)

<br/>

This will download the entire model to your compute server, which takes about 5 minutes.  (NOTE: There is no cost associated with downloading models to your compute server.) 

While waiting for the download and decompressing of data, it can be fun to open other terminals to keep an idea on cpu usage (e.g., type `top`) and disk space (`df -h`):

<br/>

![](.ollama.md.upload/paste-0.15724897662736326)

<br/>

Once your large language model is up and running, you can confirm that it is running on the GPU, and see how much memory it is using, by typing `nvidia-smi` in the terminal.   In my case, we see that about 11GB is used on each of the four L4 GPU's.  I should have rented T4 GPU's instead! 

<br/>

![](.ollama.md.upload/paste-0.9767547482392038)

<br/>

Also, you can see above what CoCalc is, in case you're curious, and yes LLama2's description is very good.  And yes, if you invite a collaborator to your cocalc project, they can open a terminal and use your ollama compute server.

## Cleaning Up

When you're done, you can either stop your compute server, so that you can easily use any models your downloaded later, or you can deprovision it.  When stopped you pay only for the allocated disk, and by hovering over Stop and the price you can see exactly what the hourly and monthly cost of that data storage is:

![](.ollama.md.upload/paste-0.43454729198940467)

NOTE: when you make the compute server, you can choose a much cheaper non-SSD, which is a very reasonable choice for storing these ollama models.

"If you instead click "Edit", then choose "Deprovision", the disk and the models that you downloaded will be deleted." You then don't have to pay anything at all, but next time you want to use ollama, you have to reinstall it and download the model you want to use.  You can also create as many different compute servers as you want in the same CoCalc project.

## Conclusion

Congratulations on getting your Large Language Model up and running on CoCalc using Ollama! You can now easily experiment, deploy, and manage a wide range of language models. Do remember to efficiently manage your resources and stop or deprovision your compute server when not in use. Feel free to create different compute servers in your CoCalc project based on your needs. We hope you enjoyed this tutorial,  and continue exploring the possibilities of language models. Happy CoCalcing!

