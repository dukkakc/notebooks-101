## Reverse Proxy Service (HTTPS)
* Connection to Notebook over HTTPS using the [Reverse Proxy Service](https://github.com/sdsc-hpc-training-org/reverse-proxy)  (very secure)

### Overview

![arch](https://github.com/sdsc-hpc-training-org/notebooks-101/raw/master/Docs/images/Reverse-Proxy-Service-for-Secure-Jupyter-Notebooks-Arch.png?raw=true)

The SDSC Reverse Proxy Service is a prototype system that will allow users to launch standard Jupyter Services on on any Comet compute node using a reverse proxy server using a simple bash script called `start_notebook`. The notebooks will be hosted on the internal cluster network as an HTTP service using standard jupyter commands. The service will then be made available to the user outside of the cluster firewall as an HTTPS connection between the external users web browser and the reverse proxy server. The goal is to minimize software changes for our users while improving the security of user notebooks running on our HPC systems. The RPS service is capable of running on any HPC system capable of supporting the RP server (needs Apache).

Using the RPS is very simple and requires no tunneling and is secure (produces HTTPS URLs). To use RPS, SSH to connect to comet, and make sure that you have the software environment installed on the login node. Verify that you have installed the required software: `Anaconda`,  `conda`, `Jupyter` (notebooks, lab), and other Python packages needed for you application.

### Clone the RPS repository
Clone [this](https://github.com/sdsc-hpc-training-org/reverse-proxy) repository directly into your comet login node.  
```
git clone https://github.com/sdsc-hpc-training-org/reverse-proxy.git
```

### Launching the Notebook

The user can run a notebook in one of two ways:
* Obtain an *interactive node* using the `srun` command, and running the `start_notebook.sh` script on the interactive node.
* Building a batch script, submitting the job to the batch queue, and waiting for the batch job to start running, at which time the `start_notebook.sh` script will start running.

The `start_notebook.sh` script performs the following tasks:
* Sends a request to the reverse proxy server (RPS) to get a one-time token and a port number
* Launches the jupyter notebook command using the token and port number.
* Prints the token to the terminal, so that the user can copy/paste the URL into a local browser:


### Usage

`./start_notebook.sh [-p <string>] [-d <string>] [-A <string>] [time]`

```
Default Dir: /home/$USER
Default Allocation is your sbatch default allocation
Default Time is 30 mins
```
(If you don't know what $USER is, try this command: `echo $USER`. This is just your comet username)

### Some common examples
Start a notebook with all defaults
`./start_notebook`

This is your waiting screen. This screen occurs before your batch job is submitted.
![alt text](https://github.com/sdsc-hpc-training-org/reverse-proxy/blob/master/.examples_images/ex1.png?raw=true)

Your notebook is ready to go!
![alt text](https://github.com/sdsc-hpc-training-org/reverse-proxy/blob/master/.examples_images/ex2.png?raw=true)

If you refresh too soon, you may see this page. This is expected and you'll just have to wait.
![alt text](https://github.com/sdsc-hpc-training-org/reverse-proxy/blob/master/.examples_images/ex3.png?raw=true)


Start a notebook in the debug queue
`./start_notebook -d ~ -p debug 30`

Start a notebook in the compute queue
`./start_notebook -d ~ -A ddp363 -p compute 60`

### Arguments
* [-b <string>] the batch script you want to submit with your notebook. Only those in the `batch` folder are supported.
* [-p <string>] the partition to wait for. debug or compute
* [-d <string>] the top-level directory of your jupyter notebook
* [-A <string>] the project allocation to be used for this notebook
* [time]        the amount of time in minutes to run this notebook for
