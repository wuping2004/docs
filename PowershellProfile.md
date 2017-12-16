- Create profile for powershell
Test-path $profile
New-Item -path $profile -type file –force

function sd
{
    shutdown -f -t 0
}
