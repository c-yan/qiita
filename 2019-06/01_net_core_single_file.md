# .NET Core 3.0 の目玉の single exe を preview6 で試してみた

.NET Core 3.0 の目玉の single exe だが、preview5 で試したところ、コンソールの Hello World が 67MB と大きかったのだが、preview6 でトリム機能が入って小さくできるようになったようなので再度試してみた.

結果 29MB とサイズは半減していた.

```
C:\Users\user\Documents\ConsoleApp1>dotnet new console
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on C:\Users\user\Documents\ConsoleApp1\ConsoleApp1.csproj...
  Restore completed in 75.44 ms for C:\Users\user\Documents\ConsoleApp1\ConsoleApp1.csproj.

Restore succeeded.


C:\Users\user\Documents\ConsoleApp1>type Program.cs
using System;

namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}

C:\Users\user\Documents\ConsoleApp1>dotnet publish -r win10-x64 -c Release /p:PublishSingleFile=true /p:PublishTrimmed=true
Microsoft (R) Build Engine version 16.2.0-preview-19278-01+d635043bd for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 92.88 ms for C:\Users\user\Documents\ConsoleApp1\ConsoleApp1.csproj.
C:\Program Files\dotnet\sdk\3.0.100-preview6-012264\Sdks\Microsoft.NET.Sdk\targets\Microsoft.NET.RuntimeIdentifierInference.targets(158,5): message NETSDK1057: You are using a preview version of .NET Core. See: https://aka.ms/dotnet-core-preview [C:\Users\user\Documents\ConsoleApp1\ConsoleApp1.csproj]
  ConsoleApp1 -> C:\Users\user\Documents\ConsoleApp1\bin\Release\netcoreapp3.0\win10-x64\ConsoleApp1.dll
C:\Program Files\dotnet\sdk\3.0.100-preview6-012264\Sdks\Microsoft.NET.Sdk\targets\Microsoft.NET.ILLink.targets(37,5): message NETSDK1101: Optimizing assemblies for size, which may change the behavior of the app. Be sure to test after publishing. See: https://aka.ms/illink [C:\Users\user\Documents\ConsoleApp1\ConsoleApp1.csproj]
  ConsoleApp1 -> C:\Users\user\Documents\ConsoleApp1\bin\Release\netcoreapp3.0\win10-x64\publish\

C:\Users\user\Documents\ConsoleApp1>dir bin\Release\netcoreapp3.0\win10-x64\publish\
 Volume in drive C has no label.
 Volume Serial Number is 02AD-7E6D

 Directory of C:\Users\user\Documents\ConsoleApp1\bin\Release\netcoreapp3.0\win10-x64\publish

06/13/2019  12:26 PM    <DIR>          .
06/13/2019  12:26 PM    <DIR>          ..
06/13/2019  12:26 PM        29,784,413 ConsoleApp1.exe
06/13/2019  12:26 PM               432 ConsoleApp1.pdb
               2 File(s)     29,784,845 bytes
               2 Dir(s)  35,051,622,400 bytes free
```
