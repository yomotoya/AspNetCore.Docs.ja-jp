---
title: "Nano Server での ASP.NET Core"
author: shirhatti
description: "IIS を実行する Nano Server インスタンスに既存の ASP.NET Core アプリを展開する方法について説明します。"
manager: wpickett
ms.author: riande
ms.date: 11/04/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/nano-server
ms.openlocfilehash: 4fc5f6874f86130da9f66d13778516d984ff8b46
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a><span data-ttu-id="6423a-103">Nano Server の ASP.NET Core と IIS</span><span class="sxs-lookup"><span data-stu-id="6423a-103">ASP.NET Core with IIS on Nano Server</span></span>

<span data-ttu-id="6423a-104">[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿</span><span class="sxs-lookup"><span data-stu-id="6423a-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="6423a-105">このチュートリアルでは、IIS を実行する Nano Server インスタンスに既存の ASP.NET Core アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="6423a-105">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="6423a-106">はじめに</span><span class="sxs-lookup"><span data-stu-id="6423a-106">Introduction</span></span>

<span data-ttu-id="6423a-107">Nano Server は Windows Server 2016 のインストール オプションです。フットプリントが小さく、セキュリティに優れています。また、Server Core やフル サーバーに比べてサービス提供機能に優れています。</span><span class="sxs-lookup"><span data-stu-id="6423a-107">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="6423a-108">詳細については、公式の [Nano Server 文書](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server)をご覧ください。180 日間の評価版をダウンロードできるリンクも記載されています。</span><span class="sxs-lookup"><span data-stu-id="6423a-108">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="6423a-109">Nano Server は 3 つの簡単な方法でお試しいただけます。</span><span class="sxs-lookup"><span data-stu-id="6423a-109">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="6423a-110">MS アカウントでサインインするとき:</span><span class="sxs-lookup"><span data-stu-id="6423a-110">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="6423a-111">Windows Server 2016 ISO ファイルをダウンロードし、Nano Server イメージを構築できます。</span><span class="sxs-lookup"><span data-stu-id="6423a-111">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="6423a-112">Nano Server VHD をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="6423a-112">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="6423a-113">Azure Gallery の Nano Server イメージを利用し、Azure で VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="6423a-113">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="6423a-114">Azure の無料試用版を利用できます。</span><span class="sxs-lookup"><span data-stu-id="6423a-114">A free trial of Azure is avaiable.</span></span>

<span data-ttu-id="6423a-115">このチュートリアルでは、2 番目の選択肢を利用します。Windows Server 2016 の構築済みの Nano Server VHD です。</span><span class="sxs-lookup"><span data-stu-id="6423a-115">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="6423a-116">このチュートリアルを進める前に、既存の ASP.NET Core アプリケーションの[発行された出力](xref:host-and-deploy/directory-structure)が必要になります。</span><span class="sxs-lookup"><span data-stu-id="6423a-116">Before proceeding with this tutorial, you will need the [published output](xref:host-and-deploy/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="6423a-117">アプリケーションが **64 ビット** プロセスで実行されるように構築されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6423a-117">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="6423a-118">Nano Server インスタンスの設定</span><span class="sxs-lookup"><span data-stu-id="6423a-118">Setting up the Nano Server instance</span></span>

<span data-ttu-id="6423a-119">以前にダウンロードした VHD を利用し、開発コンピューターで、[Hyper-V を利用して新しい仮想マシンを作成します](https://technet.microsoft.com/library/hh846766.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6423a-119">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="6423a-120">このコンピューターでは、ログオンする前に管理者パスワードを設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6423a-120">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="6423a-121">VM コンソールで、F11 を押し、最初のログイン前にパスワードを設定します。</span><span class="sxs-lookup"><span data-stu-id="6423a-121">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="6423a-122">また、新しい VM の IP アドレスを確認する必要があります。VM のプロビジョニング時に指定した DHCP サーバーの固定 IP を確認するか、Nano Server 回復コンソールのネットワーキング設定を確認します。</span><span class="sxs-lookup"><span data-stu-id="6423a-122">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="6423a-123">新しい VM のローカル V4 IP アドレスが 192.168.1.10 であると想定しましょう。</span><span class="sxs-lookup"><span data-stu-id="6423a-123">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="6423a-124">これで PowerShell リモート処理を利用して管理できるようになりました。PowerShell リモート処理は Nano Server を完全管理する唯一の方法です。</span><span class="sxs-lookup"><span data-stu-id="6423a-124">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="6423a-125">PowerShell リモート処理を利用し、Nano Server インスタンスに接続する</span><span class="sxs-lookup"><span data-stu-id="6423a-125">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="6423a-126">管理者特権で PowerShell ウィンドウを開き、リモート Nano Server インスタンスを `TrustedHosts` 一覧に追加します。</span><span class="sxs-lookup"><span data-stu-id="6423a-126">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="6423a-127">変数 `$nanoServerIpAddress` を正しい IP アドレスに変更します。</span><span class="sxs-lookup"><span data-stu-id="6423a-127">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="6423a-128">Nano Server インスタンスを `TrustedHosts` に追加したら、PowerShell リモート処理を利用して接続できます。</span><span class="sxs-lookup"><span data-stu-id="6423a-128">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="6423a-129">接続に成功すると、プロンプトに次のような形式で表示されます。`[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span><span class="sxs-lookup"><span data-stu-id="6423a-129">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="6423a-130">ファイル共有を作成する</span><span class="sxs-lookup"><span data-stu-id="6423a-130">Creating a file share</span></span>

<span data-ttu-id="6423a-131">公開されたアプリケーションをコピーできるように、Nano Server でファイル共有を作成します。</span><span class="sxs-lookup"><span data-stu-id="6423a-131">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="6423a-132">リモート セッションで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6423a-132">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="6423a-133">上記のコマンドを実行して、ホスト コンピューターの Windows Explorer で `\\192.168.1.10\AspNetCoreSampleForNano` にアクセスすると、この共有にアクセスできるはずです。</span><span class="sxs-lookup"><span data-stu-id="6423a-133">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="6423a-134">ファイアウォールでポートを開く</span><span class="sxs-lookup"><span data-stu-id="6423a-134">Open port in the firewall</span></span>

<span data-ttu-id="6423a-135">リモート セッションで次のコマンドを実行してファイアウォールでポートを開き、IIS がポート TCP/8000 で TCP トラフィックを待ち受けるようにします。</span><span class="sxs-lookup"><span data-stu-id="6423a-135">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="6423a-136">IIS のインストール</span><span class="sxs-lookup"><span data-stu-id="6423a-136">Installing IIS</span></span>

<span data-ttu-id="6423a-137">PowerShell ギャラリーから `NanoServerPackage` プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="6423a-137">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="6423a-138">プロバイダーがインストールされ、インポートされたら、Windows パッケージをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="6423a-138">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="6423a-139">先に作成した PowerShell セッションで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6423a-139">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="6423a-140">IIS が正しく設定されていることを簡単に確認するには、URL `http://192.168.1.10/` にアクセスします。ようこそページが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="6423a-140">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="6423a-141">IIS がインストールされると、ポート 80 で待ち受ける `Default Web Site` という Web サイトが既定で作成されます。</span><span class="sxs-lookup"><span data-stu-id="6423a-141">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="installing-the-aspnet-core-module-ancm"></a><span data-ttu-id="6423a-142">ASP.NET Core Module (ANCM) のインストール</span><span class="sxs-lookup"><span data-stu-id="6423a-142">Installing the ASP.NET Core Module (ANCM)</span></span>

<span data-ttu-id="6423a-143">ASP.NET Core Module は IIS 7.5+ モジュールです。ASP.NET Core HTTP リスナーのプロセス管理を担当し、管理対象のプロセスに要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="6423a-143">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="6423a-144">現時点では、ASP.NET Core Module for IIS は手動でインストールします。</span><span class="sxs-lookup"><span data-stu-id="6423a-144">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="6423a-145">(Nano ではない) 通常のコンピューターに [.NET Core Windows Server Hosting バンドル](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe)をインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6423a-145">You will need to install the [.NET Core Windows Server Hosting bundle](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) on a regular (not Nano) machine.</span></span> <span data-ttu-id="6423a-146">通常のコンピューターにバンドルをインストールしたら、先に作成したファイル共有に次のファイルをコピーする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6423a-146">After installing the bundle on a regular machine, you will need to copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="6423a-147">(Nano ではない) 通常のサーバーで、次のコピー コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6423a-147">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="6423a-148">Windows 10 コンピューターで `C:\windows\system32\inetsrv` を `C:\Program Files\IIS Express` に変更します。</span><span class="sxs-lookup"><span data-stu-id="6423a-148">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="6423a-149">Nano 側で、先に作成したファイル共有から有効な場所に次のファイルをコピーする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6423a-149">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="6423a-150">そこで、次のコピー コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6423a-150">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="6423a-151">リモート セッションで次のスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="6423a-151">Run the following script in the remote session:</span></span>

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> <span data-ttu-id="6423a-152">上記の手順の後、*aspnetcore.dll* ファイルと *aspnetcore_schema.xml* ファイルを共有から削除します。</span><span class="sxs-lookup"><span data-stu-id="6423a-152">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="6423a-153">.NET Core Framework のインストール</span><span class="sxs-lookup"><span data-stu-id="6423a-153">Installing .NET Core Framework</span></span>

<span data-ttu-id="6423a-154">アプリが[フレームワークに依存する展開 (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd) として発行される場合、.NET Core をサーバーにインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6423a-154">If your app is published as a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core must be installed on the server.</span></span> <span data-ttu-id="6423a-155">リモート PowerShell セッションで [dotnet-install.ps1 PowerShell スクリプト](https://dot.net/v1/dotnet-install.ps1)を使用し、Nano Server に .NET Core をインストールします。</span><span class="sxs-lookup"><span data-stu-id="6423a-155">Use the [dotnet-install.ps1 PowerShell script](https://dot.net/v1/dotnet-install.ps1) in a remote PowerShell session to install .NET Core on your Nano Server.</span></span> <span data-ttu-id="6423a-156">CLI バージョンを `-Version` スイッチで渡します。</span><span class="sxs-lookup"><span data-stu-id="6423a-156">Pass the CLI version with the `-Version` switch:</span></span>

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a><span data-ttu-id="6423a-157">アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="6423a-157">Publishing the application</span></span>

<span data-ttu-id="6423a-158">既存アプリケーションの公開した出力をファイル共有のルートにコピーします。</span><span class="sxs-lookup"><span data-stu-id="6423a-158">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="6423a-159">*dotnet.exe* を抽出した場所を指すように *web.config* を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6423a-159">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="6423a-160">あるいは、パスに *dotnet.exe* を追加できます。</span><span class="sxs-lookup"><span data-stu-id="6423a-160">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="6423a-161">*dotnet.exe* をパスに**置かない**場合、*web.config* は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="6423a-161">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="6423a-162">リモート セッションで次のコマンドを実行し、既定の Web サイトとは異なるポートで、公開したアプリの新しいサイトを IIS で作成します。</span><span class="sxs-lookup"><span data-stu-id="6423a-162">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="6423a-163">また、Web にアクセスするためにそのポートを開く必要があります。</span><span class="sxs-lookup"><span data-stu-id="6423a-163">You also need to open that port to access the web.</span></span> <span data-ttu-id="6423a-164">このスクリプトでは `DefaultAppPool` を利用し、わかりやすくしています。</span><span class="sxs-lookup"><span data-stu-id="6423a-164">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="6423a-165">アプリケーション プールの下で実行する場合の考慮事項については、「[アプリケーション プール](xref:host-and-deploy/iis/index#application-pools)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6423a-165">For more considerations on running under an application pool, see [Application Pools](xref:host-and-deploy/iis/index#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="6423a-166">Nano Server で .NET Core CLI を実行したときに発生する既知の問題と回避策</span><span class="sxs-lookup"><span data-stu-id="6423a-166">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="6423a-167">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="6423a-167">Running the application</span></span>

<span data-ttu-id="6423a-168">公開された Web アプリにはブラウザーでアクセスできます (`http://192.168.1.10:8000`)。</span><span class="sxs-lookup"><span data-stu-id="6423a-168">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="6423a-169">「[Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)」 (ログの作成とリダイレクト) の説明に基づいてログを設定している場合、ログを *C:\PublishedApps\AspNetCoreSampleForNano\logs* で閲覧できます。</span><span class="sxs-lookup"><span data-stu-id="6423a-169">If you've set up logging as described in [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>
