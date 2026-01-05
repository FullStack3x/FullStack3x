So my very cool, very bald mentor gave me a project to build a server over the Christmas holiday. I've basically been pressing him to give me things to do since we connected at work - I worked in sales at the time, but I was in customer service before that and one of the responsibilities I had was working the front desk from time to time while my company filled the position. Long story short, I run into one of the nicest ladies I've met so far there, who happens to work on our SAP architecture (I believe). We have some conversation, I let her know I'm studying cybersecurity myself through whatever I could find on the internet, and she introduces me to the Cybersecurity Engineer there. I start learning some more, getting projects and connecting with a straight shooter who knows his stuff and was encouraging me to just jump off the deep end out of the gate. 

I ended up standing up a MISP server, but I didn't quite finish it because my main job in sales was too demanding, and I honestly would be exhausted after talking all day, dealing with fires left and right, trying to maintain my numbers and keep my head on straight as much as possible. Fast forward to now, I work as a pricing analyst and it's almost harder now, but I'm sticking with it and doing everything I can to get some hands on practice - You can learn all you want, but without hands on application you don't get an understanding of the why behind everything. And that brings us to this moment - You reading this repo to come along with me in my journey into building servers in Ubuntu.

I'm running Windows 11 on a Dell Latitude 5410 with 32GB of RAM, a 512GB SSD, and an Intel Core i7-10610U processor.

### Gathering the Materials

The first step is getting all of the software we need to run both the Member device and the server through - That would be a Virtual Machine (VM) to run another Operating System (OS) on, which is going to be Linux Ubuntu. I could run it on my laptop without the VM, but that would require me to delete Windows and solely run Linux - It's not necessary, this situation doesn't exactly call for it, and I just don't feel like doing all of that to be completely honest. That's another project for another day.

We'll be creating two VMs for this since we'll be simulating one device communicating with another.

**Downloading the Virtual Machine**

This is where I was running into the most frustration in the beginning, you'll see why soon. So Downloading the VirtualBox is easy enough, getting it to run was a little tricky, I just needed to do a little searching on some forums to find out how to get it going. The thing is, my computer was new - like not even a month old new when I started doing this, so I didn't have Python on my computer yet to be able to make it work. The VirtualBox needs the Python Core & win32api bindings because the VirtualBox has a Python API that allows external Python scripts to manage and automate VMs on Windows hosts. 

**Downloading Python**

