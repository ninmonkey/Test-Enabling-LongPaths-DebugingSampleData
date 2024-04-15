## About

This is an experiment to see what the limits of MAX_PATHs are. 

- [Here's the script that I used to create this repository](./Example-TestingLongPaths.ps1)

## tldr

Path segments by default are limited to 255 chars, but the fullpath can be up to around 32,000 chars.

For apps that don't work, If I translate the path to the DOS/Shortname format, then they work.

```ps1
$file = gi 'G:\temp\longpathTest\stuff\aaaaaaaa.....fff'
$oldFormat = 'G:\temp\LONGPA~1\stuff\AAAAAA~1\BBBBBB~1\CCCCCC~1\DDDDDD~1\EEEEEE~1\FFFFFF~1'

explorer.exe $file      # doesn't support the fullname, so it opens the root dir
explorer.exe $oldFormat # this version opens the right directory 

[Environment]::CurrentDirectory = $file.Directory.FullName # errors 
[Environment]::CurrentDirectory = $oldFormat # this worked!
# works 
```

## See more: Related Docs

- [Unicode skip MAX_PATH limitations](https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=powershell#enable-long-paths-in-windows-10-version-1607-and-later)
- [LongPathAware part of your manifest](https://learn.microsoft.com/en-us/windows/win32/sbscs/application-manifests)
- [ApplicationManifests](https://learn.microsoft.com/en-us/windows/win32/sbscs/application-manifests)
- See [Filesytem limits comparison table](https://learn.microsoft.com/en-us/windows/win32/fileio/filesystem-functionality-comparison#limits)

There's a few causes where `LongPathsEnabled` is ignored. Here's a few quotes from the above

> The maximum path of 32,767 characters is approximate,
> path is composed of components separated by backslashes, each up to the value returned in the `lpMaximumComponentLength` parameter of the `GetVolumeInformation` function (this value is commonly 255 characters)
> - Starting in Windows 10, version 1607, MAX_PATH limitations have been removed from common Win32 file and directory functions. However, you must opt-in to the new behavior.
> - Windows API has many functions that also have Unicode versions to permit an extended-length path for a **maximum total path length of 32,767 characters**. This type of path is composed of components separated by backslashes, each up to the value returned in the `lpMaximumComponentLength` parameter of the `GetVolumeInformation` function. ( That part is commonly set to 255 )
> - Because you cannot use the `"\\?\"` prefix with a relative path, **relative paths are always limited to a total of MAX_PATH characters.**
