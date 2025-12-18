***Root Me WriteUP***

--> In this room we started after deploying the machine, our first step is the **reconnaissance**:
1. I ran nmap on the given IP to check open ports    
        ```
        nmap -sS -sV -O -vvvv -T3 MACHINE_IP
        ```
I found that there are open ports: 22(ssh), 80(http). from this I answered the first task in the room: "how many ports are open?".

2. Also from the Scan I found The versions of the services running on the open ports, so I answered the second task: "What version of Apache is running?".
    
3. As I mintioned the port 22 is open with (ssh) which answeres the third task: "What service is running on port 22?".
    
4. I ran the gobuster on the MACHINE_IP to enumerate the machine:
        
        gobuster dir --url=MACHINE_IP --wordlist=/path/to/the/word/list
        
Then I checked the fourth task: "Find directories on the web server using the GoBuster tool."

5. Through the results of the gobuster, I answered the fifth task: "What is the hidden directory?"

here I finished my recon phase.
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

--> In this phase, my aim was to gain a shell on the server. I found an upload page through the fuzzing using the gobuster and I found that it permit the upload of ```.php5``` files. I searched pentestkey for a php reverse-shell payload. I downloaded it, edited it to put my IP and my recieving port then I turned my netcat to listen:

        nc -lnvp PORT

Now, I uploaded the reverse-shell payload to the server. then I changed the dir in the MACHINE_IP/uploads and clicked the file I've uploaded to run the reverse-shell. And I got it! Now it's time to search for the first flag, I used "find" command to get the flag:

    find / -name FILE_NAME 2>/dev/null

After I found it I used cat to read its content. Guess what? I found the flag!

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

--> Again I used "find" to find the files running with ```SUID```:

        find / -user root -perm 4000 2>/dev/null

I found that python is running using ```SUID```, so I moved to GTFObins to search for python exploit for privilage the shell. I found:

        python -c 'import os; os.execl("/bin/sh", "sh", "-p")'

Now, I ran ```whoami``` and my shell said ```root```. So, I started searching for the final flag, I found it, read it and ***Solved_The_Room***

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
Author: Seif Eldien Ahmad - tryhackme: 0xdaphantom
