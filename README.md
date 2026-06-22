# AI Application Demos of MD Robot Starter Kits
[MangDang](https://mangdang.store/) Online channel: [Discord](https://discord.gg/xJdt3dHBVw), [FaceBook](https://www.facebook.com/groups/716473723088464), [YouTube](https://www.youtube.com/channel/UCqHWYGXmnoO7VWHmENje3ug/featured), [Twitter](https://twitter.com/LeggedRobot)

MD Robot Starter Kits: Unlock your AI Dream Job.
Make robotics easier for schools, homeschool families, enthusiasts and beyond.

- Generative AI: Support ChatGPT, Gemini, and Claude
- ROS: support ROS2(Humble) SLAM & Navigation robot dog at low-cost price
- OpenCV: support OpenCV official OAK-D-Lite 3D camera module and single MIPI camera
- Open-source: DIY and customize what you want.
- Raspberry Pi: It’s super expandable and endorsed by Raspberry Pi.

# Overview

The AI applications can be run on MD Robot Starter Kits, including Mini Pupper and Mini Pupper 2.
Please click the picture and refer to the demo video.

[![Run on Mini Pupper 2](https://img.youtube.com/vi/mIDuIZCevIg/0.jpg)](https://www.youtube.com/watch?v=mIDuIZCevIg)


[![Run on Mini Pupper 2](https://img.youtube.com/vi/bvH-lA1IHig/0.jpg)](https://www.youtube.com/watch?v=bvH-lA1IHig)

# Software Installation

## Solution 1: Flash the pre-built image

You can download our pre-built images and try the AI functions quickly.
- Step 1: Download the pre-built base image file(like * AI *.img),  [Mini Pupper 2](https://drive.google.com/drive/folders/1_HNbIb2RDmHpwECjqiVlkylvU19BSfOh?usp=sharing) or [Mini Pupper 1](https://drive.google.com/drive/folders/1jJm_6qBIYGGp2dpZNm668D0eH1JpfCqn?usp=sharing)
- Step 2: Flash the image into the SD card.

## Solution 2: Build by Self

### Preparation

Please make sure Mini Pupper can walk first by the webserver. 

- Install the BSP package, [Mini Pupper 2](https://github.com/mangdangroboticsclub/mini_pupper_2_bsp) or [Mini Pupper 1](https://github.com/mangdangroboticsclub/mini_pupper_bsp)

- Install the [quadruped repo](https://github.com/mangdangroboticsclub/StanfordQuadruped )


### Install

For the video guide, please click the picture and refer to the demo video.

[![Installation Guide](https://img.youtube.com/vi/1AkhJi2o8rM/0.jpg)](https://www.youtube.com/watch?v=1AkhJi2o8rM)


Clone this repo.
```
cd ~
git clone https://github.com/mangdangroboticsclub/apps-md-robots
cd apps-md-robots
```

Copy your google cloud API key file to your robot and set your google cloud API key file location in .env file.
 
```
cd ~/apps-md-robots/
# copy a template file to edit
cp env.sample .env

# Edit .env file, set your key path in .env file,
# like: API_KEY_PATH=/home/ubuntu/xxxx.josn 
vim .env
```

Install the dependency libs.

```
cd ~/apps-md-robots/ai-app/
sudo apt-get install -y python3-pyaudio
sudo apt-get install -y libgl1
sudo apt-get install -y portaudio19-dev
sudo pip install msgpack --break-system-packages
sudo pip install pillow --break-system-packages
sudo pip install -r requirements.txt --break-system-packages 
sudo cp ai.service /etc/systemd/system/ai.service
```

# Run

## Run by Webserver
run app demos.

- Step 0: After your robot boot up and IP address shows on the robot LCD screen, point a web browser to x.x.x.x:8080 where x.x.x.x is the IP address of your mini_pupper, such as, 192.168.1.103:8080

![ipaddress](imgs/ipaddress.png)

- Step 1: Select "Pupper" option in the left menu, click the “Activate/Deactive” button, robot LCD screen will change to a yellow face.
- Step 1: Select "Settings" option in the left menu, click the “AI On” button, and wait about 10s after robot LCD screen shows "Hello World". 

![WebServer](imgs/webServer.jpg)


## Run by Command Line

connect the robot by ssh command.

![ssh](imgs/ssh.png)

```
ssh ubuntu@x.x.x.x
# the default password is mangdang
```

run app demos.
 
```
cd ~/apps-md-robots/api/
# Go to movement mode
python move_api.py --api init

# Start the generative AI service
# The display will show "Hello, World!" when it's ready.
sudo systemctl start ai
```

![HelloWorld](imgs/HelloWorld.png)


You can check the output when you debug

```
sudo journalctl  -f -u ai
```

![GeminiFeedback](imgs/GeminiFeedback.png)


If you want to DIY the key words to control the robot by voice, please revise here.

![keyWordofMovement](imgs/keyWordofMovement.png)


If you want to test the API, run the following command.
```
cd ~/apps-md-robots/api/
python move_api.py
```

If you want to test the camera module, run the following command.
```
ffmpeg -i /dev/video0 -vf 'scale=320:240' -vframes 1 -an -b:v 500k output.jpg
```

## Install OpenClaw

OpenClaw is an AI assistant that can run on MiniPupper, providing voice control and intelligent interactions.

### Prerequisites

First update the system and install the required software packages:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl build-essential
```

### Install Node.js 22 (ARM64)

OpenClaw requires Node.js 22 for ARM64 architecture:

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs

# Verify installation
node --version  # Should show v22.x.x
npm --version
```

### Add Swap Memory

For optimal performance on low RAM devices, add swap memory:

```bash
# Create 2GB swap file
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Make permanent
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Optimize for low RAM (reduce swappiness)
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

### Install OpenClaw

Use the official installer to install OpenClaw:

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

### Setup and Configuration

Run the onboarding wizard to configure OpenClaw:

```bash
openclaw onboard --install-daemon
```

Follow the setup wizard to:
- Configure your preferred AI model (such as GitHub Copilot)
- Set up communication channels (WhatsApp, Discord, etc.)
- Customize voice commands and responses

After setup is complete, you can start using OpenClaw with your MiniPupper for voice-controlled interactions and AI assistance.