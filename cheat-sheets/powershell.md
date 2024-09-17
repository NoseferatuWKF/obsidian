[Windows Terminal](https://learn.microsoft.com/en-us/windows/terminal/command-line-arguments?tabs=windows)
```powershell
wt sp # will split either horizontal/vertical
wt sp -H # horizonal split
wt sp -V # vertical split
wt nt # new tab
```

[F2 - Predictive IntelliSense]([Using predictors in PSReadLine - PowerShell | Microsoft Learn](https://learn.microsoft.com/en-us/powershell/scripting/learn/shell/using-predictors?view=powershell-7.4))

local port-forward windows to WSL2
```powershell
# add interface
netsh interface portproxy add v4tov4 listenport=<yourPortToForward> listenaddress=0.0.0.0 connectport=<yourPortToConnectToInWSL> connectaddress=<IP address of your WSL2 instance>
# del interface
netsh interface portproxy del v4tov4 listenport=<yourPortToForward> listenaddress=0.0.0.0
```

[Test-NetConnection](https://learn.microsoft.com/en-us/powershell/module/nettcpip/test-netconnection?view=windowsserver2022-ps)
```powershell
Test-NetConnection -ComputerName <host> -Port <port>
```

[Get-ChildItem](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7.4)
>basically ls, it is also aliased to ls
```powershell
(gci \).FullName # list all in root with absolute path
```

cli history
```powershell
cat (Get-PSReadlineOption).HistorySavePath
```

env
```powershell
$env:SomeVariable = "SomeValue"

# get all env
gci env:* # also works with ls
```

args and vars
```powershell
# print on command line
Write-Host $args
# run with a command
npm $args
# declaring a new var
$foo = "hello"
# append to var
$foo += " world"
echo $foo # hello world
```

alias
```powershell
# open profile
notepad $PROFILE
# add alias in profile.ps1
New-Alias <ALIAS> <COMMAND>
# reload profile
. $PROFILE
```

functions
```powershell
# script.ps1
function Hello() {
	echo 'hello'
}

Hello
# can also execute single function using . or &
# ex: ./script.ps1; Hello
```

path
```powershell
$env:Path += ";C:\Users\user\AppData\Local\Programs\oh-my-posh\bin"
```

iis
```powershell
iisreset /status
iisreset /stop
iisreset /start
iisreset /restart # or just iisreset
```

symlink
```powershell
# Create symlink
New-Item -Path 'Target' -ItemType SymbolicLink -Value 'Source'

# Remove symlink
(Get-Item 'Target').Delete()
```

line breaks
```powershell
function Foo() {
	# some long command.. -- use ` to add line break
	# cont. long command `
	# cont. long command `
	# end
}
```

double hypen flags
```powershell
# use array splitters
$param = @(
	'--rm'
	'--name', 'interceder')
	
docker run @param -d -h interceder `
	-p 3435:3435 `
	-e AWSSecret=${env:AWSSecret} `
	-e AuthSignature=${env:AuthSignature} `
	-e OrgId=${env:OrgId} `
	ghcr.io/noseferatuwkf/interceder:latest
```