Below is how to setup and access a world time link which is frequently used：
a world time link： https://www.worldtimebuddy.com/

1. Open power shell, and run Set-ExecutionPolicy RemoteSigned 
run powershell command, run Test-Path $Profile ,  it is checking if power shell profile exists. 
 
If result is true, just open the power shell profile under below path 
C:\Users\<user alias>\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1

2. If result is false, run below command  to create power shell profile. 
New-Item –Path $Profile –Type File –Force
notepad $Profile

3. Add some functions in this file 
 
function worldtime
{
start-process -filepath "https://www.worldtimebuddy.com/"
}
 
function setting
{
start-process -filepath " C:\Users\<user alias>\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1"
}
4. Restart the powershell, then the function will be available for usage in powershell.

