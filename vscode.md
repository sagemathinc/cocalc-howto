# Using VS Code on CoCalc

There are many ways to quickly launch Visual Studio Code (VS Code) on https://cocalc.com.

VIDEO: https://youtu.be/c7XHYBDTplw

Open a project on https://cocalc.com, then with one click in the file explorer, launch VS Code running on the project. You can them install and use a Jupyter notebook inside VS Code, and edit Python code and using a terminal.

When you need more power, add a compute server to your project. For example, in the video we demo adding a compute server that has 128GB of RAM and the latest Google cloud n4 machine type. It's a spot instance, which is great for a quick demo. It's good to configure DNS and autorestart, and launch our compute server, watching it boot via the serial console. Once the server is running, launch VS Code with one click, use a Jupyter notebook, edit Python code, and open a terminal and confirm that the underlying machine has 128GB of RAM.

You can also make a CoCalc terminal that runs on the compute server by clicking "+New --> Linux Terminal", then clicking the Server button
and selecting your compute server.

This costs just a few cents, as you can confirm using the "Upgrades" tab (and scrolling down). When you're done, deprovision the server, unless you need to keep data that is only on the server.
