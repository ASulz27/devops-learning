Bandit Wargames Level 1-20 Commands & Answers

Intro: 

Command: ssh bandit0@bandit.labs.overthewire.org -p 2220

Answer: Host, Port number, username and password have all been provided. Follow this format to solve this level.

ssh (user)@host -p (port number)

Bandit Level 1 → Level 2

Challenge: The password is stored in a file called - in the home directory.

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