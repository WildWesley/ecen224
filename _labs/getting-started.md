---
title: Getting Started
number: 1
layout: lab
---

## GitHub Classroom
Use the GitHub Classroom link posted in the Slack channel for the lab to accept the assignment.

## Objectives

- Become familiar with the tools used for this lab
- Understand how to setup a single board computer
- Be able to gain access remotely to the Pi Z2W

## Overview

<figure class="image mx-auto" style="max-width: 750px">
  <img src="{% link assets/getting-started/pi-z2w.png %}" alt="Units of the course.">
  <figcaption style="text-align: center;">The Raspberry Pi Zero 2 W with the SD card port outlined in magenta, the mini HDMI port outlined in light blue, and the power micro USB port outlined in yellow.</figcaption>
</figure>

In this class we will be using the [Raspberry Pi Zero 2 W](https://www.raspberrypi.com/products/raspberry-pi-zero-2-w/) (or Pi Z2W for short) as the platform to explore the fundamentals of computing taught throughout the semester. In this lab you will be setting up the [single board computer](https://en.wikipedia.org/wiki/Single-board_computer) so that it is capable of handling all the labs in this course. There are 3 parts to this lab:

- Flash and configure the Pi Z2W
- Gain remote access to the Pi Z2W 
- Development environment setup

### Setting up the SD Card 
In order to make use of the Pi Z2W, we will need to put an operating system onto its SD card. An [**operating system**](https://en.wikipedia.org/wiki/Operating_system) (OS), simply put, is a collection of programs which allows a user to interact with a device's hardware. For users of PCs, this collection of programs is called [Microsoft Windows](https://www.microsoft.com/en-us/windows?r=1), for Mac users [macOS](https://www.apple.com/za/macos/what-is/), and for Linux users there exists a wide variety of [distros](https://itsfoss.com/what-is-linux-distribution/#:~:text=Your%20distributions%20also%20takes%20the,as%20Linux%2Dbased%20operating%20systems.) to choose from. 

In this lab we will become familiar with a distinct version of Linux called [Raspberry Pi OS](https://www.raspberrypi.com/software/) (formerly known as Raspbian) which was made specifically for devices like the Pi Z2W. 

1. In order to load Raspberry Pi OS to the SD card we will use the [Raspberry Pi Imager](https://www.raspberrypi.com/news/raspberry-pi-imager-imaging-utility/) tool. On your lab machine go to the **Activities** menu (in the upper lefthand corner) and then type in `imager`. Click on the icon that looks like a raspberry. You should see the following window open:
    <figure class="image mx-auto" style="max-width: 750px">
      <img src="{% link assets/getting-started/imager-start.png %}" alt="imager-start">
    </figure>
2. Select the **CHOOSE OS** option. Then click on **Raspberry Pi OS (other) > Raspberry Pi OS Lite (64-bit)**. This is the OS that will be written to the SD card. Note that the **Lite** option means that there will be no point-and-click navigation with the OS like you would be used to in Windows and macOS. Instead we will use the **command line** to get around.

3. Next we will configure some of the OS settings by clicking on the ![gear]({% link assets/getting-started/config-gear.png %}){:width="6%"} icon. This brings you to the **Advanced Options** menu:

    <figure class="image mx-auto" style="max-width: 750px">
      <img src="{% link assets/getting-started/advanced-options.png %}" alt="advanced-options">
    </figure>

4. Change the following values in the menu according to the table below:

    | Setting | Value | Description |
    | ------------------------- | ---------- | ---------- |
    | Set hostname: | `doorbell-<your_netid>` | This will be the name of your Pi Z2W. This will make it easier to find on the network. |
    | Enable SSH:   | Check **Enable SSH** and select **Use password authentication** | This allows you to login into your Pi Z2W from anywhere on the network with the `ssh` tool by using a username and password. |
    | Set username and password: | Any username and password you desire (make sure it is secure). | We change these values from the default so that you can protect your projects. You are responsible for remembering this username and password! **Any loss of these credentials may require you to re-setup your Pi Z2W.** |
    | Set locate settings: | Timezone: **America/Denver**<br/>Keyboard Layout: **us** | This makes sure that the region the Pi Z2W is in the MDT timezone with the US keyboard layout. | 
    | Configure Wireless LAN* | SSID: **name of WiFi network at home**<br/>Password:**password of network** | In case you want to work with the Pi Z2W outside of the Digital Lab. |

    <p style="text-align: right; font-size: 10pt;">*optional configurations</p>


5. Now that we have correctly configured the OS settings, we will write the OS to the SD card that came with your Pi Z2W kit. Make sure that your SD card is plugged into the USB adapter and that the adapter is plugged into the lab computer. 

6. Select **CHOOSE STORAGE** on the imager. A window that allows you to select the SD card will pop up:

    <figure class="image mx-auto" style="max-width: 750px">
      <img src="{% link assets/getting-started/select-device.png %}" alt="select-device">
    </figure>

7. Select the SD card and click **WRITE** to start writing the OS to the SD card. A pop-up window will ask you if you want to erase all contents on the SD card before continuing, select **YES** and continue with the write process. The writing process will take a while. 

8. Once the writing process finishes, remove the SD card from your laptop and insert it into the SD card slot of the Pi Z2W. 

9. Plug in the PoE adapter (the white brick) into the **first** micro USB port (the only one not circled in the figure at the beginning of this lab) to power up and supply internet to Pi Z2W. It is not necessary to have power and PoE plugged in at the same time. 

    The boot process will take a while, so wait at least two minutes (or until the green light starts flashing) to move to the next section.

<!-- This depends on whether or not the CSRs give us HDMI cables in future semesters -->

<!-- Connect the mini HDMI cable adapter (connected to the HDMI out of your lab station's monitor), the USB-Ethernet adapter, and power cable to the corresponding ports (in that order or else the video for the device will not load correctly). If all worked correctly, you should see a splash screen on your monitor followed by a lot of scrolling text. You will know that your installation is successful if you see the following prompt on your screen:

```
doorbell-<your_netid> login: _
``` -->

### Connect to Pi Z2W Remotely
Now that your Pi Z2W has Raspberry Pi OS Lite installed and is connected to the lab network, we are able to connect to the it remotely using `ssh`. A remote connection means that you are able to log into a computer (like the Pi Z2W) _from_ a different computer (like the lab machines). 

<!-- Disconnect the HDMI cable and plug it back into the lab machine.  -->

1. Make sure you are able to find your Pi Z2W by opening up the **terminal** on your lab machine. This can be done either through finding it in the **Activities** menu or simply pressing `Ctrl+Alt+T`. Once the terminal has opened, search for it by entering the command:

    ```bash
    ping doorbell-<your_netid>.local
    ```

    If you receive a message like the following:
    ```
    ping: doorbell-kitras.local: Name or service not known
    ```
    then the Pi Z2W is not connected to the lab network. Check the ethernet cable and make sure that everything is plugged in correctly then try again. 

    If you receive messages like:
    ```
    PING doorbell-kitras.local (192.168.86.48) 56(84) bytes of data.
    64 bytes from doorbell-kitras.lan (192.168.86.48): icmp_seq=1 ttl=64 time=95.7 ms
    64 bytes from doorbell-kitras.lan (192.168.86.48): icmp_seq=2 ttl=64 time=5.47 ms
    64 bytes from doorbell-kitras.lan (192.168.86.48): icmp_seq=3 ttl=64 time=25.0 ms
    64 bytes from doorbell-kitras.lan (192.168.86.48): icmp_seq=4 ttl=64 time=21.6 ms
    64 bytes from doorbell-kitras.lan (192.168.86.48): icmp_seq=5 ttl=64 time=6.53 ms
    ```
    press `Ctrl+C` to stop the command. This means your lab machine can see the Pi Z2W and you are able to login with `ssh`.

2. Login to the Pi Z2W via `ssh` by typing in:
    ```bash
    ssh <your_username>@doorbell-<your_netid>.local
    ```
3. A prompt will appear asking if you want to add the Pi Z2W to a list of trusted remote computers. Type in `yes` to add the Pi Z2W to the trusted list. 

4. After entering in your login credentials, you should receive a prompt like the following to show that you are inside of the Pi Z2W:

    <figure class="image mx-auto" style="max-width: 750px">
      <img src="{% link assets/getting-started/ssh.png %}" alt="ssh">
    </figure>

    You can tell you are inside the Pi Z2W by looking at the string before the cursor. It should be `username@computer_name` or specifically `<your_username>@doorbell-<your_netid>` on the Pi Z2W.

5. Now that you are logged into your Pi Z2W, we will download a script that will install all the dependencies we will need for future labs.

    First, run this command:
      ```bash
      wget https://byu-cpe.github.io/ecen224/assets/scripts/install.sh
      ```
   This will download the script to your Pi. Next, run:
      ```bash
      chmod 777 install.sh
      ```
    This command will change permissions on the script, allowing you to execute the .sh file. Lastly, run the script with:
      ```bash
      ./install.sh
      ```
    You will see print statements in the terminal logging the installation process.
    Upon completion of the install script, your terminal should have colorful text and look slightly different (This comes from a framework called "Oh my Zsh").
    To complete your Pi Setup Process, reboot your Pi with this command:
      ```bash
      sudo reboot
      ```
     **Note: If any prompts come up asking if you would like to continue, type 'y' and hit enter. The script should continue as normal.**
  

### Connect with VSCode
The last we'll do in this lab to setup the Pi Z2W is to ensure that we can connect to it using the **Remote - SSH** extension in VSCode. 

1. Open VSCode through the **Activities** menu on your lab machine. Look at the bottom status bar of the window. There should be an icon at the bottom left side of the screen:

    <figure class="image mx-auto" style="max-width: 750px">
      <img src="{% link assets/getting-started/vscode-icon.png %}" alt="vscode-icon">
    </figure>

2. If this icon is not there, that means the **Remote - SSH** extension is not installed. To install it, click on the extensions ![extensions]({% link assets/getting-started/vscode-extensions.png %}){:width="4%"} icon on the leftmost toolbar. This will open up the **Extensions Manager** where you can search for any type of tool you wish to add to your VSCode editor. 

3. Type in `Remote - SSH` into the searchbar and click the **Install** button on the entry that says **Microsoft**  underneath. 

4. Once the extension has successfully installed, you can now click on the green icon in the bottom left corner as seen in the figure above.

5. Click on this icon then select **Connect to Host > Add New SSH Host**. In this input box we will put the `ssh` command we used before to connect to the Pi Z2W through the terminal: `ssh <your_username>@doorbell-<your_netid>.local`. 

    <figure class="image mx-auto" style="max-width: 750px">
      <img src="{% link assets/getting-started/vs-ssh.png %}" alt="vs-ssh">
    </figure>

6. Then save this entry to your local `ssh` configuration file on our lab machine:

    <figure class="image mx-auto" style="max-width: 750px">
      <img src="{% link assets/getting-started/ssh-conf.png %}" alt="ssh-conf">
    </figure>

7. Click on the Remote SSH icon in the bottom left corner again and this time select the new `ssh` entry you just made by clicking **Connect to Host > doorbell-\<your\_netid\>.local**. 

8. A new window will pop up and prompt you to enter in your Pi Z2W password. Enter in your password. You can now browse through files and code on your Pi Z2W from VSCode on your lab machine. 

9. In the left toolbar on the window, click on files ![extensions]({% link assets/getting-started/files.png %}){:width="4%"} icon. 

10. Click on the **Open Folder** button and then click **OK** on the pop-up. This should put you into your home folder on Raspberry Pi OS.
    <figure class="image mx-auto" style="max-width: 750px">
      <img src="{% link assets/getting-started/open-folder.png %}" alt="open-folder">
    </figure>

    Now you can use the left pane of VS Code to view and edit files inside the home directory on the Pi Z2W.

### Create SSH key for GitHub
All of the labs require you to write code and upload it to a version control service called GitHub. A GitHub account is required for this class. If you do not have an account you can create one [here](http://github.com/join) otherwise you are allowed to use your personal account for this class. 

In order to allow our Pi Z2W to speak with GitHub, we will need to create an **SSH key** that will allow GitHub to know that the changes to the code online came from a trusted source (i.e. your Pi Z2W) and not from some impostor's device (i.e. your arch-nemesis's Pi Z2W).

1. To generate an SSH key, use the terminal that is connected to your Pi Z2W and type in:

    ```bash
    ssh-keygen -t ed25519 -C "<your_email_address"
    ```

    The tool will ask you several questions. For our purposes, the default values will suffice (i.e. just hit `Enter`) unless you desire to protect your key with a password. This will just require you to enter in a password any time you want to use the SSH key.

2. Once this is done you can find the contents of your new SSH keys by typing in
    ```bash
    cat ~/.ssh/id_ed25519.pub
    ```
    **NOTE:** Make sure that you `cat` the values of `id_ed25519.pub` and **NOT** `id_ed25519`. The contents in the `.pub` are meant to be shared with the `pub`lic and the contents of the other file are not meant to be shared with anyone else.

3. Copy the output of this file by selecting it and pressing `Ctrl-Shift-C`. Then navigate in a web browser to your GitHub [keys console](https://github.com/settings/keys) (you must be signed into GitHub for this step to work).

4. At the top of the page will be a big green button that says **New SSH key**. Click on this and then you should be taken to page like the one below:

    <figure class="image mx-auto" style="max-width: 750px">
      <img src="{% link assets/getting-started/github-ssh-key.png %}" alt="github-ssh-key">
    </figure>

5. Paste the contents that we copied into the **Key** box and feel free to add whatever value you desire into the **Title**. Make sure the dropdown menu for **Key type** is set to `Authentication Key`. 

6. Finally, click **Add SSH key** and now your Pi Z2W should be able to talk to your GitHub account.

### Setup GitHub Repository
Next, we need to ensure that [`git`](https://git-scm.com/) is installed on our Pi Z2W. This will be the terminal program that we use to communicate with GitHub to version control and submit our assignments. 

1. If the terminal window on VSCode is not already open, press `` Ctrl+` `` and then enter in the command:

    ```bash
    sudo apt install git
    ```

    Once `git` has installed, we use this to download the first the repository (or code-base) for this lab. Since this lab requires no code to be written, the repository will be pretty uneventful. But remember these steps as future labs will use these steps to be set up.

2. Click on the **GitHub Classroom Link** located at the top of the `#lab-1` channel in Slack. You will be taken to a page that asks you to accept the assignment. Accept it and then you will be transferred to a page with a link of your repository for this assignment. Click on that link and you should be brought to the default view of the repository.

3. Click on the green button that says **Code**. Make sure you are on the **SSH** tab as shown below and copy the text in the box beneath.

    <figure class="image mx-auto" style="max-width: 750px">
      <img src="{% link assets/getting-started/ssh-repo.png %}" alt="ssh-repo">
    </figure>

4. Back on the Pi Z2W's terminal type in
    ```bash
    git clone <GitHub Repo SSH URL>
    ```

    This downloads a copy of the lab files from to your Pi Z2W. Since this lab is focused on setting up the Pi Z2W, there are no special files that you will need to interact with other than the `README.md` for your lab submission. 

    <figure class="image mx-auto" style="max-width: 750px">
      <img src="{% link assets/getting-started/dl-repo.png %}" alt="dl-repo">
    </figure>

In future labs, any starter code or special resources for completing that lab will be included alongside that lab's `README.md`.


## Lab Submission
- Answer the questions in the `README.md`. 

- To successfully submit your lab, you will need to follow the instructions in the [Lab Setup]({{ site.baseurl }}/lab-setup) page, especially the **Commiting and Pushing Files** and **Tagging Submissions** section.


## Explore More!
- [Learn more about the terminal](https://kitras.io/linux/shells/)
- [Customize the terminal](https://www.geeksforgeeks.org/how-to-customize-linux-terminal-using-powerline10k/)
- [Ditch passwords for keys](https://dev.to/risafj/ssh-key-authentication-for-absolute-beginners-in-plain-english-2m3f)
