# <a name="custom-webhost-service-sample"></a>カスタム web ホスト サービスのサンプル

このサンプルでは、Windows サービスとして IIS を使用せずに Windows 上の ASP.NET Core アプリケーションをホストするための推奨方法を示します。 このサンプルで説明する機能[Windows サービスでの ASP.NET Core アプリ ホスト](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)です。

## <a name="instructions"></a>手順

サンプル アプリは単純な MVC web アプリの説明に従って変更[Windows サービスでの ASP.NET Core アプリ ホスト](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)です。

でサービスを、アプリを実行するには、次の手順を実行します。

1. フォルダーを作成*c:\svc*です。

1. アプリのフォルダーを公開する`dotnet publish --configuration Release --output c:\\svc`です。 コマンドは、アプリのアセット フォルダーを移動など、必要な`appsettings.json`ファイルおよび`wwwroot`フォルダとその中身です。

1. 開く、**管理者**コマンド シェル。

1. 次のコマンドを実行します。

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. ブラウザーに移動`http://localhost:5000`をサービスが実行されていることを確認します。

1. サービスを停止するには、コマンドを使用します。

   ```console
   sc stop MyService
   ```

エラー メッセージにアクセスできるようにする簡単な方法がなど、ログ プロバイダーを追加するには、アプリが、サービスで実行する場合を想定どおりに起動しない場合、 [Windows イベント ログ プロバイダー](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog)です。 別のオプションでは、システム上のイベント ビューアーを使用して、アプリケーション イベント ログを確認します。 たとえば、FileNotFound エラー アプリケーション イベント ログ内の未処理の例外を次に示します。

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
