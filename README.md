# PS_Profile
My collection of alias and functions for PowerShell

## Beautiful terminal

![image](https://github.com/DenisGas/PS_Profile/assets/81939899/edad5c5e-3d80-4d2e-9ae4-de8c7d2209a0)

### Links

[folder icons](https://www.powershellgallery.com/packages/Terminal-Icons/0.11.0)

#### Prompt theme engine for shell.

[ohmyposh](https://ohmyposh.dev/docs/installation/windows)

OR

[starship](https://starship.rs/guide/#%F0%9F%9A%80-installation)


## How use 

You need copy functions in you $PROFILE 


## "mls" compact analog comand "ls" 

### Function

```powershell
<#
.SYNOPSIS
Displays items in a directory using different filters.

.DESCRIPTION
The mls_alias function displays files and directories in the specified path. It can filter the output to show only files, only directories, or all items including hidden ones.

.PARAMETER Path
Specifies the path to list the items from. If not provided, it defaults to the current directory.

.PARAMETER All
Shows all items, including hidden files and directories, in a single column.

.PARAMETER Files
Shows only files in the specified directory.

.PARAMETER Directories
Shows only directories in the specified directory.

.PARAMETER Help
Displays this help information.

.EXAMPLE
mls_alias -All
Shows all items, including hidden ones, in the current directory, displayed in a single column.

.EXAMPLE
mls_alias -Files
Lists only files in the current directory.

.EXAMPLE
mls_alias -Directories
Lists only directories in the current directory.

.EXAMPLE
mls_alias -Path 'C:\Users'
Lists all items in the 'C:\Users' directory.
#>
function mls_alias {
  [CmdletBinding()]
  param (
      [string]$Path = ".",
      [switch]$All,
      [switch]$Files,
      [switch]$Directories,
      [switch]$Help
  )

  if ($Help) {
      # The Get-Help command can automatically display the help comments above.
      Get-Help $MyInvocation.MyCommand.Name
      return
  }

  try {
      if ($All) {
          Get-ChildItem $Path -Force | Format-Wide -Column 1
      } elseif ($Files) {
          Get-ChildItem $Path -File | Format-Wide
      } elseif ($Directories) {
          Get-ChildItem $Path -Directory | Format-Wide
      } else {
          Get-ChildItem $Path | Format-Wide
      }
  } catch {
      Write-Host "Error: $($_.Exception.Message)"

  }
}

Set-Alias -Name mls -Value mls_alias -Option AllScope;
```

### How use comand

`ls` command

![image](https://github.com/DenisGas/PS_Profile/assets/81939899/91982f1a-32da-4047-8d4d-75a0467fb180)

`mls` command

![image](https://github.com/DenisGas/PS_Profile/assets/81939899/d79038dc-3d3b-42b9-91fd-9d78082c82f2)

Display all items (files and folders) in the current directory:

```powershell
mls_alias
```

OR simply:

```powershell
mls_alias .
```

Display all items (including hidden items) in the current directory as a single column:

```powershell
mls_alias -All
```

Display only the files in the current directory:

```powershell
mls_alias -Files
```

Display only folders in the current directory:

```powershell
mls_alias -Directories
```

Display a help message:

```powershell
mls_alias -Help
```

Open the Documents directory and display all its contents:

```powershell
mls_alias "C:\Users\User\Documents".
```




## Analog VScode comand "code" for Visual Studio

### Function

```powershell

<#
.SYNOPSIS
Opens Visual Studio at the specified directory. If -Solution is specified, it opens the first solution file (.sln) found within the directory.

.DESCRIPTION
Opens Visual Studio at the specified directory. If -Solution is specified, it searches for and opens the first .sln file found in the directory or its subdirectories.

.PARAMETER Solution
Opens the solution file (.sln) found in the specified directory.

.PARAMETER Help
Displays this help message.

.PARAMETER Directory
Specifies the directory to use. Defaults to the current directory.

.EXAMPLE
VSOpen -Solution
Opens the first solution file in the current directory.

.EXAMPLE
VSOpen -Directory 'C:\Projects\MyProject'
Opens Visual Studio at the specified directory.

.EXAMPLE
VSOpen -Solution -Directory 'C:\Projects\MyProject'
Opens the first solution file found in 'C:\Projects\MyProject'.
#>
function VSOpen {
  [CmdletBinding()]
  param(
    [switch]$Solution,
    [switch]$Help,
    [string]$Directory = $PWD.Path
  )

  if ($Help) {
    Get-Help $MyInvocation.MyCommand.Name
    return
  }

  try {
    # Process if -Solution switch is used
    if ($Solution) {
      $solutionFile = Get-ChildItem -Path $Directory -Filter *.sln -Recurse -File | Select-Object -First 1

      if ($solutionFile) {
        Write-Output "Solution file found: $($solutionFile.FullName)"
        Start-Process devenv.exe $solutionFile.FullName
      }
      else {
        Write-Output "Solution file not found in the specified directory and its subdirectories."
      }
    }
    else {
      # Opens Visual Studio in the specified directory if no solution file is needed
      Start-Process devenv.exe $Directory
    }
  }
  catch {
    Write-Error "An error occurred: $_"
  }
  finally {
    # Ensure to return to the original directory (not necessarily needed unless further commands rely on it)
    Set-Location $originalDirectory
  }
}

Set-Alias -Name vs -Value VSOpen -Option AllScope



```

### How use comand

#### Open the Current Directory in Visual Studio

You can open the current directory directly in Visual Studio using either of the following commands:

```powershell
vs .
```

OR

```powershell
vs
```

Both commands will launch Visual Studio with the current directory loaded.

#### Open the First Solution Found (.sln) in the Current Directory and Its Subdirectories

To open the first `.sln` file that is found in the current directory and its subdirectories, use:

```powershell
vs -Solution
```

This command searches for the first solution file in the current and all nested directories, and opens it in Visual Studio.

#### Show Help Message

If you need assistance or want to see the usage information for the `VSOpen` function, type:

```powershell
vs -Help
```

This command displays help information about how to use the `VSOpen` command, detailing all available parameters.

#### Open a Specific Directory in Visual Studio

To open a specific directory in Visual Studio, such as the "repos" directory located at `C:\Users\user\repos`, use the command:

```powershell
vs -Directory "C:\Users\user\repos"
```

This command launches Visual Studio and loads the specified directory.
