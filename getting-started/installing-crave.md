---
order: 1
---

# Getting Started 
## Installing Crave
### *Web*

Open [sessions](https://foss.crave.io/app/#/session?team=14) tab and
click on "Create Session/Connect" button.

Now, [skip to the next
section](./setting-project.md) without worrying about entering Devspace CLI.

For menu, swipe from the middle of the left side of your phone. If you're on a computer/have access to a keyboard, press `Ctrl+Alt+Shift`. This menu allows you to customize the terminal, and switch input method, like phone keyboard instead of emulated one. 

Crave RAS is built upon [Apache Guacamole](https://guacamole.apache.org/)

There's also a VSCode Web Client, instructions to set this up can be found [here](../tricks.md#vs-code-web)

### *Windows*

On windows, download the zip, extract it, enter it.

Right click somewhere blank on the folder and click on Open In Terminal.
Alternatively,

```
cd /d Drive:\path\to\crave\folder\ 
```

(replacing `Drive:\path\to\crave\folder\` with the full or relative path to your
crave folder)


Tip: If you use powershell and crave is not recognised but crave.exe exists in your current folder, you can use `./crave.exe` instead of `crave` to launch Devspace CLI. After that, go back to using `crave` when you enter Devspace CLI since it's a ubuntu environment with crave set up correctly.

### *Linux*

- If you're on linux, run this command to download crave binary

```
curl -s https://raw.githubusercontent.com/accupara/crave/master/get_crave.sh | bash -s -- 
```

- Run these commands to install it systemwide

```
mkdir -p ${HOME}/bin/
```

```
mv ${PWD}/crave ${HOME}/bin/ 
```

```
sudo ln -sf /home/${USER}/bin/crave /usr/bin/crave; sudo chmod +x /usr/bin/crave 
```

### *Mac*

- If you're on macOS, run this command to install crave

```
brew install --cask crave
```
You will need to [install homebrew](https://brew.sh/) beforehand.

### Android
#### Installing on Termux

Guide to use termux's proot-distro for crave -n devspace:
Link to proot-distro: https://github.com/termux/proot-distro

#### Step 0: Install termux
Get it from [PlayStore](https://play.google.com/store/apps/details?id=com.termux&hl=en_IN) or [F-Droid](https://f-droid.org/en/packages/com.termux/)

> [!NOTE]  
> This guide is tested on F-Droid build
#### Step 1: Install proot-distro
Direct Command:
```bash
pkg install proot-distro
```

Alternative:
```bash
pkg install git
git clone https://github.com/termux/proot-distro
cd proot-distro
```

#### Step 2: Set Up a Container using Proot
Install Ubuntu:
```bash
pd install ubuntu
```

Enter the Ubuntu Container:
```bash
pd sh ubuntu
```

Prepare for crave by installing dependencies:
```bash
apt update; apt install ssh rsync wget -y
```

Download Crave:
```bash
curl -s https://raw.githubusercontent.com/accupara/crave/refs/heads/master/get_crave.sh | bash -s --
```

Grant Permissions and Move it:
```bash
chmod +x crave; mv crave /usr/local/bin/
```

#### Step 2.5: Set up Credentials
Now open up your browser and navigate back to foss.crave.io dashboard. On the same Left Hand Side menu, and go to API keys.
- Create a new one, name it whatever you like to easily identify it and
click on Create API Key Button.
- Copy it
- Create a new file in your current shell called crave.conf
    - Paste the API Key here
#### Step 3: Connect
Once you take your valid crave.conf, just connect using `crave -n devspace` and return to the [previous guide](./setting-project.md)

> [!NOTE]  
> -n here tells crave to not look for updates. As of crave 7064, Updating step causes segmentation fault error, which can be worked around this way.

#### Step 3.5: Quick Reconnection
To Quickly reconnect, just run:
```bash
pd sh ubuntu -- crave -n devspace
```

#### Notes: 
if you facing issues:
- try to exit from termux
- then re-login with: 
    `pd sh ubuntu`
- then try again:
    `crave -n devspace`

Tested on `ubuntu`, though your results may vary.

You can try other distro: 
`pd list`

Thanks to Ica for discovering this method and writing the guide that this was based on