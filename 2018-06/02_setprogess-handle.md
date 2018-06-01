# ITaskbarList3.SetProgress[State|Value] に指定する Handle について (Delphi)

Windows 7 でタスクバーで進捗表示をする機能が実装された. Delphi 10.2.3 で以下の通り PoC を実装したところ動作した.

```
procedure TForm1.Button1Click(Sender: TObject);
var
  I: Integer;
  TaskBarList : ITaskbarList3;
begin
  TaskBarList := CreateComObject(CLSID_TaskbarList) as ITaskBarList3;
  TaskBarList.SetProgressState(Handle, TBPF_NORMAL);
  for I := 0 to 100 do
  begin
    TaskBarList.SetProgressValue(Handle, I, 100);
    Application.ProcessMessages;
    Sleep(100);
  end;
  TaskBarList.SetProgressState(Handle, TBPF_NOPROGRESS);
  TaskBarList := nil;
end;
```

ところが、このコードを Delphi XE2 で開発しているアプリケーションに持っていったところ、動作しなかった. ピンときて `Handle` を `Application.Handle` にしたところ動作した.

Delphi はウインドウ管理が MDI (Multiple Documents Interface) のような設計になっており、SDI (Single Document Interface) アプリケーションでも MDI のような振る舞いをする. そのせいで過去にも、最小化のアニメーションがないだの、タスクバーで右クリックしたときのメニューが普通と違うだの、微妙な挙動の違い問題が発生していた. なので、タスクバーに紐付いている本当の `Window Handle` である `Application.Handle` を渡したら動作したわけだ.

しかし、MDI とか死語になったなあ. EXCEL が SDI のフリをした MDI である以外に、広く使われている MDI アプリケーションは残っているのだろうか. って、EXCEL も 2013 から SDI になってしまったのか(チーン).

## 追記:

Delphi2007 以降であれば、以下の行を dpr ファイルに書くことにより、メインフォームの `Handle` がタスクバーに紐付いたものになるよう(また、新規にプロジェクトを作成すれば以下の行が含まれる).

```
Application.MainFormOnTaskbar := True;
```

Delphi XE2 で開発しているアプリケーションで動作しなかったのは、Delphi 4 から引き継いできたソースコードが原因だった.
