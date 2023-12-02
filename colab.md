# Using the Google Colab Environment with and without a GPU Online Via CoCalc

Video: https://youtu.be/kcxyShH3wYE

In CoCalc you can use the same Python/R environments that are part of Colab, both with and without a GPU.  

To use Colab with a GPU on CoCalc, create a compute server with a GPU, and select the "Google Colab" image.  

Once your compute server is running you can open any Jupyter notebook or Terminal, and use the server selector in the upper left to run it on your compute server.  You can then use Colab with GPU acceleration. 

![](.colab.md.upload/paste-0.5777361984626368)

CoCalc also lets you directly run the Colab environment \(and Tensorflow and a million other things!\) completely for free without a GPU by just making a Jupyter notebook and NOT running it on a compute server.   

![](.colab.md.upload/paste-0.01372619388439289)

This is useful because:

- It is a single Python environment with excellent support for both PyTorch and Tensorflow at the same time.
- It makes it easier for you move code back and forth between Colab and CoCalc, depending on who you are collaborating with.
- It ensures code you write on CoCalc will work on Colab when you share it publicly.
- You may be able to find better deals on GPU's in CoCalc than you would directly on hosted Colab.

Note that both Colab and Cocalc have a way to run Jupyter kernels "on prem" on your own server.  For CoCalc the steps are exactly as above with a GPU, but instead of choosing a compute server on Google cloud, you select "on prem", and paste a line of code into a Virtual Machine that you start running on your own server. 

![](.colab.md.upload/paste-0.00412101785647101)
