[Windows Terminal](https://learn.microsoft.com/en-us/windows/terminal/command-line-arguments?tabs=windows)

[F2 - Predictive IntelliSense]([Using predictors in PSReadLine - PowerShell | Microsoft Learn](https://learn.microsoft.com/en-us/powershell/scripting/learn/shell/using-predictors?view=powershell-7.4))

keybinds
```powershell
GET-PSReadLineKeyHandler # Get current keybinds

# bash style keybinds
Set-PSReadLineKeyHandler 'ctrl+e' -ScriptBlock {
  [Microsoft.PowerShell.PSConsoleReadLine]::EndOfLine()
}

Set-PSReadLineKeyHandler 'ctrl+a' -ScriptBlock {
  [Microsoft.PowerShell.PSConsoleReadLine]::BeginningOfLine()
}

Set-PSReadLineKeyHandler 'ctrl+p' -ScriptBlock {
  [Microsoft.PowerShell.PSConsoleReadLine]::Insert('UNL-Projects')
  [Microsoft.PowerShell.PSConsoleReadLine]::AcceptLine()
}

Set-PSReadLineKeyHandler 'ctrl+u' -ScriptBlock {
  [Microsoft.PowerShell.PSConsoleReadLine]::BackwardDeleteInput()
}

Set-PSReadLineKeyHandler 'ctrl+e' -ScriptBlock {
  [Microsoft.PowerShell.PSConsoleReadLine]::EndOfLine()
}

# remove annoying clear terminal keybind
Remove-PSReadlineKeyHandler -Chord ctrl+l

```

local port-forward windows to WSL2
```powershell
# add interface
netsh interface portproxy add v4tov4 listenport=<yourPortToForward> listenaddress=0.0.0.0 connectport=<yourPortToConnectToInWSL> connectaddress=<IP address of your WSL2 instance>
# del interface
netsh interface portproxy del v4tov4 listenport=<yourPortToForward> listenaddress=0.0.0.0
```

[Get-ChildItem](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7.4)
>basically ls, it is also aliased to ls
```powershell
(gci \).FullName # list all in root with absolute path
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
# create or update alias in profile.ps1
Set-Alias <ALIAS> <COMMAND> # prefer to use Set-Alias than New-Alias
# reload profile and clear buffer
. $PROFILE; clear
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
