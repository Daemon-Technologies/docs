---
sort: 1
---

# Building Mining-Bot Before Use

The goal of this article: Demonstrate how to run Mining Bot Beta version.

## Requirements:

Make sure you have installed:

- `Nodej >= 14`
- `yarn` package management tool
- `wget`
- `unzip`

### Environmental installation and configuration

#### Install Node.js and yarn

Refer：[Node.js and yarn installation reference guide](https://github.com/Daemon-Technologies/docs/blob/master/_draft/Mining-Bot-Alpha-Version/Build-Before-Using/Mining-Bot-Alpha-Tutorial-EN.md)

#### wget 

**On MacOS**:

```bash
brew install wget
```

**On Ubuntu/Debian**:

```bash
apt-get install wget
```

**On RHEL，CentOS，Fedora**:

```bash
yum install wget
```

Make sure to install, check the version:

```bash
wget --verison
```

#### unzip

**On MacOS**:

```bash
 brew install unzip
```

**On Ubuntu/Debian**:

```bash
sudo apt install unzip
```

**On RHEL，CentOS，Fedora**:

```bash
sudo yum install unzip
```

## Mining-Bot Beta version installation and running

### Step 1: Download the installation package

Two ways to download:

- Command line download (this article demonstrates)
- Download via browser

Open a command line window, and Win10 users open a new WSL window.

Create an empty directory: 

```bash
mkdir stacks-mining
```

The name of the directory can be taken whatever you want. For the purpose of demonstration here, it is named `stacks-mining`.

Enter this empty directory:

```bash
cd stacks-mining
```



Access link: [Mining-Bot Releases](https://github.com/Daemon-Technologies/Mining-Bot/releases/tag/2.0.0)

Enter the Mining-Bot Beta 2.0.0 Release interface:

![Mining-Bot-Release-Page](assets/Mining-Bot-Release-Page.png)



Choose your own system version to copy and download the link. In this tutorial, the local environment is MacOS, select [Mining-Bot_V2.0.0_mac.zip](https://github.com/Daemon-Technologies/Mining-Bot/releases/download/2.0.0/Mining-Bot_V2.0.0_mac.zip), right click to copy the link:



![Right-Click-Copy-Link](assets/Right-Click-Copy-Link.png)



Use the `wget` command to download the `zip` file of the corresponding system:

```bash
wget https://github.com/Daemon-Technologies/Mining-Bot/releases/download/2.0.0/Mining-Bot_V2.0.0_mac.zip
```

![wget-mining-bot-zip](assets/wget-mining-bot-zip.png)

Or directly click the corresponding version, download it directly from the browser, and choose to store it in the current working directory: `stacks-mining`。

### Step 2: Unzip the zip file

Two ways tp extract files:

1. Command line extract (this article demonstrates)
2. In the graphical interface, double-click to extract, or right-click to extract

Here is a demonstration of the first way via `unzip -d` command：

```bash
unzip Mining-Bot_V2.0.0_mac.zip -d Mining-Bot-Beta/
```

Among them, `-d` represents the name of the output directory. If it does not exist, create a new directory and output the compressed file content to the file directory. Here we output to the `Mining-Bot-Beta` folder under the current directory (this directory will be created).

![ls-unzip-d](assets/ls-unzip-d.png)



### Step 3: Start the program

Enter the unzipped folder: `Mining-Bot-Beta`。

Excute the command: 

```bash
cd Mining-Bot-Beta
```

Check the file:`ls`

![ls-files-in-mining-bot-beta](assets/ls-files-in-mining-bot-beta.png)

Run`yarn install` command or `npm install`, or directly use `yarn`, install dependent packages and ensure that the Nodejs version is greater than 14.

After the installation is complete, the `node_modules` folder appears.

![yarn-node-modules](assets/yarn-node-modules.png)



#### 1. Start Mining-Bot

Execute the command: `yarn start node1234`

And，`node1234` is the authentication password, configure after `yarn start`.

The information displayed when successful startup is as follows:

![yarn-start-node1234](assets/yarn-start-node1234.png)



**Note**: If you are using a server, you need to access Mining-Bot according to the `ip` address of your server.

The ip address is not displayed when my machine is tested, it will be displayed on Win10, and the `ip` address of the machine can be obtained through the `ifconfig` command.

Note that after starting the program, there are two processes running separately, port `5000` is the background service process, and port `8000` is the client process of the mining bot.

#### 2. Access Mining-Bot

In order to access the mining bot, we only need to copy the Mining-Bot Client address.

Type/Paste in the browser: http://localhost:8000/`，or`http://your-ip:8000 ,e.g. http://192.168.31.171:8000/

![Mining-Bot-Index](assets/Mining-Bot-Index.png)

When you log in for the first time, you will be prompted to enter the **Lock Password.** This password is mainly used for login authentication and private key encryption protection. The lock password here has nothing to do with the previous `yarn start node1234` authentication password, and there is no need to keep the same.

At this point, the program is successfully installed. The next steps will be operated on the browser page. Please refer to the [User Guide](../Use-Mining-Bot-For-Mining/User-Guide-of-Mining-Bot-Beta-EN.md).

