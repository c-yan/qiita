# PowerShell の GC が壊れているとしか思えない件について

```powershell
PS> $PSVersionTable

Name                           Value
----                           -----
PSVersion                      3.0
WSManStackVersion              3.0
SerializationVersion           1.1.0.1
CLRVersion                     4.0.30319.42000
BuildVersion                   6.2.9200.22198
PSCompatibleVersions           {1.0, 2.0, 3.0}
PSRemotingProtocolVersion      2.2


PS> [GC]::Collect()
PS> [GC]::GetTotalMemory($false)
6122184                                 ※5.8MB
PS> 1..[math]::Pow(10, 8) > $null
PS> [GC]::GetTotalMemory($false)
3205013128                              ※2.98GB
PS> [GC]::Collect()
PS> [GC]::GetTotalMemory($false)
5412832                                 ※5.16MB
```

PowerShell どうなってんの?
