# Using PyTorch with a GPU in a Jupyter Notebook Online Via CoCalc

Video: [https://youtu.be/JG6jm6yv\_KE](https://youtu.be/JG6jm6yv_KE) 

To use PyTorch with a GPU on CoCalc, create a compute server and enable a GPU, and select the PyTorch image.  This is the [official image hosted by Nvidia](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch), but [augmented with additional support for huggingface](https://github.com/sagemathinc/cocalc-compute-docker/blob/main/src/pytorch/Dockerfile).  Of course, you can easisly type `sudo su` and install anything that is missing.   

Once your compute server is running you can open any Jupyter notebook or Terminal, and use the server selector in the upper left to run it on your compute server.  You can then use PyTorch with GPU acceleration. 

Of course, CoCalc also lets you directly run PyTorch \(and Tensorflow and a million other things!\) completely for free without a GPU by just making a Jupyter notebook and NOT running it on a compute server. 
