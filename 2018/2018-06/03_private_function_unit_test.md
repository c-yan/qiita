# 非公開関数のユニットテストについて (C#)

備忘録として.

非公開関数のユニットテストをしたいときには以下のようにする.

1. 対象のアクセス修飾子を`internal`にする.
2. 対象のプロジェクトの`Properties\AssemblyInfo.cs`を開き、`[assembly: InternalsVisibleTo("ユニットテストのアセンブリ名")]`を追記する.
