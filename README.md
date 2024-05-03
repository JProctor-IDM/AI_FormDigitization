# AI_FormDigitization

This is a repository aimed at sharing code with partners on how to perform record digitization.  This code is a prototype and designed to look at a specific type of form to be digitized by the Carter Center to support the Guinea worm eradication effort.  

The motivation for this approach is that standard OCR tools do not seem to quickly and with high fidelity extract the structured data.  This approach was to see if GPT can help mitigate the issues from standard techniques.  For the training data, this workflow worked quite well with high accuracy.  

## The workflow  

This is a two part workflow.  

### The OCR step using the unstructured package

First, we implement the python package unstructured to perform the OCR extraction.  In particular, I used the docker container provided by unstructured:  https://unstructured-io.github.io/unstructured/installation/docker.html.  

To run the jupyter notebook in this repository, there are a number of steps that I needed to perform to run the jupyter notebook in the container, mount the local drive into the container, and port an IP address from the container to the local browser.  

Here are the details for running on a mac with apple chip:

1.  I needed to install Rosetta 2 and docker for Mac apple chip.

2.  Download the image:  https://unstructured-io.github.io/unstructured/installation/docker.html via 
docker pull downloads.unstructured.io/unstructured-io/unstructured:latest 

3. Make sure docker desktop is running.

4.  Need to run the docker command to create a container based on that image.  Also need to mount the local volumes and make sure the container port aligns with the local port for the jupyter notebook.  Note that I kept my own local path in the command below.  Insert your own as you need.

docker run -dt --name unstructfinal -v /SRC/TestUnstructured:/host -p 127.0.0.1:2023:8888 downloads.unstructured.io/unstructured-io/unstructured:latest

5.  Now you should have a running container called unstructured.  You can bash into that container:  

docker exec -it unstructured bash

6.  Now, you can navigate to the /host folder.  Go to the root directory, find the host directory.

7.  In the host directory start a jupyter notebook 

jupyter notebook --ip=0.0.0.0

8.  In the jupyter notebook output in the terminal, you have to copy and paste the http url for the notebook.  It looks something like this:  
 http://127.0.0.1:8888/tree?token=76be311b27f2903c34354205fef04123e3e4b3f465be538c
 
Change the port number from 8888 to 2023:

http://127.0.0.1:2023/tree?token=76be311b27f2903c34354205fef04123e3e4b3f465be538c

9.  That should allow you to open a jupyter notebook interface in a local browser.

### The LLM step

I would recommend using anaconda to set up your environment to run the jupyter notebook.  There are only a few packages to install which can be easily seen in the notebook.  You will need to set up your own OpenAI account, acquire an API key, and potentially an organizational key.  These will need to be set as system variables.  
