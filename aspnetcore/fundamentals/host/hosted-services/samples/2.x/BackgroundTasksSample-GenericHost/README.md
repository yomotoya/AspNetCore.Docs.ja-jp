# <a name="aspnet-core-background-tasks-sample-generic-host"></a>ASP.NET Core のバックグラウンド タスクのサンプル (汎用ホスト)

このサンプルでは、[IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice) の使用方法を説明します。 このサンプルでは、「[ASP.NET Core でホステッド サービスを使用するバックグラウンド タスク](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services)」トピックで説明されている機能を示します。

[Visual Studio Code](https://code.visualstudio.com/) でサンプルを実行する場合は、*.vscode/launch.json* 内のコンソールの構成の **console** の値を、`externalTerminal` か `integratedTerminal` のいずれかに設定します。 `internalConsole` の使用は、バックグラウンド作業項目をエンキューするためにアプリが使用するコンソールのキー操作入力と互換性がありません。
