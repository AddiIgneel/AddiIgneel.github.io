---
layout: post
title: Steps
---


This guide assumes you have created an account and are using trial version.

1. Click on IAM & ADMIN from left bar and then select Quotas. Update your account to Billing Account. There would be a blue button with option on right of the screen.

2. Select Region option and only select " ". After selection scroll down and select Tesla K80 GPU by click the box left to it. Choose Edit Quotas on top of the page. Provide your cell no and set the current limit to 1. Provide the justification. Wait for email. You will get e-mail from google confirming the increase in GPU.

![useful image]({{ AddiIgneel.github.io}}/images/20310_10203860305057904_1499559795408981273_n.jpg.png)

3. Select VPC Network from the left bar. Go to Firewall rules. Create a new rule. Fill the deatils as shown below in image.
4. Now go to Compute Engine. Select Images and then create new Image. Fill the next form as shown in picture below. Image will take 10-15 minutes to be created.
5. Click on storage from the left column. Create a bucket. Choose Regional option. 
6. Upload the "setting.zip" file to this bucket. The file can be downloaded from following link : 
7. Go to VM Instances and create new. Select the region. Select the RAM, CPU you like. Click customize and select GPU as shown below. 
Select change button shown in boot disk. Select Custom Images. Select the image created in step 4. 
7. Scroll down and select the tick box next to "Allow HTTP traffic and Allow HTTPS traffic"
8. Click on Management,Disks,Networking blue bar. Select Networking and type jupyter in network tag as shown below.
9. Click on create Instance. 
10. Click on ssh to launch the instance.
11. Create a new file with following command : "nano script.sh" and paste the following contents in it. Ctrl+C and Ctrl+V works
      
        #!/bin/bash
        echo "Checking for CUDA and installing."
        # Check for CUDA and try to install.
        if ! dpkg-query -W cuda-8-0; then
          # The 16.04 installer works with 16.10.
          curl -O http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
          dpkg -i ./cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
          apt-get update
          apt-get install cuda-8-0 -y
        fi
        
12. Use Ctrl+X and then Y to save the file.
13. Run the following command: chmod +x script.sh
14. Run the script by : sudo ./script.sh
15. Wait for the latest GPU drivers to be installed.
16. Run the following commands to install anaconda, jupyter notebook and tensorflow.
    
    1. wget https://repo.continuum.io/archive/Anaconda2-5.0.0-Linux-x86_64.sh
    2. bash Anaconda2-5.0.0-Linux-x86_64.sh
    3. Follow the guide and type yes when it prompts you. Click enter when it asks you to appened the path.
    4. Type yes when asked to include path.
    5. source .bashrc
    6. conda create -n tensorflow
    7. source activate tensorflow
    8. wget https://pypi.python.org/packages/04/c4/ffb89dbea9e43e82665ff088fd08aa25aa93301aa8c480de278c8f576ea1/tensorflow_gpu-1.0.1-cp27-cp27mu-manylinux1_x86_64.whl#md5=c06b11dee765a99b1814ca393aaf558a
    9. pip install tensorflow_gpu-1.0.1-cp27-cp27mu-manylinux1_x86_64.whl

16. Run the following commands one by one to install gcsfuse to use the bucket we created in step 5.
    
    1. export GCSFUSE_REPO=gcsfuse-`lsb_release -c -s`
    2. echo "deb http://packages.cloud.google.com/apt $GCSFUSE_REPO main" | sudo tee /etc/apt/sources.list.d/gcsfuse.list
    3. curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    4. sudo apt-get update
    5. sudo apt-get install gcsfuse
    6. mkdir mount
    7. gcsfuse <your bucket name> ./mount
    8. cp mount/setting.zip ./
    9. unzip setting.zip
    10. jupyter notebook --generate-config  ** the following command will print a path like /home/<your username>..../.jupyter/...
    12. copy the path till .jupyter
    11. mv mycert.pem <paste path here>
    12. mv jupyter_notebook_config.py <paste path here>
    13. Run jupyter notebook password and type a password.
 
 17. Go to VM instance page and copy the external IP for your instance. Open a new tab in browswer paste it and write :9999 after it. 
 18. Enter the password. Jupyter notebook should be working.
    
