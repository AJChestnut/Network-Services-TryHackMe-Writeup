# Network-Services-TryHackMe-Writeup

This is a writeup for the [TryHackMe.com](https://tryhackme.com) room, [Network Services,](https://tryhackme.com/room/networkservices) created by PoloMints.

This writeup will include the answers to most questions, but, as requested by Try Hack Me, it will not include the Flags or password. 

At time of writing, this room can be found in the Cyber Defense learning path and at this [Link.](https://tryhackme.com/room/networkservices)

The tasks in this room follow a repeated pattern, Understanding, Enumerating, and Exploiting. The Understanding tasks teach you about the service you are going to be exploiting. The Enumerating tasks teach you how to collect data about the service. And then, the Exploiting tasks have you execute the exploit for each service. 

The VMs that this room uses are all in the Enumerating tasks; denoted by the green stack icon ![Green Stack Icon](https://user-images.githubusercontent.com/84687845/125002750-de631280-e012-11eb-8ae2-52b947763a04.JPG). Since the VMs can take anywhere from 1-5 minutes to spin up, I recommend that before you start an Understand task, hop down to the Enumerating task and startup the machine. That gives it time to set up while you go back and run through the Understanding task.

Also, if you are as new to TryHackMe as I am, you may have only experienced VMs that you can connect to with OpenVPN by entering their IP address into your browser, usually with a port number mentioned in the tasks, which then takes you to a GUI. That is not the case with this room. These VMs are set up to provide the services you are testing and only those services. That means you will be using a Terminal on your local machine, the Attack Box, or if you are a premium user, the browser-based Kali box to communicate with the VMs. I used the browser-based Kali box. 

If you are going to be using your local machine, I would suggest using either a Linux machine or Windows Subsystem for Linux with Kali/your favorite flavor installed. All of the tools you will need are preinstalled with Kali, but can be added to other distros. 

There is a part that requires a file provided in the browser-based Kali machine/the Attack Box. If you want to use your local machine, you are going to have to start up the Attack Box and download this file. You could also use the Attack Box for just Task 10. 

***How to Download using WinSCP on Windows 10***

If you would like to download the 136MB file and use your local terminal, here is how to do it on a Windows Machine. For the transfer, I used WinSCP. First, start the splitview Attack Box. Then once you have access to the Desktop, click on the "i" located at the bottom left of the screen ![Information Icon](https://user-images.githubusercontent.com/84687845/125015266-630e5a80-e02c-11eb-808c-ea9e6e2e73be.jpg). That will give you the Attack Box's IP address, Username, and Password. Then, in WinSCP, on the login screen, select "New Site," then enter the IP address of the Attack Box as the Host Name, the username as the username, and the password as the password.

![WinSCP Login](https://user-images.githubusercontent.com/84687845/125015700-43c3fd00-e02d-11eb-9b9a-cfe60117f52a.jpg) 

Keep in mind, your information will be different from mine. Select sftp as the protocol, and it should auto-populate port 22, if not type in 22 for the port. Then click Login. You will probably get a pop-up asking if you want to "Continue connecting to unknown server..." Click Yes. Once logged in, you will be in the root directory of the Attack Box, double click "Tools" then "wordlists" (the path in WinSCP will change to /usr/share/wordlists/ it's fine.) Find the file named rockyou.txt, select it, and click download. 

![File Download](https://user-images.githubusercontent.com/84687845/125023119-6e1cb700-e03b-11eb-894e-57d0b4dd7e9c.JPG)

The Download diaglog box will open, select whatever file path you'd like, and click OK. You now have the file you need for Task 10. Remember its filepath, you'll need it. 

***End, How to Download using WinSCP on Windows 10***

***How to download using a Linux terminal***

Just like with Windows, you are going to first start the split view Attack Box. Then once you have access to the Desktop, click on the "i" located at the bottom left of the screen ![Information Icon](https://user-images.githubusercontent.com/84687845/125015266-630e5a80-e02c-11eb-808c-ea9e6e2e73be.jpg). That will give you the Attack Box's IP address, Username, and Password. 

In your terminal, you are going to type `sftp root@{Your Attack Box Private IP}` without the brackets. The username of every Attack Box I've seen is root, if yours is different, enter the username in place of root in the above command. It will then ask you for the password. Copy and Paste the password from the Machine Information window of the Attack Box. Don't worry if the password doesn't show up, just paste/type it in and hit enter. After connecting, you should get an `sftp>` prompt. If something goes wrong, double check that you have [OpenVPN connected,](https://tryhackme.com/room/openvpn) and that you are using the correct username/IP address/password. Once you have the `sftp` prompt type `get /root/Desktop/Tools/wordlists/rockyou.txt` paying close attention to capitalization. It should start downloading the file to your local system. 

![Linux sftp Download](https://user-images.githubusercontent.com/84687845/125023676-93f68b80-e03c-11eb-8c89-27b260674f14.JPG)

If you're using a similar distro to mine (Kali), it should drop the file into your user's home folder. Remember the filepath, you'll need it for Task 10.

***End, How to download using a Linux terminal***


Now, with all that out of the way, let's get to the tasks!

### Task 1 Get Connected
This task is purely informational and does not require an answer to the question. Reading through it lets you know that you need a basic understanding of navigating Linux systems and suggests a room if you need to brush up on your skills. Also, reminds you to stay hydrated.

Click the Completed button and on to the next Task.

### Task 2 Understanding SMB
This tasks teaches you about the Microsoft service Server Message Block (SMB), which is a client-server protocol used for sharing access to files, printers, and other resources across a network. As it is 

**Question 1**
What does SMB stand for. 
Server Message Block
**Question 2**
What type of protocol is SMB?
response-request
**Question 3**
What do clients connect to servers using?
TCP/IP
**Question 4**
What systems does Samba run on?
Unix

### Task 3 Enumerating SMB
This task covers the information gathering stage that is crucial to setting up a successful exploitation. 

It begins with a brief description of the port scanning tool nmap. If you want/need more experience with nmap before proceeding, it recommends a room that is part of the Red Primer series. However, this room provides all of the information that you should need to use nmap effectively and answer the questions. 

While I had previous experience with nmap, it was good to have the suggested options to acquire the data that these questions require. Remembering all of the different options is a lot. 

We are also going to use the tool enum4linux, which was new to me. enum4linux, is used to enumerate SMB shares on Windows and Linux systems. There is a list of available enum4linux options; however, the -A option, which covers all of them, is what I used. 


