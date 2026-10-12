##### IP: 209.38.170.206
##### NAME: THE LODGE
##### DIFFICULTY: INTERMEDIATE(What they said)
##### GOAL: FIND AND SUBMIT FLAG

### SCANNING

```
❯ nmap --min-rate 1000 -p- -sV 209.38.170.206
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-16 14:50 WAT
Nmap scan report for 209.38.170.206
Host is up (0.19s latency).
Not shown: 65523 closed tcp ports (reset)
PORT     STATE    SERVICE       VERSION
21/tcp   open     ftp           vsftpd 3.0.5
22/tcp   open     ssh           OpenSSH 8.9p1 Ubuntu 3ubuntu0.11 (Ubuntu Linux; protocol 2.0)
25/tcp   filtered smtp
80/tcp   filtered http
135/tcp  filtered msrpc
136/tcp  filtered profile
137/tcp  filtered netbios-ns
138/tcp  filtered netbios-dgm
139/tcp  filtered netbios-ssn
445/tcp  filtered microsoft-ds
631/tcp  open     ipp           CUPS 2.4
3389/tcp open     ms-wbt-server Microsoft Terminal Service
Service Info: OSs: Unix, Linux, Windows; CPE: cpe:/o:linux:linux_kernel, cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 83.83 seconds

```

From the above scan, we can see ftp is opened and we also have ssh opened. Another thing that looks interesting is http and port 631. Not to overwhelm ourselves, we will start by enumerating ports one by one starting from FTP as it might contain information that can lead us on.

### ENUMERATING FTP
<img width="1920" height="789" alt="image" src="https://github.com/user-attachments/assets/1c0ef22f-d2af-48af-8330-1bd322a4ecc5" />

Upon enumerating FTP, we saw a number of files which looked interesting. Don't get too excited sha coz not all that gold is glitter abi how dem dey talk am 💀😂. Shaa make we check wetin dey there

Ladies and gentlemen, the files we got from the ftp told us to play with port 22.

<img width="1920" height="1053" alt="image" src="https://github.com/user-attachments/assets/2c4cfac3-f92f-4b22-a76a-a7e4097d6c68" />

So we will do just that and see how we can brute-force for a valid credential. We will be using hydra for this with the syntax;

`hydra -L username.txt -P password.txt ssh://<ip>`

<img width="1920" height="273" alt="image" src="https://github.com/user-attachments/assets/74243a4b-59e4-41a6-aadb-046487ad120a" />
4
Easy day. Now we log in and find our flag.

### IN THE SERVER

There were alot of rabbit holes and alot of things that could lead us astray but tell me, with God on our side, how can we be led astray? 😂. Shaaa, I sha checked one `flag.txt` file and it gave me the possible next step.

```
ctfuser@ubuntu:~$ cat flag.txt 
So you thought you found the flag, huh? 😏

Well... gather 'round, adventurer. Let me tell you a tale.

🧙 Long ago, in the ancient server of `ctf-lab-001`, there lived a bold security intern named Kevin. Kevin was tasked with hiding one single flag. But Kevin had trust issues.

So he didn’t just hide the flag… oh no. He encoded it. Split it. Compressed it. Hid it in a zip within a zip within a bash alias. Then base64’d it. Then… *he forgot where he put it.* 😩

Legend says Kevin now wanders the Linux file system, `cd`-ing from folder to folder, whispering "grep... grep... grep..." in the night. 👻

Moral of the story?

This ain't the flag.

💀 FLAG{Almost_Got_Me_Lol} 💀

But don’t give up now. The real flag is waiting... somewhere *deeper* 🧠🔥

Hint: Sometimes, it’s the config you never check. 😉

Good luck, cyberwarrior. 🫡

```

- An intern named Kevin(might not even be alive)
- We are looking for something encoded
- The flag is scattered and hidden
- We have something to do with bash alias and zip file
- Some of this might be a lie

First things first, we check hidden files to see what we can find any interesting thing there before we check the alias whatever. 

<img width="882" height="900" alt="image" src="https://github.com/user-attachments/assets/9c9e28a2-8c2b-41d9-9adf-d05c8c1a82e8" />

The first part looks sweet, so we will keep it and save it and then if you look at it well, it the pattern is enctypted+something== which could mean the first part is the beginning of our flag. We also got a note about backups(could be rabbit hole).

<img width="1369" height="260" alt="image" src="https://github.com/user-attachments/assets/60540ee6-4a8d-4bf6-99eb-6107e8b5c877" />

Nice one Mr Clifford. I see what you did there 😂👏. Onto the next sha, searching through our bash files starting from the .bash_aliases it'self.

> [!Bash alias] What are bash aliases
> Bash aliases are shortcuts that allow you to run long commands using a shorter name, making it easier and faster to work in the terminal.

Now we will start with the main file. That is `bash_aliases` to see what is there

<img width="1033" height="363" alt="image" src="https://github.com/user-attachments/assets/ce03eee5-b394-4ceb-8ee6-953f80dc3844" />

Nothing was found there so we checked the `bashrc` file because it can also be used to set aliases. Upon checking that, we got another hint again to know what to do next.
To cut the short story long, we sha found a flag and it was hidden.

<img width="1036" height="561" alt="image" src="https://github.com/user-attachments/assets/26b55cda-3a54-47c0-a67d-22a634ec763a" />

My first reaction when I saw this was "damn!!! I be Mr Robot joor 😂" until i tried to decrypt it and it was giving me something I can't read or understand. Then i noticed there is a space in between the cup(something arsenal don't have) and the strings of character looking like base 64. I decided to add the first set of strings that was seen earlier.

<img width="709" height="130" alt="image" src="https://github.com/user-attachments/assets/3694a521-e88d-498d-8761-0a19d70ee107" />

And boom!!! 💥. That was it. 


