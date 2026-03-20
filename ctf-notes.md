Bandit OverTheWire Write-up (Level 0 to 5)
Level 0
Goal: Login to the game server via SSH.

Process: I used the given credentials to connect to the host.

Commands: ```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220

Password: bandit0



Level 0 -> 1
Goal: Find the password for the next level stored in a file called readme.

Logic: After logging in, I listed the files in the home directory and read the content of the only file there.

Commands:

Bash
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
# [Password Found]



Level 1 -> 2
Goal: Find the password in a file named -.

Logic: In Linux, the - character can be confused with a command option. To read this file, I used the relative path ./ to tell the system it's a filename, not a flag.

Commands:

Bash
bandit1@bandit:~$ cat ./-
# [Password Found]



Level 2 -> 3
Goal: The password is in a file with spaces in its name: spaces in this filename.

Logic: To handle spaces in the terminal, I used double quotes around the filename so the shell treats it as a single argument.

Commands:

Bash
bandit2@bandit:~$ cat "spaces in this filename"
# [Password Found]



Level 3 -> 4
Goal: Find a hidden file in the inhere directory.

Logic: Hidden files in Linux start with a dot (.). I used ls -la to list all files, including the hidden ones.

Commands:

Bash
bandit3@bandit:~$ cd inhere
bandit3@bandit:~/inhere$ ls -la
bandit3@bandit:~/inhere$ cat ...Hiding-From-You
# [Password Found]



Level 4 -> 5
Goal: Find the only human-readable file in the inhere directory.

Logic: There are multiple files, but most are binary/data. I used the file command to identify the file types. Only one was ASCII text.

Commands:

Bash
bandit4@bandit:~$ cd inhere
bandit4@bandit:~/inhere$ file ./*
./-file07: ASCII text
bandit4@bandit:~/inhere$ cat ./-file07
# [Password Found]



Level 5 -> 6
Goal: Find a specific file in the inhere directory with three properties: human-readable, 1033 bytes in size, and not executable.

Logic: There are many subdirectories and files. Using a manual search is impossible. I used the find command with specific flags to filter by file size (1033c) and excluded executable files using the ! operator.

Commands:

Bash
bandit5@bandit:~$ cd inhere
bandit5@bandit:~/inhere$ find . -type f -size 1033c ! -executable
./maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
# [Password Found]



Level 6 -> 7
Goal: Find a file hidden somewhere on the server. The properties are: owned by user bandit7, owned by group bandit6, and 33 bytes in size.

Logic: Since the file could be anywhere on the file system, I searched starting from the root directory (/). I used the -user, -group, and -size flags together to pinpoint the exact file. To avoid a screen full of permission errors, redirecting standard error to /dev/null is a better practice.

Commands:

Bash
bandit6@bandit:~$ find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
# [Password Found]



Level 7 -> 8
Goal: Find the password in a large file named data.txt, located next to the word "millionth".

Logic: Since the file contains thousands of lines, reading it manually is not an option. I used the grep command to search for the specific keyword "millionth" and filter out the rest of the data.

Commands:

Bash
bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ grep "millionth" data.txt
millionth       # [Password Found]



Level 8 -> 9
Goal: Find the only line of text in data.txt that occurs only once.

Logic: The file contains many repeating lines. To find the unique one, I first sorted the file because the uniq command only detects duplicate lines that are adjacent. By using sort followed by uniq -u, I filtered out all repeating lines. I also verified the counts using uniq --count to see that only one line appeared once.

Commands:

Bash
bandit8@bandit:~$ sort data.txt | uniq -u
# [Password Found]

# Alternative check for counts:
bandit8@bandit:~$ sort data.txt | uniq --count



Level 9 -> 10
Goal: Find a human-readable string in a binary file, preceded by several '=' characters.

Logic: The file is in binary format. I used strings to extract printable characters and grep to filter the password pattern.

Commands:

Bash
bandit9@bandit:~$ strings data.txt | grep "=="
# [Password Found]



Level 10 -> 11
Goal: Decode the password from a Base64 encoded file named data.txt.

Logic: The file contains a Base64 string. I used the base64 command with the -d (decode) flag to convert the encoded data back into human-readable text.

Commands:

Bash
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==
bandit10@bandit:~$ base64 -d data.txt
The password is # [Password Found]



Level 11 -> 12
Goal: Decode a password from data.txt where all letters are rotated by 13 positions (ROT13).

Logic: ROT13 is a simple substitution cipher. Since there are 26 letters in the alphabet, shifting by 13 twice returns the original text. I used a rotation tool to reverse the shift.

Commands:

Bash
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
# Method: ROT13 Decode
The password is # [Password Found]



Level 12 -> 13
Goal: Extract the password from a file that has been repeatedly compressed and stored as a hexdump.

Logic: This level requires identifying file types and decompressing them recursively. I started by reversing the hexdump using xxd -r. Then, I repeatedly used the file command to determine the compression format (gzip, bzip2, or tar) and applied the appropriate decompression tool until I reached the final ASCII text file.

Commands:

Bash
# Step 1: Reverse hex dump
bandit12@bandit:~$ xxd -r data.txt > data.bin

# Step 2: Decompress recursively (Example of the cycle)
bandit12@bandit:~$ file data.bin
bandit12@bandit:~$ mv data.bin data.gz
bandit12@bandit:~$ gzip -d data.gz

# Repeated the process for bzip2 and tar until ASCII text was found.
bandit12@bandit:~$ cat final_file
The password is # [Password Found]



📝 Level 13 -> 14
Goal: Use a private SSH key to log in as bandit14 and retrieve the password from /etc/bandit_pass/bandit14.

Logic: Instead of a password, I used an identity file (sshkey.private) for authentication. It is important to specify the correct port (2220) even when connecting to localhost.

Commands:

Bash
bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost -p 2220

# Once logged in as bandit14:
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
# [Password Found]



Level 14 -> 15
Goal: Submit the current level's password to port 30000 on localhost to retrieve the next password.

Logic: I used the nc (Netcat) utility to establish a TCP connection to the specified port. After sending the Bandit 14 password, the server validated it and returned the password for Level 15.

Commands:

Bash
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
# [Current Password]
bandit14@bandit:~$ nc localhost 30000
# [Paste Password and Press Enter]
Correct!
# [Password Found]



Level 15 -> 16
Goal: Submit the password to a specific but unknown port between 31000-32000 using SSL.

Logic: First, I performed a port scan using nmap to find open ports in the given range. Then, I used version detection (-sV) to identify which port was running an SSL service. Once found, I connected using ncat --ssl.

Commands:

Bash
bandit15@bandit:~$ nmap -p 31000-32000 -sV localhost
# Found the port running SSL
bandit15@bandit:~$ ncat --ssl localhost <PORT>
# Result: Received RSA Private Key



Level 16 -> 17
Goal: Identify the correct SSL port in the 31000-32000 range and retrieve the RSA private key for the next level.

Logic: 1. Discovery: I used nmap to scan the range and found 5 open ports.
2. Verification: I tested the ports with ncat --ssl. Port 31790 was the only one that validated the password and returned an RSA Private Key instead of just echoing my input.
3. Authentication: I saved the key to a file and set strict permissions (chmod 400) because SSH clients reject private keys that are "too open" (accessible by others).

Commands:

Bash
bandit16@bandit:~$ nmap localhost -p 31000-32000
bandit16@bandit:~$ ncat --ssl localhost 31790
# [RSA Key Received]

# On local machine (Kali):
chmod 400 bandit17.key
ssh -i bandit17.key bandit17@bandit.labs.overthewire.org -p 2220



Level 17 -> 18
Goal: Find the difference between passwords.old and passwords.new and log in to the next level.

Logic: 1. Comparison: I used diff to find the unique line in passwords.new.
2. Bypassing the Logout: The bandit18 shell is configured to log out immediately upon login. To bypass this, I used SSH to execute commands directly without requesting an interactive shell.

Commands:

Bash
bandit17@bandit:~$ diff passwords.old passwords.new
# Result: [Password Found]

# Login workaround:
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"



Level 18 -> 19
Goal: Retrieve the password from readme in a home directory where the .bashrc file is modified to log you out immediately upon login.

Logic: When an SSH session is initiated, it normally starts an interactive shell. If the login scripts (.bashrc, .profile) contain an exit command, the session closes. I bypassed this by passing the cat readme command directly to the SSH client, which executes the command and returns the output without starting an interactive shell.

Commands:

Bash
# Bypassing the auto-logout
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
# Result: [Password Found]



Level 19 -> 20
Goal: Retrieve the password for Bandit 20 using a SUID binary.

Logic: The binary bandit20-do has the SUID bit set and is owned by bandit20. This allows any user to execute commands with bandit20's privileges. I used this to read the password file that is otherwise restricted.

Commands:

Bash
bandit19@bandit:~$ id
bandit19@bandit:~$ ./bandit20-do id
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
# [Password Found]



Level 20 -> 21
Goal: Use a local network connection to pass the current password to a SUID binary.

Logic: Since the binary suconnect (running as bandit21) expects the current password over a network socket, I simulated a server using nc. By running it in the background (&), I was able to trigger the client (suconnect) to connect to my own listener.

Commands:

Bash
# 1. Start the listener with the password in the background
echo "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -lp 12345 &

# 2. Run the SUID program and point it to our listener
./suconnect 12345
Password matches, sending next password
# [Password Found]



Level 21 -> 22
Goal: Retrieve the password for Bandit 22 by analyzing a scheduled cron job.

Logic: I examined the cron configuration in /etc/cron.d/ to find a script running as bandit22. The script redirects the password into a publicly readable file in /tmp. I simply read that temporary file to get the password.

Commands:

Bash
bandit21@bandit:~$ ls /etc/cron.d/
bandit21@bandit:~$ cat /etc/cron.d/cronjob_bandit22
bandit21@bandit:~$ cat /usr/bin/cronjob_bandit22.sh
bandit21@bandit:~$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
# [Password Found]



📝 Level 22 -> 23
Goal: Find the password for Bandit 23 by replicating the filename generation logic used in a cron script.

Logic: The script /usr/bin/cronjob_bandit23.sh generates a unique filename in /tmp based on the MD5 hash of the string "I am user bandit23". By manually running the same command, I identified the target filename and read the password stored inside it.

Commands:

Bash
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23
bandit22@bandit:~$ cat /usr/bin/cronjob_bandit23.sh
# Manually calculate the filename for bandit23
bandit22@bandit:~$ echo I am user bandit23 | md5sum | cut -d ' ' -f 1
# Read the resulting file in /tmp
bandit22@bandit:~$ cat /tmp/[calculated_hash]
# [Password Found]



📝 Level 23 -> 24
Goal: Execute a custom script via a cron job to leak the bandit24 password.

Logic: I encountered an "event not found" error because of the ! character in double quotes. Using single quotes resolved this. I created a script that reads the restricted password file and outputs it to a world-readable directory. Once the bandit24 cron job executed my script, the password became accessible.

Commands:

Bash
bandit23@bandit:~$ mkdir /tmp/mywork_123
bandit23@bandit:~$ cd /tmp/mywork_123
bandit23@bandit:~$ echo '#!/bin/bash' > hack.sh
bandit23@bandit:~$ echo 'cat /etc/bandit_pass/bandit24 > /tmp/mywork_123/password.txt' >> hack.sh
bandit23@bandit:~$ chmod 777 hack.sh
bandit23@bandit:~$ cp hack.sh /var/spool/bandit24/foo/
# After 1 minute:
bandit23@bandit:~$ cat password.txt
# [Password Found]



Level 24 -> 25
Goal: Brute-force a 4-digit PIN code on a network service to retrieve the Bandit 25 password.

Logic: A daemon on port 30002 requires the current password and a secret 4-digit PIN. Since there are only 10,000 possibilities (0000-9999), I wrote a bash script to automate the process. Instead of opening a new connection for each attempt, I piped the entire list of combinations into a single nc (netcat) session to comply with the "no need to create new connections" hint.

Commands:

Bash
bandit24@bandit:/tmp/tufan2$ cat myscript.sh
#!/bin/bash
for i in {0000..9999}; do
    echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i"
done | nc localhost 30002 | grep -v "Wrong"

bandit24@bandit:/tmp/tufan2$ ./myscript.sh
# [Wait for the script to finish...]
# Correct! The password for bandit25 is [Password Found]



Level 25 -> 26
Goal: Break out of a restricted shell (showtext) to gain an interactive bash session as Bandit 26.

Logic: User bandit26 has a custom shell that runs more on a text file and then logs out. By shrinking the terminal window, I forced the more command to pause. From the pause state, I used the v shortcut to open the vi editor. Since vi allows executing shell commands, I reconfigured the shell variable and escaped to a full bash session.

Commands:

Bash
# 1. On your LOCAL machine, resize the terminal window to be very small (2-3 rows).
# 2. Connect directly to bandit26 using the key found in bandit25:
ssh -i bandit26.key bandit26@bandit.labs.overthewire.org -p 2220

# 3. Once the "--More--" prompt appears at the bottom, press 'v' to open vi.

# 4. Inside the vi editor, type the following commands (press Enter after each):
:set shell=/bin/bash
:shell

# 5. You are now in a bash shell as bandit26! Locate the password:
bandit26@bandit:~$ ls
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit26
# [Password Found]



Level 26 -> 27
Goal: Retrieve the password for Bandit 27 by exploiting an SUID binary after escaping a restricted shell.

Logic: Once I successfully escaped the restricted showtext shell using the vi editor trick, I gained access to an interactive bash session as bandit26. In the home directory, I found an SUID binary named bandit27-do. Since this binary is owned by bandit27 and has the SUID bit set, it allows execution of commands with bandit27's privileges. I used it to read the protected password file for the next level.

Commands:

Bash
# 1. Inside the escaped bash shell, list files
bandit26@bandit:~$ ls -la
# Output shows: bandit27-do (SUID binary owned by bandit27)

# 2. Check current privileges and effective UID
bandit26@bandit:~$ ./bandit27-do id
# uid=11026(bandit26) euid=11027(bandit27)

# 3. Read the password for Bandit 27
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
# Result: # [Password Found]



Level 27 -> 28
Goal: Clone a Git repository from a local machine because localhost connections are restricted on the game server.

Logic: The Bandit server blocks SSH/Git connections originating from itself (localhost) to save resources. Therefore, the repository must be cloned from an external machine. By specifying the correct port (2220) and using the Bandit 27 password for the bandit27-git user, the repository can be accessed.

Commands:

Bash
# Execute this on your LOCAL terminal
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo

# Authenticate with bandit27 password
# cd into the repo and find the next password
cd repo
cat README
# [Password Found]



Level 28 -> 29
Goal: Recover a censored password by inspecting the Git commit history.

Logic: Developers sometimes accidentally commit sensitive information (like passwords) and then try to hide it in a later commit. Since Git tracks every change, we can use git show to see the exact difference (diff) between the current version and the previous one. The password was found in the "removed" line of the most recent commit.

Commands:

Bash
# 1. Enter the cloned repository
cd repo

# 2. View the latest changes and differences
git show

# 3. Analyze the diff output
# Look for the '-' line which indicates what was there before the "fix info leak" commit.
# - password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
# + password: xxxxxxxxxx

# [Password Found]



Level 29 -> 30
Goal: Find the hidden password by exploring different Git branches.

Logic: Git allows developers to work on different versions of a project using branches. Sometimes, sensitive data is removed from the master branch but still exists in other branches like dev or testing. By listing all branches and switching to them, we can find information that was meant to be kept out of the production version.

Commands:

Bash
# 1. Clone the repository
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo

# 2. Check the master branch (Empty/Censored)
cat README.md

# 3. List all remote branches
git branch -r

# 4. Switch to the 'dev' branch
git checkout dev

# 5. Read the password from the dev branch version of the file
cat README.md
# [Password Found]



Level 30 -> 31
Goal: Discover the hidden password by inspecting Git tags.

Logic: In Git, a tag is a pointer to a specific point in history, usually used to mark release versions (like v1.0). However, tags can also store information or point to commits that aren't easily visible in the main branch history. By listing the tags and showing the content of the "secret" tag, I uncovered the next password.

Commands:

Bash
# 1. Clone the repository and enter the directory
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo
cd repo

# 2. Check the obvious files (Empty/Redacted)
cat README.md

# 3. List all tags in the repository
git tag
# Output: secret

# 4. Show the data associated with the 'secret' tag
git show secret
# [Password Found]



Level 31 -> 32
Goal: Submit a file to a remote Git repository to trigger an automated password release.

Logic: I encountered a "pre-receive hook declined" error, which is expected in this level. The server-side script validates the pushed content, prints the password to the console, and then intentionally rejects the push to keep the repository in its original state.

Commands:

Bash
# 1. Create the key file (as instructed by README)
echo "May I come in?" > key.txt
# 2. Add, commit, and push
git add -f key.txt
git commit -m "pushing key"
git push origin master

# 3. Read the 'remote' output from the server
# [Password Found]



Level 32 -> 33
Goal: Escape from a restricted "Uppercase Shell" that converts every command to capital letters.

Logic: Since Linux commands are case-sensitive (e.g., LS is not the same as ls), the uppercase filter prevents normal command execution. However, using the positional parameter $0 (which represents the name of the current shell) allows a user to spawn a new, unrestricted sub-shell. Once inside the sub-shell, the filter is bypassed.

Commands:

Bash
# Inside the Uppercase Shell:
>> $0
# A standard shell prompt ($) appears.
$ ls
$ cat /etc/bandit_pass/bandit33
# [Password Found]