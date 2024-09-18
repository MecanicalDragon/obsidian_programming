`$Env:var='val'` - set value *val* to the environment variable *var*.
`echo $Env:var` - print environment variable *var*.

Two ways to create custom PowerShell commands:
- Create a dedicated directory, add this directory to the `%PATH%`, put custom `.bat` scripts to this directory.
- Create a custom PowerShell profile, declare functions there, set this profile as default.

PowerShell profiles reside in
`$HOME\Documents\PowerShell\Microsoft.PowerShell_profile.ps1`

```PowerShell
# Run git bash
Function RunGitBash {
    start "C:\Progs\Git\bin\sh.exe" --login
}
Set-Alias -Name gsh -Value RunGitBash
```

```PowerShell
Function Test-Profile {
	param (
		[string]$str1,
		[string]$str2 = "default string value",
		[int]$n,
	)
	echo "str1 = $str1; str2 = $str2; n = $n; args: $args"
}
Set-Alias -Name test -Value Test-Profile
```

