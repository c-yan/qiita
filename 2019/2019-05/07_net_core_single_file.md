# .NET Core 3.0 の目玉の single exe を preview5 で試してみた

.NET Core 3.0 の目玉の single exe がどれくらいのサイズになるのか興味があって、 preview5 で試してみた.
Hello World が 67MB だった.
出来た EXE を .NET Core 3.0 preview が入っていない別のマシンに持っていって動くことは確認できたが、Assembly trimmer 待ちかなあと思わされるサイズである.

```
C:\Users\user\source\repos\ConsoleApp1>type ConsoleApp1\Program.cs
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

C:\Users\user\source\repos\ConsoleApp1>dotnet publish -r win10-x64 -c Release /p:PublishSingleFile=true
Microsoft (R) Build Engine version 16.0.462+g62fb89029d for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 33.73 ms for C:\Users\user\source\repos\ConsoleApp1\ConsoleApp1\ConsoleApp1.csproj.
C:\Program Files\dotnet\sdk\3.0.100-preview5-011568\Sdks\Microsoft.NET.Sdk\targets\Microsoft.NET.RuntimeIdentifierInference.targets(157,5): message NETSDK1057: You are using a preview version of .NET Core. See: https://aka.ms/dotnet-core-preview [C:\Users\user\source\repos\ConsoleApp1\ConsoleApp1\ConsoleApp1.csproj]
  ConsoleApp1 -> C:\Users\user\source\repos\ConsoleApp1\ConsoleApp1\bin\Release\netcoreapp3.0\win10-x64\ConsoleApp1.dll
  ConsoleApp1 -> C:\Users\user\source\repos\ConsoleApp1\ConsoleApp1\bin\Release\netcoreapp3.0\win10-x64\publish\

C:\Users\user\source\repos\ConsoleApp1>dir ConsoleApp1\bin\Release\netcoreapp3.0\win10-x64\publish\
 Volume in drive C has no label.
 Volume Serial Number is 9697-D272

 Directory of C:\Users\user\source\repos\ConsoleApp1\ConsoleApp1\bin\Release\netcoreapp3.0\win10-x64\publish

05/06/2019  07:05 PM    <DIR>          .
05/06/2019  07:05 PM    <DIR>          ..
05/06/2019  07:05 PM        70,272,314 ConsoleApp1.exe
05/06/2019  07:05 PM               440 ConsoleApp1.pdb
               2 File(s)     70,272,754 bytes
               2 Dir(s)  79,950,237,696 bytes free
```
