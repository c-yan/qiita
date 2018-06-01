# Win64 における SetDIBits 関数の hdc パラメータについて

MSDN の [SetDIBits function](https://msdn.microsoft.com/ja-jp/library/windows/desktop/dd162973(v=vs.85).aspx) には以下の記述がある.

> The device context identified by the hdc parameter is used only if the DIB_PAL_COLORS constant is set for the fuColorUse parameter; otherwise it is ignored.

DIB_PAL_COLORS ではない場合には hdc パラメータは無視される(is ignored)と書かれているが、実際には hdc を指定すると、6: ERROR_INVALID_HANDLE のエラーとなり、動作しない(Windows 10 April 2018 Update で確認). なので DIB_RGB_COLORS の場合には hdc に 0 を指定する必要がある.
