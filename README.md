# PS_Profile
My collection of alias and functions for PowerShell

## How use 

You need copy functions in you $PROFILE 


## Analog VScode comand "code" for Visual Studio

### Function

```powershell

function VSOpen {
    param(
        [switch]$Solution,
        [switch]$Help,
        [string]$Directory = $PWD
    )

    $originalDirectory = $PWD

    if ($Help) {
        Write-Output "Usage:"
        Write-Output "VSOpen [-Solution] [-Help] [-Directory <directory_path>]"
        Write-Output "Options:"
        Write-Output "-Solution    : Open solution (.sln) file if found."
        Write-Output "-Help        : Show this help message."
        Write-Output "-Directory   : Specify the directory to open. Default is the current directory."
    }
    elseif ($Solution) {
        $solutionFile = Get-ChildItem -Path $Directory -Filter *.sln -Recurse -File | Select-Object -First 1

        if ($solutionFile) {
            Write-Output "Solution file found: $($solutionFile.FullName)"
            Set-Location $Directory
            Start-Process "devenv.exe" $solutionFile.FullName
        }
        else {
            Write-Output "Solution file not found in the specified directory and its subdirectories."
        }
    }
    else {
        Set-Location $Directory
        Start-Process "devenv.exe" .
    }

    Set-Location $originalDirectory
}

Set-Alias -Name vs -Value VSOpen -Option AllScope; 


```

### How use comand

Open current directory in VS

```powershell
 vs . 
```

OR

```powershell
 vs
```

Open the first solution found (.sln) in curent directory and its subdirectories in the VS  

```powershell
 vs -s
```

Show help-message 

```powershell
 vs -h 
```

Open directory "repos" in VS

```powershell
  vs "C:\Users\user\repos" 
```
