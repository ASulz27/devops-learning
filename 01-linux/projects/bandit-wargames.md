Bandit Wargames Level 1-20 Commands & Answers
Intro

Command: ssh bandit0@bandit.labs.overthewire.org -p 2220

Answer: Host, port number, username and password have all been provided. Follow this format to solve this level.

bash
ssh (user)@host -p (port number)
Bandit Level 1 → Level 2

Challenge: The password is stored in a file called - in the home directory.

Command: ssh bandit1@bandit.labs.overthewire.org -p 2220

Solution:

bash
ls
cat ./-

Explanation:

ls shows the file - in the home directory
A bare cat - doesn't work because - is treated as "read from standard input", so the command just hangs
./- gives the file's path explicitly, so the shell and cat treat it as a filename instead of an option

Password: PK8fYLZg2hnHSz83plBL1iEPKdD3QToB

What I learned: A leading - makes the shell and most commands think it's an option or stdin. Prefixing with ./ fixes it.

Bandit Level 2 → Level 3

Challenge: The password is stored in a file called spaces in this filename in the home directory.

Command: ssh bandit2@bandit.labs.overthewire.org -p 2220

Solution:

bash
ls
cat "spaces in this filename"

Explanation:

Without quotes, the shell splits the name into four separate arguments and cat looks for four files
Wrapping the name in quotes keeps it as a single argument
Escaping each space also works: cat spaces\ in\ this\ filename
Tab completion fills in the escaping automatically

Password: 7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME

What I learned: The shell splits arguments on spaces, so filenames with spaces need quotes or backslash escapes. Tab completion saves a lot of typing here.

Bandit Level 3 → Level 4

Challenge: The password is stored in a hidden file in the inhere directory.

Command: ssh bandit3@bandit.labs.overthewire.org -p 2220

Solution:

bash
cd inhere
ls -la
cat ...Hiding-From-You

Explanation:

A plain ls shows nothing inside inhere, because the file is hidden
Files whose names start with . are hidden by default
ls -la lists all files, including hidden ones, in long format
cat then reads the hidden file like any other

Password: xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq

What I learned: Hidden files in Linux are just files that start with a dot, and ls -a reveals them.

Bandit Level 4 → Level 5

Challenge: The password is stored in the only human-readable file in the inhere directory.

Command: ssh bandit4@bandit.labs.overthewire.org -p 2220

Solution:

