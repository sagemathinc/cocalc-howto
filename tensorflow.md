# Using Tensorflow with and without a GPU on CoCalc

VIDEO: https://youtu.be/aGHxjVHgOr8

This tutorial is about how to use tensorflow on [CoCalc](https://cocalc.com) with a GPU. It covers:

- creating a compute server with a GPU and installing tensorflow \(if necessary\)
- verifying that tensorflow has access to the GPU
- run a simple example of training a model

In this tutorial we will go through [the official Tensorflow Quickstart](https://www.tensorflow.org/tutorials/quickstart/beginner) on CoCalc in a few environments:

- a CoCalc project with no GPU
- a budget spot instance T4 GPU on Google Cloud using the Tensorflow Image
- a high\-end H100 GPU on Hyperstack Cloud using the Anaconda Image

## CoCalc Project with No GPU

Open any [CoCalc](https://cocalc.com) project, then click \+New and paste in the URL are of the notebook.

```
https://storage.googleapis.com/tensorflow_docs/docs/site/en/tutorials/quickstart/beginner.ipynb
```

![](.tensorflow.md.upload/paste-0.7241903121971556)

NOTE: If you are using CoCalc for free, then instead you should [download the notebook from here](https://www.tensorflow.org/tutorials/quickstart/beginner) to your laptop, then upload it to CoCalc by clicking on the Explorer, then dragging and dropping the notebook.

Once you load the notebook select "Edit \-&gt; Clear Output \-&gt; Clear All Cell Outputs" to clear all of the output, then go through and evaluate each cell.  it should take about a minute. 

![](.tensorflow.md.upload/paste-0.08732476430609948) You can see that there is no GPU by running this code:

```py
import tensorflow as ts
tf.test.is_gpu_available()
```

## CoCalc Compute Server with a Budget GPU

Click the Server butotn on the action bar to the left, then click "T4" in the templates at the top, select the first template, then make sure the provisioning model is "Spot" and the Image is "Tensorflow":

![](.tensorflow.md.upload/paste-0.8146195776916769)

Start the compute server and make a duplicate of the Jupyter notebook via "File \-\-&gt; Duplicate File":

![](.tensorflow.md.upload/paste-0.9671277126444764)

Then set the duplicate to run on your new compute server:

![](.tensorflow.md.upload/paste-0.5687743632113116)The compute server will take about 5 minutes to fully start up.  While waiting, delete all the output again, like you did above.  Also, since this is a _**budget GPU server**_ it might not start up at all if there are no resources available \-\- if this happens and the server is not running, you can click "Settings", then try a different geographical region and zone \-\- you might try to select a more obscure/unpopular location \(unfortunately, Google doesn't provide any information about actual popularity so you just have to try\):

![](.tensorflow.md.upload/paste-0.40406404932880524)

Once everything is running, step through and run all of the code in the notebook.  

## CoCalc Compute Server with a High End H100 GPU

As above, create a new compute server, but click on the H100 filter at the top, and select the "Python \(Anaconda\)" template on Hyperstack.   Alternatively, select the Hyperstack cloud, then select the "Python \(Anaconda\)" image. This costs a lot but is a very high end GPU, it is not a spot instance, and there is clear information about availability.

![](.tensorflow.md.upload/paste-0.024372453515674408)Just as before, duplicate the notebook and clear the output, then set this third notebook to run on your new compute server. You can do this while the compute server is starting up \(which should take about 5 minutes\), and you can use multiple compute servers in the same project at once:

![](.tensorflow.md.upload/paste-0.3891880316322305)A difference here is that tensorflow is NOT installed by default.  You can install it by typing this in a cell, which should take about a minute to run:

```sh
! conda install -y tensorflow
```

<img src=".tensorflow.md.upload/paste-0.8213492422418471"   width="715.852px"  height="365.203px"  style="object-fit:cover"/>

Once that is done, run all of the code as before.

Hint: You can click on the first cell to run, then shift click on the last cell, then type shift\+enter to run all selected cells.

Observe that even on this trivial toy example, the H100 GPU is MUCH faster than the T4 or CPU training:

![](.tensorflow.md.upload/paste-0.9643330354916679)

Thanks!  Please let us know if you have any questions or comments.  Compute servers are an amazing collaborative powerful way to use Tensorflow.

