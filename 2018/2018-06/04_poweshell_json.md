# PowerShell で JSON 変換をするときに発生するアレコレ

## Q1. ConvertTo-Json で変換すると、深いところが変換されない

```powershell
PS> @{a=@{b=@{c=@{d=1}}}} | ConvertTo-Json
{
    "a":  {
              "b":  {
                        "c":  "System.Collections.Hashtable"
                    }
          }
}
```

## A1. -Depth を指定する.

```powershell
PS> @{a=@{b=@{c=@{d=1}}}} | ConvertTo-Json -Depth 100
{
    "a":  {
              "b":  {
                        "c":  {
                                  "d":  1
                              }
                    }
          }
}
```

## Q2. ConvertTo-Json で変換すると、Enum が数値になる

```powershell
PS> (Get-Date).DayOfWeek | ConvertTo-Json
4
```

## A2. Newtonsoft.Json に頼る

```powershell
PS> Import-Module .\Newtonsoft.Json.dll
PS> $settings = New-Object Newtonsoft.Json.JsonSerializerSettings -Property @{ TypeNameHandling = "None"; MaxDepth = 1024; Formatting = "Indented"; NullValueHandling = "Ignore"}
PS> $settings.Converters.Add((New-Object Newtonsoft.Json.Converters.StringEnumConverter))
PS> [Newtonsoft.Json.JsonConvert]::SerializeObject((Get-Date).DayOfWeek, $settings)
"Thursday"
```

## Q3. PSCustomObject を Newtonsoft.Json で変換すると大変なことになる

```powershell
PS> [Newtonsoft.Json.JsonConvert]::SerializeObject((Get-Date | Select-Object Year, Month, Day), $settings)
{
  "CliXml": "<Objs Version=\"1.1.0.1\" xmlns=\"http://schemas.microsoft.com/powershell/2004/04\">\r\n  <Obj RefId=\"0\">\r\n    <TN RefId=\"0\">\r\n      <T>Selected.System.DateTime</T>\r\n      <T>System.Management.Automation.PSCustomObject</T>\r\n      <T>System.Object</T>\r\n    </TN>\r\n    <ToString>@{Year=2018; Month=6; Day=14}</ToString>\r\n    <Obj RefId=\"1\">\r\n      <TNRef RefId=\"0\" />\r\n      <MS>\r\n        <I32 N=\"Year\">2018</I32>\r\n        <I32 N=\"Month\">6</I32>\r\n        <I32 N=\"Day\">14</I32>\r\n      </MS>\r\n    </Obj>\r\n    <MS>\r\n      <I32 N=\"Year\">2018</I32>\r\n      <I32 N=\"Month\">6</I32>\r\n      <I32 N=\"Day\">14</I32>\r\n    </MS>\r\n  </Obj>\r\n</Objs>"
}
```

## A3. PSCustomObject を避けるか、ConvertTo-Json を使う

```powershell
PS> $d = Get-Date
PS> [Newtonsoft.Json.JsonConvert]::SerializeObject(@{Year = $d.Year; Month = $d.Month; Day = $d.Day}, $settings)
{
  "Day": 14,
  "Year": 2018,
  "Month": 6
}
```

または

```powershell
PS> Get-Date | Select-Object Year, Month, Day | ConvertTo-Json
{
    "Year":  2018,
    "Month":  6,
    "Day":  14
}
```

## Q4. ConvertFrom-Json で変換すると、PSCustomObject になる

```powershell
PS> ('{"a": 1, "b": 2 }' | ConvertFrom-Json).GetType()

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     False    PSCustomObject                           System.Object
```

## A4. System.Web.Script.Serialization.JavaScriptSerializer を使う

```powershell
PS> (New-Object System.Web.Script.Serialization.JavaScriptSerializer).Deserialize('{"a": 1, "b": 2 }', [System.Collections.Hashtable]).GetType()

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     Hashtable                                System.Object
```
