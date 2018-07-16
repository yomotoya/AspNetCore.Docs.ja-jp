# <a name="custom-webhost-service-sample"></a>カスタム WebHost サービスのサンプル

このサンプルは、IIS を使用せずに、Windows サービスとして ASP.NET Core アプリをホストする方法を示しています。 このサンプルでは、「[Windows サービスでの ASP.NET Core のホスト](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)」で説明されているシナリオを紹介します。

## <a name="instructions"></a>手順

このサンプル アプリは、「[Windows サービスでの ASP.NET Core のホスト](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)」の指示に従って変更した Razor Pages Web アプリです。

サービスでこのアプリを実行するには、次の手順を実行します。

1. *c:\svc* にフォルダーを作成します。

1. `dotnet publish --configuration Release --output c:\\svc` を使用してフォルダーにアプリを発行します。 このコマンドで、必要な `appsettings.json` ファイルと `wwwroot` フォルダーを含め、アプリの資産が *svc* フォルダーに移動されます。

1. **管理者**のコマンド プロンプトを開きます。

1. 次のコマンドを実行します。

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  *等号記号 (=) とパス文字列の先頭の間にはスペースが必要です。*

1. ブラウザーで `http://localhost:5000` に移動し、サービスが実行されていることを確認します。 アプリがセキュリティで保護されたエンドポイント `https://localhost:5001` にリダイレクトされます。

1. サービスを停止するには、次のコマンドを使用します。

   ```console
   sc stop MyService
   ```

アプリが正常に起動しない場合は、[Windows EventLog プロバイダー](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog)などのロギング プロバイダーを追加すると、エラー メッセージに簡単にアクセスできるようになります。 また、システムでイベント ビューアーを使用してアプリケーション イベント ログを確認する方法もあります。 たとえば、アプリケーション イベント ログには次のような FileNotFound エラーのハンドルされない例外があります。

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
