So my very cool, very bald mentor gave me a project to build a server over the Christmas holiday. I've basically been pressing him to give me things to do since we connected at work - I worked in sales at the time, but I was in customer service before that and one of the responsibilities I had was working the front desk from time to time while my company filled the position. Long story short, I run into one of the nicest ladies I've met so far there, who happens to work on our SAP architecture (I believe). We have some conversation, I let her know I'm studying cybersecurity myself through whatever I could find on the internet, and she introduces me to the Cybersecurity Engineer there. I start learning some more, getting projects and connecting with a straight shooter who knows his stuff and was encouraging me to just jump off the deep end out of the gate. 

I ended up standing up a MISP server, but I didn't quite finish it because my main job in sales was too demanding, and I honestly would be exhausted after talking all day, dealing with fires left and right, trying to maintain my numbers and keep my head on straight as much as possible. Fast forward to now, I work as a pricing analyst and it's almost harder now, but I'm sticking with it and doing everything I can to get some hands on practice - You can learn all you want, but without hands on application you don't get an understanding of the why behind everything. And that brings us to this moment - You reading this repo to come along with me in my journey into building servers in Ubuntu.

I'm running Windows 11 on a Dell Latitude 5410 with 32GB of RAM, a 512GB SSD, and an Intel Core i7-10610U processor.

### Setting up

The first step is getting all of the software we need to run the server - That would be a Virtual Machine (VM) to run another Operating System (OS) on, and Linux Ubuntu OS to run the server on. I could run it on my laptop without the VM, but that would require me to delete Windows and solely run Linux - IT's not necessary, this situation doesn't exactly call for it, and I just don't feel like doing all of that to be completely honest. That's another project for another day.

**Downloading Ubuntu**

I'm working on Ubuntu 24.04.3 LTS. There is a newer version available right now - Ubuntu 25.10 - the LTS is the most reliable version to have for servers and workstations as it offers stability, security, and 5+ years of updates. The newer versions only offer 9 months of support and require more frequent updates so they're good for trying out the newest features and for people who are comfortable with making updates that frequently, just depends on what your purpose is. Mine is to build a server and use it for something down the line, I just dont know exactly what it will be yet, I think LTS is good to start since it will be lower maintenance.

I downloaded it from [here](https://ubuntu.com/download/desktop)

**Downloading the Virtual Machine**

This is where I was running into the most frustration in the beginning, you'll see why soon. So Downloading the VirtualBox is easy enough, getting it to run was a little tricky, I just needed to do a little searching on some forums to find out how to get it going. The thing is, my computer was new - like not even a month old new when I started doing this, so I didn't have Python on my computer yet to be able to make it work. The VirtualBox needs the Python Core & win32api bindings because the VirtualBox has a Python API that allows external Python scripts to manage and automate VMs on Windows hosts. 

I downloaded the newest version of Python [here](https://www.python.org/downloads/) and installed it using the command prompts that showed up in the terminal that popped up at the installation - just a bunch of pressing y > enter. The main thing is making sure you press y when it asks you about if you want to create a PATH for python. This is important so you're able to execute Python commands and run Python scripts from anywhere in the terminal, otherwise they won't work when it's time.

That's important because when it was time to install the bindings on my terminal, I had to use a specific command to get the win32api bindings downloaded for them to be able to be used by the VirtualBox:

py -m pip install --upgrade pip

**Creating a Virtual Machine**

So now we're at the point where I have to actually create the VM that Ubuntu is going to run on. The Virtual Machine in itself is a vehicle to run the second OS in without having to swap the base Windows 11 OS I'm using. Think about it as a holster - without one, you can only carry one weapon (Windows 11). When you equip yourself with the holster (Virtual Machine), you can now carry another weapon (Ubuntu) to switch to for a different situation. 

When I select new, It'll ask me to name it - It feels like my first Pokemon so I'm gonna call it Totodile. If you remember Pokemon Gold, it was the water type base you could pick and that was the first one I went all the way with, so I'll sprinkle a little nostalgia in there for me. 

**Selecting the Disk File**

Back to the important stuff: Selecting an ISO Image is key here - An ISO Image is a .iso file - a digital replica of a CD or DVD that serves as a software installer. A.K.A. the file that holds the operating system we want to run in the VM. in this case, the download we got from the Ubuntu site is going to be the .iso file we need to import.

**Password Security: VVVImportant**

After making sure we include that file and make sure the dropdowns below are correctly aligned with the version we need, the next page basically asks for the password you want to use - create one and move along. For best practices, I make sure to apply entrophy over complexity and add length to the passwords. When I was studying for the Intro to Cybersecurity Certification with ISC(2) a while back, one of the things I really gained from that was knowledge about how encryption works. Namely, when an attacker is using a password cracker to try and gain access to an account, the longer the password is the longer amount of time it takes to try and get into an account, making it considerably harder to get hacked unless you just give the password up. I would use 1Password or something like that with a compilation of random words and some symbols to make it more secure and then randomize the rest of the passords based on that same principle.

**Sidebar: Cryptography**

Small lesson on cryptography and how it connects because I think this is super cool: Almost every action in the modern digital world involves cryptography in some way. In short, it's the way information is protected behind the scenes, making it unreadable to the naked eye unless you have whatever the key is to unlock that information.

Passwords generally get encrypted with hashing - when you enter your password on a website, the actual password you enter isn't stored anywhere, it gets hashed. In short, it's mathematically tuned into an unrecognizable compilation of letters and numbers. the longer the password the more time it takes to hack.

**Back to Regularly Scheduled Programming: Specifying Virtual Hardware**

Allocating base memory, an amount of CPUs and disk size is important because it determines the power of your VM vs the amount of power you leave for the host device. It's good to leave a good amount of base memory in general for the host device so it's not overworking itself and you have space for normal usage. 2-4 GB is a safe place to start so I'm going with 3. 

As Far as CPUs go, similar to the Base Memory, this is how many cores you allocate to the VM vs how many you leave for the host device. For this simple server 2 is okay - one for the OS and one for the overhead. In general, you want to avoid assigning more CPUs than your computer actually has cores. You'll want to take **context switching** into account when doing this - when the CPU schedueler decides to switch from one running process to another. Like if you're running a video and a music player at the same time, your CPU isn't actually doing it at the same time, it's switching processes between those two more times than you can count in a second to keep those running. 
**
For Disk Size, you want to just make sure you have more disk space on your host device than you do on your VMs total. We'll be running maybe 2 VMs here, so for this first one Im gonna allocate 25GB. I have abbout 477 available and this computer is primarily only used for Cybersecurity, so that should be enough for now.

It'll ask you if you want to confirm after this, hit finish and bow - Totodile has been born. You can also always go back and change these settings in the future if you want/need.

Once you finish here, your VM should boot Ubuntu and open the installation window. It's a bunch of clicking next, naming the distribution, and letting it setup. For this project, we'll need Active Directory so make sure to select that option when it comes up.

**Install Problem: Unattended Installation**

I ran into an issue here where Ubuntu just wouldn't install fully, I kept getting error messages. I thought That something went wrong initially because I stopped the install at one point trying to go back and have it connect Active Directory, so I deleted the ISO image and redownloaded it, but I got the error again. Turns out the installer tried to format my virtual hard drive and write the partition table, but it couldn't complete the process. It sounds like this is normally caused by the unattended installation feature in VirtualBox, meaning I had to delete the VM and recreate it without having that selected.
