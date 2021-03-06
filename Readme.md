# DllExport

*Unmanaged Exports ( .NET DllExport )*

```
Copyright (c) 2009  Robert Giesecke
Copyright (c) 2016  Denis Kuzmin <entry.reg@gmail.com>
```

[![Build status](https://ci.appveyor.com/api/projects/status/yh1pnuhaqk8h334h/branch/master?svg=true)](https://ci.appveyor.com/project/3Fs/dllexport/branch/master)
[![NuGet package](https://img.shields.io/nuget/v/DllExport.svg)](https://www.nuget.org/packages/DllExport/) 


```csharp
[DllExport("Init", CallingConvention.Cdecl)]
public static int entrypoint(IntPtr L)
{
    // ... should be called by lua script

    Lua.lua_pushcclosure(L, onProc, 0);
    Lua.lua_setglobal(L, "onKeyDown");

    return 0;
}
```
*Note: for more flexible work with Lua, use also - [LunaRoad](https://github.com/3F/LunaRoad)*

```csharp
[DllExport("Init", CallingConvention.Cdecl)]
[DllExport(CallingConvention.StdCall)] //v1.1+
[DllExport("MyFunc")]
[DllExport]
```

Where to look ? **v1.2+** provides dynamic definition of namespace, so you can use what you want ! for more details see **[here](https://github.com/3F/DllExport/issues/2)**

```cpp
    Offset(h) 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F

    000005B0                 00 C4 7B 01 00 00 00 2F 00 12 05       .�{..../...
    000005C0  00 00 02 00 00 00 00 00 00 00 00 00 00 00 26 00  ..............&.
    000005D0  20 02 00 00 00 00 00 00 00 49 2E 77 61 6E 74 2E   ........I.want.   <<<-
    000005E0  74 6F 2E 66 6C 79 00 00 00 00 00 00 00 00 00 00  to.fly..........  <<<-
    000005F0  00 00 00 00 00 00 03 00 00 00 00 00 00 00 00 00  ................
    00000600  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
    ...
    000007A0  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
    000007B0  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
    000007C0  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
    000007D0  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
    000007E0  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 3C  ...............< 
    
      - - - -            
      byte-seq via chars: 
      + Identifier        = [32]bytes
      + size of buffer    = [ 4]bytes (range: 0000 - FFF9; reserved: FFFA - FFFF)
      + buffer of n size
      - - - -
      v1.2: 01F4 - allocated buffer size    
```

----


[Initially](https://github.com/3F/DllExport/issues/3), the original tool `UnmanagedExports` was distributed by Robert Giesecke as a closed-source tool **under the [MIT License](https://opensource.org/licenses/mit-license.php)**:

* [Official page](https://sites.google.com/site/robertgiesecke/Home/uploads/unmanagedexports) - *posted Jul 9, 2009 [ updated Dec 19, 2012 ]*
* [Official NuGet Packages](https://www.nuget.org/packages/UnmanagedExports) 

Now, we will be more open ! all details [here](https://github.com/3F/DllExport/issues/3)

## License

It still under the [MIT License (MIT)](https://github.com/3F/DllExport/blob/master/LICENSE) - be a ~free~ and open

##

### How to get

Available variants:

* NuGet PM: `Install-Package DllExport`
* GetNuTool: `msbuild gnt.core /p:ngpackages="DllExport"`
* NuGet Commandline: `nuget install DllExport`
* [/releases](https://github.com/3F/DllExport/releases) ( [latest](https://github.com/3F/DllExport/releases/latest) )
* [Nightly builds](https://ci.appveyor.com/project/3Fs/dllexport/history) (`/artifacts` page). But remember: It can be unstable or not work at all. Use this for tests of latest changes.

### How to Build

No requires additional steps for you, just build as you need.

Use build.bat if you need final NuGet package as a `DllExport.<version>.nupkg` etc.
* *You do not need to do anything inside IDE, if you already have installed plugin.*


### How to Debug

For example, find the DllExport.MSBuild project in solution:

* `Properties` > `Debug`:
    * `Start Action`: set as `Start External program`
        * Add full path to **msbuild.exe**, for example: C:\Program Files (x86)\MSBuild\14.0\Bin\MSBuild.exe
    * `Start Options` > `Command line arguments` write for example:

```bash
"<path_to_SolutionFile_for_debugging>.sln" /t:Build /p:Configuration=<Configuration>
```

use additional `Diagnostic` key to msbuild if you need details from .targets
```bash
"<path_to_SolutionFile_for_debugging>.sln" /verbosity:Diagnostic /t:Rebuild /p:Configuration=<Configuration>
```

Go to `Start Debugging`. Now you can debug at runtime.