I downloaded the newest version of Python [here](https://www.python.org/downloads/) and installed it using the command prompts that showed up in the terminal that popped up at the installation - just a bunch of pressing y > enter. The main thing is making sure you press y when it asks you about if you want to create a PATH for python. This is important so you're able to execute Python commands and run Python scripts from anywhere in the terminal, otherwise they won't work when it's time.

That's important because when it was time to install the bindings on my terminal, I had to use a specific command to get the win32api bindings downloaded for them to be able to be used by the VirtualBox:

py -m pip install --upgrade pip

**Sidenote**

**Ubuntu Server** and **Ubuntu Desktop** are 2 completely different downloads, so make sure to pay attention when you're doing the setup.

## Member Device Setup

So now we're at the point where I have to actually create the VM that Ubuntu is going to run on. This distribution is going to be for the member device as the VM setup will be different for each of our devices.

**Downloading Ubuntu**

I'm working on Ubuntu 24.04.3 LTS. There is a newer version available right now - Ubuntu 25.10 - the LTS is the most reliable version to have for servers and workstations as it offers stability, security, and 5+ years of updates. The newer versions only offer 9 months of support and require more frequent updates so they're good for trying out the newest features and for people who are comfortable with making updates that frequently, just depends on what your purpose is. Mine is to build a server and use it for something down the line, I just dont know exactly what it will be yet, I think LTS is good to start since it will be lower maintenance.

I downloaded it from [here](https://ubuntu.com/download/desktop)

**Creating a Virtual Machine**

The Virtual Machine in itself is a vehicle to run the second OS in without having to swap the base Windows 11 OS I'm using. Think about it as a holster - without one, you can only carry one weapon (Windows 11). When you equip yourself with the holster (Virtual Machine), you can now carry another weapon (Ubuntu) to switch to for a different situation. Two VMs, 3 total weapons to use.

We're at the point where I have to actually create the VM that Ubuntu is going to run on. The Virtual Machine in itself is a vehicle to run the second OS in without having to swap the base Windows 11 OS I'm using. Think about it as a holster - without one, you can only carry one weapon (Windows 11). When you equip yourself with the holster (Virtual Machine), you can now carry another weapon (Ubuntu) to switch to for a different situation. 

When I select new, It'll ask me to name it - It feels like my first Pokemon so I'm gonna call it Totodile. If you remember Pokemon Gold, it was the water type base you could pick and that was the first one I went all the way with, so I'll sprinkle a little nostalgia in there for me. 

**Selecting the Disk File**

Back to the important stuff: Selecting an ISO Image is key here - An ISO Image is a .iso file - a digital replica of a CD or DVD that serves as a software installer. A.K.A. the file that holds the operating system we want to run in the VM. in this case, the download we got from the Ubuntu site is going to be the .iso file we need to import.

**Password Security: VVVImportant**

After making sure we include that file and make sure the dropdowns below are correctly aligned with the version we need, the next page basically asks for the password you want to use - create one and move along. For best practices, I make sure to apply entrophy over complexity and add length to the passwords. When I was studying for the Intro to Cybersecurity Certification with ISC(2) a while back, one of the things I really gained from that was knowledge about how encryption works. Namely, when an attacker is using a password cracker to try and gain access to an account, the longer the password is the longer amount of time it takes to try and get into an account, making it considerably harder to get hacked unless you just give the password up. I would use 1Password or something like that with a compilation of random words and some symbols to make it more secure and then randomize the rest of the passords based on that same principle.

**Sidebar: Cryptography**

Small lesson on cryptography and how it connects because I think this is super cool: Almost every action in the modern digital world involves cryptography in some way. In short, it's the way information is protected behind the scenes, making it unreadable to the naked eye unless you have whatever the key is to unlock that information.

Passwords generally get encrypted with hashing - when you enter your password on a website, the actual password you enter isn't stored anywhere, it gets hashed. In short, it's mathematically tuned into an unrecognizable compilation of letters and numbers. the longer the password the more time it takes to hack.

**Back to Regularly Scheduled Programming: VM Setup - Specifying Virtual Hardware**

Allocating base memory, an amount of CPUs and disk size is important because it determines the power of your VM vs the amount of power you leave for the host device. It's good to leave a good amount of base memory in general for the host device so it's not overworking itself and you have space for normal usage. 2-4 GB is a safe place to start so I'm going with 4. 

As Far as CPUs go, similar to the Base Memory, this is how many cores you allocate to the VM vs how many you leave for the host device. For this simple server 3 is okay - one for the OS and two for the overhead. In general, you want to avoid assigning more CPUs than your computer actually has cores. You'll want to take **context switching** into account when doing this - when the CPU schedueler decides to switch from one running process to another. Like if you're running a video and a music player at the same time, your CPU isn't actually doing it at the same time, it's switching processes between those two more times than you can count in a second to keep those running. 
**
For Disk Size, you want to just make sure you have more disk space on your host device than you do on your VMs total. We'll be running maybe 2 VMs here, so for this first one Im gonna allocate 25GB. I have abbout 477 available and this computer is primarily only used for Cybersecurity, so that should be enough for now.

It'll ask you if you want to confirm after this, hit finish and **bow** - Totodile has been born. You can also always go back and change these settings in the future if you want/need.

Once you finish here, your VM should boot Ubuntu and open the installation window. It's a bunch of clicking next, naming the distribution, and letting it setup. For this project, we'll need Active Directory so make sure to select that option when it comes up.

**Install Problem: Unattended Installation**

I ran into an issue here where Ubuntu just wouldn't install fully, I kept getting error messages. I thought That something went wrong initially because I stopped the install at one point trying to go back and have it connect Active Directory, so I deleted the ISO image and redownloaded it, but I got the error again. Turns out the installer tried to format my virtual hard drive and write the partition table, but it couldn't complete the process. It sounds like this is normally caused by the unattended installation feature in VirtualBox, meaning I had to delete the VM and recreate it without having that selected.

**Back On Track - Finish installing Member Device**

So we got it uninstalled, I added another CPU just in case, but the manual install is going well so far. When setting up the VM, unselecting the Unattended Installation forces Ubuntu to basically have you choose your path instead of using an autoinstaller, which I think is missing and the reason I was running into issues before. This is more hands on and informative anyway, which is the reason I'm even doing this. 

I cheated and used Gemini Pro to help me think through how to get through that and had it breakdown the issue:

_"The log snippet you provided shows the installation failing (or hanging) exactly at the cmd-install/stage-partitioning step. This means the installer tried to format your virtual hard drive and write the partition table, but it couldn't complete the process.

In VirtualBox 7.x with Ubuntu 24.04, this is almost always caused by a feature called "Unattended Installation."_

So, we repeat the steps from earlier and go through with the install. You should have  an ubuntu bootstrap as well, when it asks you to update ubuntu and you should do so. This one is the ubuntu-desktop-bootstrap - basically it uses Linux's installer backend, Subiquity, to push the setup along, and Flutter - A Google open source UI SDK used to create the GUI for Linux. 

The Ubuntu bootstrap is what's going to make this the member device, since it needs the GUI. The Server VM is going to be completely run from the Terminal, but we'll get into that later.

The setup will be the same up to the internet connection part - If you're confused, just go with wired connection, Virtual box acts like a wired ethernet cable for the VM so it shows like that. After that, things work a little different, so far so good.

When we get to the Application Setup part, we'll want to choose Interactive Installation. Automatic gives us the same issue as last time - essentially Subuquity not being able to locate the path the necessary _autoinstall.yaml_ file is located within, and breaking the install.

From there it's easy & breezy for a while - Select Install Third Party software for Graphics and Wi-Fi as well as download and install support for additional media formats. This, in addition to the extended selection packages are going to help us mimic an actual user machine that has access to these things - Spreadsheets, word processors, email clients, etc.

Now we erase the disk and install Ubuntu - This just erases what was in the disk space we set aside in VirtualBox when we set the machine up. In this instance, we won't use advanced features - it'll come in handy later, but we'll keep it simple for now.

Name the computer, name the user, give it a password, all that good stuff and we're good here. We'll join the domain after the installation as this is a cybersecurity best practice. Overall it looks like it's just easier as there can be install issues where the installer gets stuck, the DNS isn't configured correctly, etc.

**SUCCESS**

The install went perfect this time and we have a restarted VM with Ubuntu on it. When it asks to delete the installation medium ignore that, it's a carry over from the days of CD-ROMs apparently and is saying to remove the "disc" from the "tray" so it doesn't boot from the installer again, but since we're doing this from the virtualbox, it should know not to do that, so we'll be fine.

We're now at the final stages of setup - The first thing it asks is if we want to enable Ubuntu pro. While it is ideal, especially for enterprise environments and DoD environments, we'll skip it to have the most plain system possible, showing us where all the vulnerabilities are and patching/managing them as we learn about them. IT can also be turned on later so no sweat.

**Setting up Apps**

Here's where we want to add the apps we'll be using and need access to, cybersecurity focused. The first 3 apps are going to be Nmap (Network Mapper), Wireshark (Packet Analyzer) and VSCode (Remote-SSH) - Nmap to discover the server and see what ports are open, Wireshark to watch the traffic between the member and the server (We can see if passwords are sent in plaintext here), and VS code to be able to write code on the Member Desktop VM that will be deployed on the Server VM.

Nmap worked beautifully - Wireshark shows as a Debian package, and VSCode just doesn't show up at all, so we'll have to work around this. For context and simplicity, Debian is a Linux distribution that Ubuntu is essentially built on top of to provide ease of use. So technically it should be able to use it, we'll just have to install it from the Terminal.

Apparently it's a common issue so it shouldn't be hard to get past. The new app center prioritizes "Snap" packages - universal application bundles that include all dependencies, making them easy to install. They sometimes struggle with .deb files, or Ubuntu lacks the permission prompt to install these, which is why we have to install from the Terminal.

**Into the Terminal**

Ctrl+Alt+T brings up the Terminal in Linux - From here, we're using Bash Script. I'll give the play by play while we're doing it:

**Update available software**

sudo apt update

sudo: "SuperUser Do." This tells Linux to run the command with Administrator (Root) privileges. Without this, you cannot install software or change system settings.

apt: "Advanced Package Tool." This is the program Ubuntu uses to manage software (install, delete, update - Choose your adventure).

update: The specific action you want to perform - **This does not update your software.** It updates the list of available software. It tells your computer to check the central Ubuntu servers to see what the latest versions of every app are, so when you do install something, you get the newest one.


**Wireshark Install/Configuration**

sudo apt install wireshark -y

So now we know the beginning is telling linux to use Superuser privileges from the advanced package tool: here's the rest for the second command:

install: The specific action you want apt to perform.

wireshark: The exact name of the package you want to download.

-y: "Yes." Usually, when you install software, Linux pauses and asks, "This will use 50MB of disk space. Do you want to continue? [Y/n]". Adding -y automatically answers "Yes" to that question so the script doesn't stop and wait for you.


We're sending these one after another so they should be separate commands. The update comands updates the list with the other available "applications," and the install command gets us Wireshark.

In this instance, non superusers **should** be able to capture packets, otherwise you would have to run wireshark as root every single time - This is an extreme security risk. root grants unrestricted system privleges. Should an application have a vulnerability or be malicious, whoever gets root access has the keys to the kingdom, and if they're smart they'll be very hard to get out if they're able to be purged at all. In conclusion, non superusers should be able to run wireshark. It allows you to give access to accounts that can be further limited by Identity and Access Management. This means the sudo command would have to be used as it's currently still restricted as an administrator and access is only granted with the password. Remember, my password is more complicated due to me enforcing a strict **password policy** on myself of having several unrelated words, at least 1 number and at least 1 special character. Length matters more than complexity. 

Now that wireshark is added and non superusers can use it, we have to **add this user to the group** using the following code:

sudo usermod -aG wireshark $USER

usermod: "User Modify." The command to change settings for a user account.

-aG: These are two flags combined:

-a (Append): This is critical. It means "Add this user to a group, but keep all their existing groups." (If you forgot the -a and just used -G, it would remove you from every other group, including the 'sudo' group, effectively locking you out of your own computer).

-G (Group): Tells the command that the next word you type will be a Group Name.

wireshark: The name of the specific security group that allows access to the network card.

$USER: This is a "Variable." Linux automatically replaces $USER with your actual username (e.g., Totodile) so you don't have to type it manually.

Now that wireshark group has been created, but normally we would have to log out and log back in to actually see the changes take place. To push the change without a log out:

newgrp wireshark

newgrp: "New Group."

What it does: Normally, when you change your group memberships like before Linux doesn't notice until you log out and log back in. This command forces the current terminal window to refresh its identity and recognize that you are now part of the wireshark group immediately.

Now that the group has been created, we'll want to check and make sure we were added properly. There are 3 ways to do this.

**Current Session Check**

This command is a simple one:

groups

This will show you a list of every group the current user belongs to. as long as wireshark is in that list, we've been added successfully added. If not, something went wrong.

**Official Record Check**

Now if we want to verify that the usermod command worked on the system database. This will work even if the terminal hadn't updated yet:

grep wireshark /etc/group

grep
This stands for "Global Regular Expression Print." It is essentially the "Ctrl + F" (Find) function for the command line.

You give it a text pattern, and it scans a file line-by-line. If a line contains that pattern, it prints the entire line to your screen. If the pattern isn't found, it stays silent.

wireshark
In this instance, since grep is like the Ctrl+F, wireshark would be the Search Pattern (or keyword).

This is the specific text string grep is looking for. In this case, we are looking for the group named "wireshark."

/etc/: This directory is the "nervous system" of Linux. It contains all the system-wide configuration files (startup scripts, network configs, password files).

group: This specific file is a plain text database. It defines every single "Group" on the operating system, who belongs to it, and what its ID number is.

It should look something like:

wireshark:x:1001:yourusername. 

Wireshark: The Group Name
x: the password - the x means the pasword is encrypted stored in a different, more secure file (/etc/gshadow)
128: The group ID - Linux used numbers, not words to handle permissions
yourusername: this would be whatever members are included in that list, it's not always just one person.

As long as our name shows up at the end of that line, we're in.

**Real World Test**

This is where we open the application in the GUI/Desktop and test it from the app menu. You can also do it from the terminal.

Clicking show apps and searching for wireshark opens it there. To open it from the Terminal:

You should e able to see a list of network adapters like enp0s3, eth0, or loopback with hearbeat rhythms to the right of them. They may take a second to show - if you see "no interfaces found" or the list is empty, it was a failure and something went wrong adding the user to the group. 

**VSCode Install**

Wireshark and Nmap are downloaded, now it's time to make sure we have VSCode. To install, we'll run the following bash command:

sudo snap install code --classic

sudo: "Run as Administrator."

snap: This calls the Snap Package Manager (instead of apt).

install code: "Find the package named 'code' (which is VS Code) and install it."

--classic: This is the most important part.

By default, Snap apps are "sandboxed"—they live in a bubble and can't touch your other files (for security).

However, VS Code is a code editor; it needs to read your files, open scripts, and run compilers elsewhere on your drive.

The --classic flag tells Ubuntu: "Burst the security bubble for this one app. Let it access the whole hard drive just like a normal 'classic' program."

The download should start running from the terminal there, a few minutes later you should have VSCode in that VM.

We've added apps by the GUI and the Terminal now, and our member device is set. Time to move on to the Server VM.


## Server Device Setup

Time to get the server stood up - This is going to be a little different since our server hardware/software needs aren't the same.

**Downloading Ubuntu**

If we want a slow, bloated server, we continue with the .iso we used in the member server. Nobody ever wants that, so we're going to grab a different version of Ubuntu for it. 

I downloaded Ubuntu Server [here](https://ubuntu.com/download/server)

This version comes without the GUI installed - Saving RAM - and is pre-installed with server Grade Kernels.

**Creating A Virtual Machine**
