# file-association

# Set Default Applications in Windows

This repository provides a utility for managing file type associations in Windows using a batch script.

## Requirements

- Windows Operating System
- SetUserFTA Utility
- ProgID of the desired default application

## Using the Batch Script

1. **Disable Command Echo**: We use `@echo off` to prevent the command prompt from displaying the commands as they are executed.

2. **Change Directory**: `cd /d %~dp0` changes the active directory to the directory where the batch script is located.

3. **Set Default Applications**: The lines using `SetUserFTA.exe` are where we set the default applications. Here's how the syntax works:

    `SetUserFTA.exe <extension> <ProgID>`

    - `SetUserFTA.exe`: This is the executable for the SetUserFTA utility.
    - `<extension>`: This is the file extension that you want to change the default program for. Examples include `.mkv`, `.mp4`, `.avi`, etc.
    - `<ProgID>`: This is the ProgID of the application you want to set as the default for the given file extension.

For example, `SetUserFTA.exe .mkv Applications\vlc.exe` sets VLC as the default program to open .mkv files.

## Finding ProgIDs

ProgIDs are stored in the Windows Registry. They can be viewed using the Registry Editor (`regedit.exe`).

1. Press `Win + R` to open the Run dialog.
2. Type `regedit` and hit `Enter`.
3. In the Registry Editor, navigate to `HKEY_CLASSES_ROOT\Applications`.
4. Expand the `Applications` key and browse through the list of applications.

You can also use PowerShell to extract ProgIDs:

```powershell
Get-ChildItem HKLM:\Software\Classes -ea 0 |
    Where-Object { $_.PSChildName -match '^\w+\.\w+$' -and (Test-Path -Path "$($_.PSPath)\CLSID") } |
    ForEach-Object { $_.PSChildName }
