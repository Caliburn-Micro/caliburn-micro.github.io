---
layout: page
title: Obtain and build the code
---

## Want to checkout the project
### You can build it from source

### Caliburn.Micro uses Git for source control. 
>We recommend you use [GitHub Desktop client][gd], [GitKracken][gk] or [GitAhead][ga].


- [Get the code](#get-the-code)
- [Build the code](#build-the-code)
- [Troubleshooting](#troubleshooting)


## Get the code
### 1. Via the Caliburn.Micro git repository
1. The Current release can be found on the [project page][pp].
1. Select the green __Code__ in the upper right.
    - Copy the address for cloning 
    - Or download the zip file.
1. __Clone__ the project into your directory of choosing (or unzip)
    - example: `git clone https://github.com/Caliburn-Micro/Caliburn.Micro.git`
         
### 2. Via the Caliburn.Micro git release page
2. [Releases page][gr] will get you specific commits that you can clone or download as a zip or tar.gz file.
2. Follow the same instructions as [Via the Caliburn.Micro git repository](#1-via-the-caliburnmicro-git-repository)
2. Checkout the specific commit you wish to work with using the `commit_sha`(Looks like: 56adb07)
   - code: `git checkout -b <commit_sha>`

## Build the code 
1. Navigate to the folder where you cloned or unzipped the repository.
2. Navigate to the `src` directory.
3. Open `Caliburn.Micro.sln`.
4. Press Ctrl+Shift+B (or use the Build menu) to build the solution.
5. The assemblies will be generated in the `bin` directory under the root of the repository.

## Troubleshooting
### Projects in the solution were not loaded correctly
- Problem: 
  * When opening Caliburn.Micro.sln the project files show up as unloaded with the following error: `error  : The expression "[System.IO.Path]::GetDirectoryName('')" cannot be evaluated. The path is not of a legal form.`
- Solution:
  1. Close the project solution
  1. Open a terminal (powershell or command prompt)
  1. Find your .NET version by typing:
   ``` shell
   dotnet --version
   ```
   - example response:
   ```shell
   3.1.402
   ```
  1. In the Caliburn.Micro\sln folder open the Global.json file and edit the "sdk" to match your dotnet version.
     - example (original):
     ```
     {
     "sdk": {
         "version": "3.1.101"
        },
        "msbuild-sdks": {
           "MSBuild.Sdk.Extras": "2.0.54"
        }
     }  
     ```
       - example (edited):
     ```
     {
     "sdk": {
         "version": "3.1.402"
        },
        "msbuild-sdks": {
           "MSBuild.Sdk.Extras": "2.0.54"
        }
     }  
     ``` 
  1. Reload your project
   
### Need more help?
* Post a question on [stackoverflow][so] with the tag [Caliburn.Micro]
>Or
* Open an issue on [Github][issue]


[gd]: https://desktop.github.com/
[gk]: https://www.gitkraken.com/
[ga]: https://gitahead.github.io/gitahead.com/
[pp]: https://github.com/Caliburn-Micro/Caliburn.Micro
[gr]: https://github.com/Caliburn-Micro/Caliburn.Micro/releases 
[so]: https://stackoverflow.com/
[issue]: https://github.com/Caliburn-Micro/Caliburn.Micro/issues