# <a name="custom-webhost-service-sample"></a><span data-ttu-id="01f96-101">カスタム web ホスト サービスのサンプル</span><span class="sxs-lookup"><span data-stu-id="01f96-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="01f96-102">このサンプルでは、Windows サービスとして IIS を使用せずに Windows 上の ASP.NET Core アプリケーションをホストするための推奨方法を示します。</span><span class="sxs-lookup"><span data-stu-id="01f96-102">This sample shows the recommended way to host an ASP.NET Core app on Windows without using IIS as a Windows Service.</span></span> <span data-ttu-id="01f96-103">このサンプルで説明する機能[Windows サービスでの ASP.NET Core アプリ ホスト](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)です。</span><span class="sxs-lookup"><span data-stu-id="01f96-103">This sample demonstrates the features described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="01f96-104">手順</span><span class="sxs-lookup"><span data-stu-id="01f96-104">Instructions</span></span>

<span data-ttu-id="01f96-105">サンプル アプリは単純な MVC web アプリの説明に従って変更[Windows サービスでの ASP.NET Core アプリ ホスト](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)です。</span><span class="sxs-lookup"><span data-stu-id="01f96-105">The sample app is a simple MVC web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="01f96-106">でサービスを、アプリを実行するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="01f96-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="01f96-107">フォルダーを作成*c:\svc*です。</span><span class="sxs-lookup"><span data-stu-id="01f96-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="01f96-108">アプリのフォルダーを公開する`dotnet publish --configuration Release --output c:\\svc`です。</span><span class="sxs-lookup"><span data-stu-id="01f96-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="01f96-109">コマンドは、アプリのアセット フォルダーを移動など、必要な`appsettings.json`ファイルおよび`wwwroot`フォルダとその中身です。</span><span class="sxs-lookup"><span data-stu-id="01f96-109">The command will move the app's assets to the folder, including the required `appsettings.json` file and the `wwwroot` folder with its contents.</span></span>

1. <span data-ttu-id="01f96-110">開く、**管理者**コマンド シェル。</span><span class="sxs-lookup"><span data-stu-id="01f96-110">Open an **administrator** command shell.</span></span>

1. <span data-ttu-id="01f96-111">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="01f96-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. <span data-ttu-id="01f96-112">ブラウザーに移動`http://localhost:5000`をサービスが実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="01f96-112">In a browser, go to `http://localhost:5000` to verify that the service is running.</span></span>

1. <span data-ttu-id="01f96-113">サービスを停止するには、コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="01f96-113">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="01f96-114">エラー メッセージにアクセスできるようにする簡単な方法がなど、ログ プロバイダーを追加するには、アプリが、サービスで実行する場合を想定どおりに起動しない場合、 [Windows イベント ログ プロバイダー](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog)です。</span><span class="sxs-lookup"><span data-stu-id="01f96-114">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="01f96-115">別のオプションでは、システム上のイベント ビューアーを使用して、アプリケーション イベント ログを確認します。</span><span class="sxs-lookup"><span data-stu-id="01f96-115">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="01f96-116">たとえば、FileNotFound エラー アプリケーション イベント ログ内の未処理の例外を次に示します。</span><span class="sxs-lookup"><span data-stu-id="01f96-116">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
