# Delphi Starter Edition で作成したプロジェクトに Delphi Community Edition で Win64 が追加できない件について

Community Edition で新規に作成したプロジェクトに、ターゲットプラットフォームとして 64bit Windows を追加することはできるのですが、以前に Starter Edition で作成したプロジェクトには出来ないという問題に遭遇しました. (プラットフォームの追加メニューがグレーアウトしててできない)

Community Edition で新規に作成したプロジェクトの dproj ファイルを眺めたところ 64bit Windows が追加される前から、以下のように Win64 の記述があるようです.
※ dproj は XML ファイルです. 念の為.

```xml
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    ....
    <ProjectExtensions>
        ....
        <BorlandProject>
            ....
            <Platforms>
                <Platform value="Win32">True</Platform>
                <Platform value="Win64">False</Platform>
            </Platforms>
        </BorlandProject>
        ....
    </ProjectExtensions>
    ....
</Project>
```

この`<Platform value="Win64">False</Platform>`を Starter Edition で作成したプロジェクトの dproj に加えたところ、メニューのグレーアウトがなくなり、64bit Windows が追加できました.
