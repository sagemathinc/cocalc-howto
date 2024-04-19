# Using RStudio Server on [CoCalc](https://cocalc.com) via a Compute Server

VIDEO: https://youtu.be/Owq90O0vLJo

[CoCalc's compute servers](https://doc.cocalc.com/compute_server.html) let you very easily run arbitrary virtual machines associated to any CoCalc project.  The cost is by the second \(pay as you go\) for cloud hosted VM's.  

### Create a Compute Server

Create a new compute server \(e.g., a 16GB instance with 4 vCPUs on Google Cloud costs about a \$0.03/hour\) and select the R image:

![](.rstudio.md.upload/paste-0.0816161743016135)

Also, when creating your compute server, be sure to create a "Custom Domain Name with SSL":

![](.rstudio.md.upload/paste-0.8190500175006552)

For better performance, select your compute server to be in a region that is geographically close to you.  When using RStudio Server, all communication is directly between you and the compute server.

### Connect to RStudio Server

Once your compute servers is running \(about 1\-2 minutes\), click on the https link, paste in your random token, and you have the latest version of RStudio Server running.  In the example below, I visit https://rstudio.cocalc.cloud and I paste in that token `LmWIML...` .

![](.rstudio.md.upload/paste-0.12529211121764594)

RStudio then has full access to your files from your project and the CPU power of whatever compute server you're using:

<img width="1721" alt="image" src="https://github.com/sagemathinc/cocalc/assets/1276278/5cea60c0-a68a-41c3-bbfa-92482bfe9d68">

## It's all there \-\- R Markdown, R Notebooks, etc.

RStudio of course supports a wide range of document formats in the R ecosystem, including R Notebook, R Markdown, Shiny Apps, Quarto etc.

<img src=".rstudio.md.upload/paste-0.3798811744763373"   width="374px"  height="461px"  style="object-fit:cover"/>

## CoCalc also features Jupyter Notebooks

The RStudio hosted on a compute server does not have any support for realtime collaboration or AI integration, etc.   If you need all that, you can also use a Jupyter notebook running on your compute server.

<img width="1060" alt="image" src="https://github.com/sagemathinc/cocalc/assets/1276278/a170e419-bb84-409c-b43b-053dc78dac47">

## Compute Server Terminals

If you want a terminal on your compute server you can also just create a normal CoCalc Terminal \(outside R Studio\), via \+New \-\-&gt; Terminal, then move it to your R server via the dropdown.  This terminal is collaborative, has chat on the side, and you can use AI to get help.  You can also run R here.

<img src=".rstudio.md.upload/paste-0.3790893118581635"   width="677px"  height="365px"  style="object-fit:cover"/>

Then:

![](.rstudio.md.upload/paste-0.5013666302162143)

