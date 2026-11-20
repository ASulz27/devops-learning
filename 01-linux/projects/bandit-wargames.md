OverTheWire Bandit: Levels 0 → 20

Write-ups for each level: Challenge, Solution, Explanation, Password, What I learned. Connect with ssh bandit0@bandit.labs.overthewire.org -p 2220.

Bandit Level 0 → 1

Challenge: Log in over SSH on port 2220 as bandit0 (password bandit0) and find the password in a file in the home directory.
Solution: ssh bandit0@bandit.labs.overthewire.org -p 2220 → ls → cat readme
Explanation: SSH here listens on a non-default port, so -p 2220 is required.
Explanation (cont.): ls shows a file called readme, and cat prints it.
Password: 6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR (level 0 login was bandit0)
What I learned: Always pass -p when a service is not on its default port.
What I learned (cont.): Start every level with ls -la to see what is actually there.


Bandit Level 1 → 2

Challenge: The password is in a file called - in the home directory.
Solution: cat ./-
Explanation: A bare - is read by many commands as "standard input", so cat - just waits for typing.
Explanation (cont.): Prefixing ./ makes it an explicit path, so it is treated as a file name.
Password: PK8fYLZg2hnHSz83plBL1iEPKdD3QToB
What I learned: Dashed file names are confused with options or stdin.
What I learned (cont.): ./ (or --) is the standard way to avoid that.


Bandit Level 2 → 3
Challenge: The password is in a file called spaces in this filename.
Solution: cat "spaces in this filename" (or cat spaces\ in\ this\ filename)
Explanation: The shell splits arguments on spaces, so an unquoted name becomes four separate arguments.
Explanation (cont.): Quotes or backslash-escapes keep the name as a single argument.
Password: 7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME
What I learned: Quote or escape file names that contain spaces.
What I learned (cont.): Tab completion escapes them for you.

Bandit Level 3 → 4

Challenge: The password is in a hidden file inside the inhere directory.
Solution: cd inhere → ls -la → cat ./.Hiding-From-You
Explanation: Files starting with a dot are hidden from plain ls.
Explanation (cont.): ls -a shows them, and then cat reads the file as normal.
Password: xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq
What I learned: "Hidden" files are just dotfiles, not protected files.
What I learned (cont.): ls -la should be a habit.

