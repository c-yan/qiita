# providerName="System.Data.EntityClient" にしているのに System.ArgumentException: 'Keyword not supported: 'metadata'.' が出る話

ASP.NET MVC で Entity Framework な Web アプリを Azure App Service に載せたら System.ArgumentException: 'Keyword not supported: 'metadata'.' が出て動かない.
接続文字列の providerName="System.Data.SqlClient" を providerName="System.Data.EntityClient" にすれば OK という記事がググるといっぱい出てくるが既になっている.
何故? 何故?

答え: Azure ポータルで、Azure App Service のアプリケーション設定の接続文字列を設定するとき、種類に SQL Azure を選んでいた.
Azure SQL Database を使うからとよく考えずにこれを選んでいたが、ここは providerName 相当の設定箇所であり、SQL Azure を選んだことにより System.Data.SqlClient 相当で上書きされてしまっていた. Entity Framework を使うときは、ここは Custom を選ぶのが正解.