bash
cd inhere
file ./*
cat ./-file07

Explanation:

The directory has ten files named -file00 to -file09, and only one contains readable text
file ./* reports the type of each file, so the one marked ASCII text stands out from the data files
The ./ prefix is needed again because the filenames start with -
cat on the ASCII file prints the password. Running cat on the others would dump binary into the terminal

Password: 6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG

What I learned: file tells you what a file actually contains before you open it. That's safer than cat-ing everything and hoping for the best.

Bandit Level 5 → Level 6

Challenge: Find a file somewhere under inhere with these properties:

Human-readable
1033 bytes in size
Not executable

Command: ssh bandit5@bandit.labs.overthewire.org -p 2220

Solution:

bash
cd inhere
find . -type f -size 1033c ! -executable
cat ./maybehere07/.file2

Explanation:

find . searches from the current directory down through all subdirectories
-type f matches regular files only
-size 1033c matches files of exactly 1033 bytes (c means bytes)
! -executable excludes executable files
Only one file matches all three, and cat prints the password

Password: pXa26xhMWaC2SvDotA4r9EgZkulOeSBW

What I learned: find can filter on several properties at once, which is much faster than browsing directories by hand. The c suffix on -size means bytes, and without it the size is counted in 512-byte blocks.

Bandit Level 6 → Level 7

Challenge: The password is stored somewhere on the server and has all of these properties:

Owned by user bandit7
Owned by group bandit6
33 bytes in size

Command: ssh bandit6@bandit.labs.overthewire.org -p 2220

Solution:

bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password

Explanation:

find / searches the whole filesystem, since there's no hint about where the file is
-user bandit7 and -group bandit6 filter by owner and group
-size 33c matches files of exactly 33 bytes
2>/dev/null throws away the flood of "Permission denied" errors so only the real match is visible
cat on the one result prints the password

Password: Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3

What I learned: find can filter by owner and group as well as size. Redirecting stderr to /dev/null is what makes a search from / readable.

Bandit Level 7 → Level 8

Challenge: The password is stored in data.txt next to the word millionth.

Command: ssh bandit7@bandit.labs.overthewire.org -p 2220

Solution:

bash
grep millionth data.txt

Explanation:

data.txt is a huge file with thousands of lines, far too long to read by eye
grep millionth prints only the lines containing that word
The matching line has the password right next to the word

Password: VR1ljMayciFxbnUokuQmJFw6QC9VKtub

What I learned: grep is the quickest way to pull one line out of a big file.

Bandit Level 8 → Level 9

Challenge: The password is stored in data.txt and is the only line of text that occurs exactly once.

Command: ssh bandit8@bandit.labs.overthewire.org -p 2220

Solution:

bash
sort data.txt | uniq -u

Explanation:

uniq only compares neighbouring lines, so the input has to be sorted first to bring duplicates together
sort data.txt groups identical lines next to each other
uniq -u prints only the lines that appear once
The pipe | sends the output of sort straight into uniq

Password: EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl

What I learned: uniq doesn't work properly on unsorted input, so sort | uniq is a pair. Pipes let me chain small tools into something more useful.

Bandit Level 9 → Level 10

Challenge: The password is stored in data.txt in one of the few human-readable strings, preceded by several = characters.

Command: ssh bandit9@bandit.labs.overthewire.org -p 2220

Solution:

bash
strings data.txt | grep "=="

Explanation:

data.txt is mostly binary, so cat would fill the terminal with garbage
strings extracts only the printable text from a binary file
grep "==" narrows that down to lines containing the = characters
The password is on one of the matching lines

Password: B0s2khmbT9u0geKuOoVGW3JZKhndE3BG

What I learned: strings pulls readable text out of binary data, and combining it with grep narrows the output down quickly.

Bandit Level 10 → Level 11

Challenge: The password is stored in data.txt, which contains base64 encoded data.

Command: ssh bandit10@bandit.labs.overthewire.org -p 2220

Solution:

bash
base64 -d data.txt

Explanation:

Base64 is an encoding, not encryption. It turns binary data into plain text characters
base64 -d decodes it back to the original text
The decoded output contains the password

Password: pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro

What I learned: Base64 hides nothing, because anyone can decode it with a single command. It's for transporting data, not protecting it.

Bandit Level 11 → Level 12

Challenge: The password is stored in data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.

Command: ssh bandit11@bandit.labs.overthewire.org -p 2220

Solution:

bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'

Explanation:

This is a ROT13 cipher, where each letter is shifted 13 places through the alphabet
tr translates characters from one set to another
'A-Za-z' is the normal alphabet and 'N-ZA-Mn-za-m' is the same alphabet starting 13 letters along
Because the alphabet has 26 letters, running ROT13 twice returns the original text, so the same command encodes and decodes

Password: GROozWPO8QyN0mGrjUkID0WCYkZiQxrN

What I learned: ROT13 is a simple substitution cipher that's trivial to reverse, and tr handles it with one command.

Bandit Level 12 → Level 13

Challenge: The password is stored in data.txt, which is a hexdump of a file that has been compressed repeatedly. Work in a temporary directory.

Command: ssh bandit12@bandit.labs.overthewire.org -p 2220

Solution:

bash
tmp=$(mktemp -d)
cp data.txt $tmp
cd $tmp
xxd -r data.txt > data.bin
file data.bin

Then repeat for each layer, checking the type with file and unpacking with the matching tool:

bash
mv data.bin data.gz && gunzip data.gz        # gzip
mv data bin.bz2 && bunzip2 bin.bz2           # bzip2
tar xf data.tar                              # tar archive

Keep going until file reports ASCII text, then cat it.

Explanation:

mktemp -d creates a private temporary directory, because the home directory isn't writable
xxd -r reverses the hexdump back into the original binary
file identifies the compression format at each layer, and the file extension is misleading
Each tool needs the right extension: gunzip expects .gz, bunzip2 expects .bz2, and tar xf extracts an archive
The layers alternate between gzip, bzip2 and tar several times before the final text file appears

Password: qQYQiHOBPR8zR61qxYqX45quvihF2uzk

What I learned: Never trust a file's name. file tells you what it really is, and repeating a check-then-unpack loop works for as many layers as there are.

Bandit Level 13 → Level 14

Challenge: The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. There's no password this time. Instead, the home directory contains a private SSH key that can be used to log in as bandit14.

Command:

bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
ssh -i ssh.txt bandit14@bandit.labs.overthewire.org -p 2220

The first logs in as bandit13 to read the key. The second runs on my own machine with the key saved as ssh.txt, and logs in as bandit14.

Solution:

On the server, logged in as bandit13:

bash
ls -l
cat sshkey.private

On my own machine, after pasting the key into a new file:

bash
nano sshkey.private
chmod 600 sshkey.private
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220

Once logged in as bandit14:

bash
cat /etc/bandit_pass/bandit14

Explanation:

ls -l shows sshkey.private is owned by bandit14 with group bandit13, and the group has read permission, so I can read it as bandit13
cat prints the key, which is plain text between -----BEGIN ... PRIVATE KEY----- and -----END ... PRIVATE KEY-----
Trying ssh -i sshkey.private bandit14@localhost -p 2220 from the server fails, because the game blocks connections from the server to itself, so the login has to start from my own machine
I created a file on my machine, pasted the whole key into it, and used -i to tell SSH to log in with that key instead of a password
chmod 600 restricts the file to my user only, because SSH rejects private keys that other users can read
As bandit14, I can read /etc/bandit_pass/bandit14, which holds the password used for the next level

Password: [password here]

What I learned: SSH can log in with a private key instead of a password, and key files need tight permissions (chmod 600). I also learned that a key generated on my own machine wouldn't work, because the server only accepts keys that were set up on the account in advance.

Bandit Level 14 → Level 15

Challenge: The password for the next level is retrieved by submitting the password of the current level to port 30000 on localhost.

Command: ssh bandit14@bandit.labs.overthewire.org -p 2220

Solution:

bash
cat /etc/bandit_pass/bandit14
telnet localhost 30000

Then paste the bandit14 password at the prompt and press Enter. The service replies with Correct! followed by the password for bandit15.

Explanation:

cat /etc/bandit_pass/bandit14 prints the current level's password. It's a file that only bandit14 can read, which is why the last level needed the SSH key
telnet opens a plain text connection to a service on another host. The format is telnet [options] [host [port]]
Here the host is localhost (this same machine) and the port is 30000, both given in the level description
Something is listening on that port, and it waits for a line of text
Once connected, I submit the bandit14 password. The service checks it, prints Correct!, and returns the password for bandit15
Connecting to localhost works for this level. It's SSH that the game blocks from the server to itself, not other connections
Telnet sends everything unencrypted, which is why it's no longer used for logins. It's still handy as a quick way to talk to a service on a port
nc localhost 30000 (netcat) would do the same job

Password: aaWecNkG4FhxJQxz07uiwzVP6bJiYS65

What I learned: A service doesn't have to be a website or a login. Any program can listen on a port and respond to text, and telnet lets me talk to it directly. A port number identifies which service on a host I'm connecting to.

Bandit Level 15 → Level 16

Challenge: The password for the next level is retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.

Command: ssh bandit15@bandit.labs.overthewire.org -p 2220

Solution:

bash
openssl s_client localhost:30001

Then paste the bandit15 password at the prompt and press Enter. The service replies with Correct! followed by the password for bandit16.

Explanation:

This is the same idea as the last level, but the service on port 30001 only accepts encrypted connections, so plain telnet won't work
openssl s_client is a command-line client that opens an SSL/TLS connection to a host and port and then lets me type into it like a telnet session
localhost:30001 is the host and port in host:port form. Newer versions of openssl accept it directly, and -connect localhost:30001 is the older, equivalent way to write it
On connecting, openssl prints details about the handshake and the server's certificate before waiting for input
I submit the bandit15 password, and the service checks it and returns the bandit16 password
If it prints DONE, RENEGOTIATING or KEYUPDATE, or closes the connection after I paste, s_client is treating my input as one of its own commands. Lines starting with certain letters can trigger these. The "CONNECTED COMMANDS" section of man s_client explains this, and -quiet turns those commands off

Password: kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V

What I learned: SSL/TLS encrypts the traffic between me and the service, so unlike telnet, it can't be read in transit. openssl s_client gives me a way to talk to a TLS service by hand, and I should check the man page when a tool acts strangely.