Bandit Level 4 → 5
Challenge: Among the files in inhere, only one is human-readable. The password is in it.
Solution: cd inhere → file ./* → cat ./-file07
Explanation: The files are named -file00 to -file09, so each needs a ./ prefix.
Explanation (cont.): file reports the type of each one, and only one says ASCII text.
Password: 6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG
What I learned: file tells you what a file contains without opening it.
What I learned (cont.): Catting binary data can garble the terminal (reset fixes it).

Bandit Level 5 → 6

Challenge: Somewhere under inhere is a file that is human-readable, 1033 bytes, and not executable.
Solution: find inhere -type f -size 1033c ! -executable → cat the result
Explanation: find filters by type, size and permissions in one pass.
Explanation (cont.): The c suffix on -size means bytes, and ! negates the next test.
Password: pXa26xhMWaC2SvDotA4r9EgZkulOeSBW
What I learned: find can combine several conditions instead of me checking files by hand.
What I learned (cont.): Size units matter: c is bytes, k is kilobytes.

Bandit Level 6 → 7

Challenge: Somewhere on the server is a file owned by user bandit7, group bandit6, 33 bytes in size.
Solution: find / -user bandit7 -group bandit6 -size 33c 2>/dev/null → cat the result
Explanation: Searching from / hits many directories I cannot read, which floods the screen with errors.
Explanation (cont.): 2>/dev/null discards stderr so only the real match remains.
Password: Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3
What I learned: find can match on owner and group as well as size.
What I learned (cont.): Redirecting stderr (2>) separates errors from results.


Bandit Level 7 → 8

Challenge: The password is stored in data.txt next to the word millionth.
Solution: grep millionth data.txt
Explanation: data.txt is thousands of lines long, so reading it manually is impractical.
Explanation (cont.): grep prints only the lines that contain the search word.
Password: VR1ljMayciFxbnUokuQmJFw6QC9VKtub
What I learned: grep is the first tool for finding a known string in a big file.
What I learned (cont.): Check the output format, since the password is the second field on the line.

Bandit Level 8 → 9

Challenge: The password is the only line in data.txt that occurs exactly once.
Solution: sort data.txt | uniq -u
Explanation: uniq only compares neighbouring lines, so the input must be sorted first.
Explanation (cont.): -u prints only lines that are not repeated.
Password: EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl
What I learned: sort | uniq is a standard pairing, and order matters.
What I learned (cont.): Pipes let small tools combine into a bigger one.

Bandit Level 9 → 10

Challenge: The password is one of the few human-readable strings in data.txt, preceded by several = characters.
Solution: strings data.txt | grep "=="
Explanation: data.txt is mostly binary, and strings extracts the printable text from it.
Explanation (cont.): grep then narrows that text to lines with the = marker.
Password: B0s2khmbT9u0geKuOoVGW3JZKhndE3BG
What I learned: strings pulls readable text out of binary files.
What I learned (cont.): Chaining strings into grep is a common first step when inspecting an unknown binary.

Bandit Level 10 → 11

Challenge: The password is in data.txt, which contains base64-encoded data.
Solution: base64 -d data.txt
Explanation: Base64 turns binary or text into safe ASCII characters.
Explanation (cont.): -d decodes it back to the original text.
Password: pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro
What I learned: Base64 is an encoding, not encryption, so anyone can reverse it.
What I learned (cont.): Trailing = padding is a typical sign of base64.

Bandit Level 11 → 12

Challenge: The password is in data.txt, where all lowercase and uppercase letters have been rotated by 13 positions.
Solution: cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
Explanation: This is the ROT13 substitution cipher: each letter is replaced by the one 13 places later.
Explanation (cont.): tr maps each character in the first set to the matching one in the second.
Password: GROozWPO8QyN0mGrjUkID0WCYkZiQxrN
What I learned: ROT13 is its own inverse, because applying it twice restores the text.
What I learned (cont.): tr does character-level translation, not word-level.

Bandit Level 12 → 13

Challenge: data.txt is a hexdump of a file that has been compressed repeatedly. Recover the password.
Solution: mktemp -d → cp data.txt there → xxd -r data.txt > data.bin → loop of file, then gzip -d / bzip2 -d / tar xf (renaming as needed) until ASCII text appears
Explanation: xxd -r reverses the hexdump back into binary.
Explanation (cont.): file identifies the compression format at each layer, which decides the next command.
Password: qQYQiHOBPR8zR61qxYqX45quvihF2uzk
What I learned: Work in a mktemp -d directory, because the home directory is not writable.
What I learned (cont.): Let file guide each step instead of guessing the format.

Bandit Level 13 → 14

Challenge: The home directory holds a private SSH key for bandit14, and the password cannot be read directly.
Solution: ssh -i sshkey.private bandit14@localhost -p 2220 → cat /etc/bandit_pass/bandit14
Explanation: The key authenticates me as bandit14 without typing a password.
Explanation (cont.): Once logged in as that user, the password file is readable.
Password: aaWecNkG4FhxJQxz07uiwzVP6bJiYS65 (login: ssh -i ~/.ssh.txt bandit14@bandit.labs.overthewire.org -p 2220)
What I learned: SSH key authentication uses a private key file passed with -i.
What I learned (cont.): The key file must not be accessible to other users, or SSH refuses it.

Bandit Level 14 → 15

Challenge: Submit the current level's password to port 30000 on localhost to get the next one.
Solution: nc localhost 30000 (then paste the bandit14 password)
Explanation: nc (netcat) opens a raw TCP connection to the service.
Explanation (cont.): The service reads the password I send and replies with the next one.
Password: pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
What I learned: nc lets me talk to a network service by hand.
What I learned (cont.): The password is in /etc/bandit_pass/bandit14, readable once I'm that user.

Bandit Level 15 → 16

Challenge: Submit the current password to port 30001 on localhost using SSL/TLS encryption.
Solution: openssl s_client -connect localhost:30001 -quiet (then paste the bandit15 password)
Explanation: Plain nc cannot do the TLS handshake, which the service requires.
Explanation (cont.): openssl s_client handles the handshake and gives me an encrypted channel.
Password: kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V
What I learned: openssl s_client is the encrypted counterpart to nc.
What I learned (cont.): -quiet hides the certificate output and keeps the input flowing.

Bandit Level 16 → 17

Challenge: Find which port in 31000–32000 speaks SSL and returns credentials rather than echoing back, then use them.
Solution: nmap -p 31000-32000 localhost → openssl s_client against each SSL port → submit the bandit16 password → save the returned RSA key, chmod 600 it, ssh -i
Explanation: nmap finds the open ports, and several of them just echo my input back.
Explanation (cont.): Exactly one responds with a private key, which logs me in as bandit17.
Password: no typed password; log in with the saved key: ssh -i bandit17.private bandit17@bandit.labs.overthewire.org -p 2220
What I learned: Port scanning narrows a range down to the services that matter.
What I learned (cont.): SSH refuses a private key with loose permissions, so chmod 600 is needed.

Bandit Level 17 → 18

Challenge: Two files, passwords.old and passwords.new, differ by a single line. The password is the line that changed in passwords.new.
Solution: diff passwords.old passwords.new
Explanation: diff prints the differing lines, marked < for the old file and > for the new one.
Explanation (cont.): The password is the > line, from passwords.new.
Password: OQxXZjELndr90zuhOTDYBEomI0SZITXI
What I learned: diff is the quick way to compare two similar files.
What I learned (cont.): Read which side of the output belongs to which file.

Bandit Level 18 → 19

Challenge: The password is in readme in the home directory, but .bashrc was modified to log you out immediately.
Solution: ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
Explanation: The log showed exit-status 0 and Byebye !, a clean exit started by the shell's own startup file.
Explanation (cont.): Passing a command to ssh runs just that command on the server, so I don't have to survive the logout.
Password: KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI
What I learned: When a session exits cleanly, ask "who decided to end this?" instead of "what broke?".
What I learned (cont.): man ssh documents the command argument, which is the key to this level.

Bandit Level 19 → 20

Challenge: Use the setuid binary in bandit19's home directory to read the level 20 password from /etc/bandit_pass.
Solution: ls -l → ./bandit20-do whoami (prints bandit20) → ./bandit20-do cat /etc/bandit_pass/bandit20
Explanation: -rwsr-x--- means the s (setuid) makes the binary run as its owner, bandit20, and I can run it as a member of group bandit19.
Explanation (cont.): Each file in /etc/bandit_pass is -r-------- and readable only by its own user, so a cat run as bandit20 can read bandit20's file.
Password: 4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA
What I learned: Setuid is a borrowed key, not a master key: bandit2 gave Permission denied because the borrowed identity is just bandit20.
What I learned (cont.): Run local files with ./name (not bash name), and read errors like Is a directory as clues to fix the path.
