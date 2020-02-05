---
title: Setup Guide
layout: single
sidebar:
  nav: docs
permalink: "/docs/setup-guide"
excerpt: How to quickly install and setup your cloud IDE to run the Amazon FreeRTOS Labs.
last_modified_at: '2019-11-06 09:53:43 +0800'
toc: true
toc_label: Contents
---

The following Setup Guide is designed to help you get up and running quickly for the workshop.

For this, we will be using [AWS Cloud9 Development ](https://aws.amazon.com/cloud9/) environment. AWS Cloud9 is a cloud development IDE that we will use to compile and build our Amazon FreeRTOS code.

Once you have setup the environment, you will be able to build and compile the different labs, you will download the binary compiled firmware files, and you will flash your device locally on your laptop.

**ProTip:** You can also follow the [Local Setup Guide]({{ '/docs/local-setup-guide' | relative_url }}) to setup the environment for local development.
{: .notice--info}


## Create Cloud9 Environment

Log in to your AWS Account Console and search for Cloud9

![Search for Cloud9]({{ "/assets/images/docs/setup-guide/c9-00-search.png" | relative_url }})

Create a New Environment by clicking the **Create Environment** button

![Create the Cloud9 env]({{ "/assets/images/docs/setup-guide/c9-01-create-env.png" | relative_url }})

Name your environment and provide a description, then press **Next step**

**Warning** If you are running this workshop within the same account as other people, please make sure to name your environment with a customized name of your choosing in order to find your environment. Why not your name?
{: .notice--danger}

![Start]({{ "/assets/images/docs/setup-guide/c9-02-env-name.png" | relative_url }})

Now configure your Cloud9 instance:
- For Environment type, choose **Create a new instance for environment (EC2)**
- Choose **t2.micro** for the instance type
- Choose **Amazon Linux** for the Platform
- Press **Next step**

![Configure the Cloud9 env]({{ "/assets/images/docs/setup-guide/c9-03-env-settings.png" | relative_url }})

- Review and press **Create environment**

![Review the Cloud9 env]({{ "/assets/images/docs/setup-guide/c9-04-env-review.png" | relative_url }})

Open new Terminal window

![Open Cloud9 terminal]({{ "/assets/images/docs/setup-guide/c9-04-new-terminal.png" | relative_url }})

## Install ESP32 Toolchain

Download the 64-bit version of Xtensa ESP32 toolchain:

```bash
wget https://dl.espressif.com/dl/xtensa-esp32-elf-linux64-1.22.0-80-g6c4433a-5.2.0.tar.gz -P ~/
```

Install the toolchain in the *esp* directory and unzip the tar archive there:

```bash
mkdir -p ~/environment/esp
cd ~/environment/esp
tar -xvzf ../../xtensa-esp32-elf-linux64-1.22.0-80-g6c4433a-5.2.0.tar.gz
```

Add the toolchain path to **~/.bash_profile** PATH variable

```bash
echo "PATH=\$PATH:\$HOME/environment/esp/xtensa-esp32-elf/bin:/usr/local/bin" >> ~/.bash_profile
```

Re-evaluate ~/.bash_profile

```bash
source ~/.bash_profile
```

## Install dependencies

First update pip

```bash
sudo pip install --upgrade pip
```

Now lets install CMAKE and required dependencies for the project:

```bash
pip install argparse cryptography serial pyserial pyparsing==2.0.3 cmake ninja
```

## Done

You are done with the IDE setup and are ready to move to [Lab 0 - Setup the Labs]({{ "/labs/00-setup-the-labs" | relative_url }}).
