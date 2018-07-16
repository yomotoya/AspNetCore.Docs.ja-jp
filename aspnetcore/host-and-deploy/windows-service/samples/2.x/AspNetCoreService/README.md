# <a name="custom-webhost-service-sample"></a><span data-ttu-id="12cd9-101">カスタム WebHost サービスのサンプル</span><span class="sxs-lookup"><span data-stu-id="12cd9-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="12cd9-102">このサンプルは、IIS を使用せずに、Windows サービスとして ASP.NET Core アプリをホストする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="12cd9-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="12cd9-103">このサンプルでは、「[Windows サービスでの ASP.NET Core のホスト](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)」で説明されているシナリオを紹介します。</span><span class="sxs-lookup"><span data-stu-id="12cd9-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="12cd9-104">手順</span><span class="sxs-lookup"><span data-stu-id="12cd9-104">Instructions</span></span>

<span data-ttu-id="12cd9-105">このサンプル アプリは、「[Windows サービスでの ASP.NET Core のホスト](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)」の指示に従って変更した Razor Pages Web アプリです。</span><span class="sxs-lookup"><span data-stu-id="12cd9-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="12cd9-106">サービスでこのアプリを実行するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="12cd9-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="12cd9-107">*c:\svc* にフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="12cd9-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="12cd9-108">`dotnet publish --configuration Release --output c:\\svc` を使用してフォルダーにアプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="12cd9-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="12cd9-109">このコマンドで、必要な `appsettings.json` ファイルと `wwwroot` フォルダーを含め、アプリの資産が *svc* フォルダーに移動されます。</span><span class="sxs-lookup"><span data-stu-id="12cd9-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="12cd9-110">**管理者**のコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="12cd9-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="12cd9-111">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="12cd9-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="12cd9-112">*等号記号 (=) とパス文字列の先頭の間にはスペースが必要です。*</span><span class="sxs-lookup"><span data-stu-id="12cd9-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="12cd9-113">ブラウザーで `http://localhost:5000` に移動し、サービスが実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="12cd9-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="12cd9-114">アプリがセキュリティで保護されたエンドポイント `https://localhost:5001` にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="12cd9-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="12cd9-115">サービスを停止するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="12cd9-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="12cd9-116">アプリが正常に起動しない場合は、[Windows EventLog プロバイダー](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog)などのロギング プロバイダーを追加すると、エラー メッセージに簡単にアクセスできるようになります。</span><span class="sxs-lookup"><span data-stu-id="12cd9-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="12cd9-117">また、システムでイベント ビューアーを使用してアプリケーション イベント ログを確認する方法もあります。</span><span class="sxs-lookup"><span data-stu-id="12cd9-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="12cd9-118">たとえば、アプリケーション イベント ログには次のような FileNotFound エラーのハンドルされない例外があります。</span><span class="sxs-lookup"><span data-stu-id="12cd9-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
