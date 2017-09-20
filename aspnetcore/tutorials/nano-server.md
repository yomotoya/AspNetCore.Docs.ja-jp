---
title: "Nano Server での ASP.NET Core"
author: shirhatti
description: "IIS を実行する Nano Server インスタンスに既存の ASP.NET Core アプリを展開する方法について説明します。"
keywords: "ASP.NET Core、nano server"
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.assetid: 50922cf1-ca58-4006-9236-99b7ff2dd0cf
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: dd1f2c8de58ea8d3a57e64ecc519184400cb52c8
ms.sourcegitcommit: ad01283f299d346cf757c4f4744c48634dc27e73
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2017
---
# <a name="aspnet-core-with-iis-on-nano-server"></a>Nano Server の ASP.NET Core と IIS

[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿

このチュートリアルでは、IIS を実行する Nano Server インスタンスに既存の ASP.NET Core アプリを展開します。

## <a name="introduction"></a>はじめに

Nano Server は Windows Server 2016 のインストール オプションです。フットプリントが小さく、セキュリティに優れています。また、Server Core やフル サーバーに比べてサービス提供機能に優れています。 詳細については、公式の [Nano Server 文書](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server)をご覧ください。180 日間の評価版をダウンロードできるリンクも記載されています。 

Nano Server は 3 つの簡単な方法でお試しいただけます。 MS アカウントでサインインするとき:

1. Windows Server 2016 ISO ファイルをダウンロードし、Nano Server イメージを構築できます。

2. Nano Server VHD をダウンロードします。

3. Azure Gallery の Nano Server イメージを利用し、Azure で VM を作成します。 Azure アカウントを持っていない場合、30 日間無料でお試しいただけます。

このチュートリアルでは、2 番目の選択肢を利用します。Windows Server 2016 の構築済みの Nano Server VHD です。

このチュートリアルを進める前に、既存の ASP.NET Core アプリケーションの[発行された出力](xref:hosting/directory-structure)が必要になります。 アプリケーションが **64 ビット** プロセスで実行されるように構築されていることを確認します。

## <a name="setting-up-the-nano-server-instance"></a>Nano Server インスタンスの設定

以前にダウンロードした VHD を利用し、開発コンピューターで、[Hyper-V を利用して新しい仮想マシンを作成します](https://technet.microsoft.com/library/hh846766.aspx)。 このコンピューターでは、ログオンする前に管理者パスワードを設定する必要があります。 VM コンソールで、F11 を押し、最初のログイン前にパスワードを設定します。 また、新しい VM の IP アドレスを確認する必要があります。VM のプロビジョニング時に指定した DHCP サーバーの固定 IP を確認するか、Nano Server 回復コンソールのネットワーキング設定を確認します。

> [!NOTE]
> 新しい VM のローカル V4 IP アドレスが 192.168.1.10 であると想定しましょう。

これで PowerShell リモート処理を利用して管理できるようになりました。PowerShell リモート処理は Nano Server を完全管理する唯一の方法です。

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a>PowerShell リモート処理を利用し、Nano Server インスタンスに接続する

管理者特権で PowerShell ウィンドウを開き、リモート Nano Server インスタンスを `TrustedHosts` 一覧に追加します。

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> 変数 `$nanoServerIpAddress` を正しい IP アドレスに変更します。

Nano Server インスタンスを `TrustedHosts` に追加したら、PowerShell リモート処理を利用して接続できます。

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

接続に成功すると、プロンプトに次のような形式で表示されます。`[192.168.1.10]: PS C:\Users\Administrator\Documents>`

## <a name="creating-a-file-share"></a>ファイル共有を作成する

公開されたアプリケーションをコピーできるように、Nano Server でファイル共有を作成します。 リモート セッションで次のコマンドを実行します。

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

上記のコマンドを実行して、ホスト コンピューターの Windows Explorer で `\\192.168.1.10\AspNetCoreSampleForNano` にアクセスすると、この共有にアクセスできるはずです。

## <a name="open-port-in-the-firewall"></a>ファイアウォールでポートを開く

リモート セッションで次のコマンドを実行してファイアウォールでポートを開き、IIS がポート TCP/8000 で TCP トラフィックを待ち受けるようにします。

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a>IIS のインストール

PowerShell ギャラリーから `NanoServerPackage` プロバイダーを追加します。 プロバイダーがインストールされ、インポートされたら、Windows パッケージをインストールできます。

先に作成した PowerShell セッションで次のコマンドを実行します。

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

IIS が正しく設定されていることを簡単に確認するには、URL `http://192.168.1.10/` にアクセスします。ようこそページが表示されるはずです。 IIS がインストールされると、ポート 80 で待ち受ける `Default Web Site` という Web サイトが既定で作成されます。

## <a name="installing-the-aspnet-core-module-ancm"></a>ASP.NET Core Module (ANCM) のインストール

ASP.NET Core Module は IIS 7.5+ モジュールです。ASP.NET Core HTTP リスナーのプロセス管理を担当し、管理対象のプロセスに要求を送信します。 現時点では、ASP.NET Core Module for IIS は手動でインストールします。 (Nano ではない) 通常のコンピューターに [.NET Core Windows Server Hosting バンドル](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe)をインストールする必要があります。 通常のコンピューターにバンドルをインストールしたら、先に作成したファイル共有に次のファイルをコピーする必要があります。

(Nano ではない) 通常のサーバーで、次のコピー コマンドを実行します。

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

Windows 10 コンピューターで `C:\windows\system32\inetsrv` を `C:\Program Files\IIS Express` に変更します。

Nano 側で、先に作成したファイル共有から有効な場所に次のファイルをコピーする必要があります。 そこで、次のコピー コマンドを実行します。

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

リモート セッションで次のスクリプトを実行します。

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
> 上記の手順の後、*aspnetcore.dll* ファイルと *aspnetcore_schema.xml* ファイルを共有から削除します。

## <a name="installing-net-core-framework"></a>.NET Core Framework のインストール

アプリが[フレームワークに依存する展開 (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd) として発行される場合、.NET Core をサーバーにインストールする必要があります。 リモート PowerShell セッションで [dotnet-install.ps1 PowerShell スクリプト](https://dot.net/v1/dotnet-install.ps1)を使用し、Nano Server に .NET Core をインストールします。 CLI バージョンを `-Version` スイッチで渡します。

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a>アプリケーションの発行

既存アプリケーションの公開した出力をファイル共有のルートにコピーします。

*dotnet.exe* を抽出した場所を指すように *web.config* を変更する必要があります。 あるいは、パスに *dotnet.exe* を追加できます。

*dotnet.exe* をパスに**置かない**場合、*web.config* は次のようになります。

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

リモート セッションで次のコマンドを実行し、既定の Web サイトとは異なるポートで、公開したアプリの新しいサイトを IIS で作成します。 また、Web にアクセスするためにそのポートを開く必要があります。 このスクリプトでは `DefaultAppPool` を利用し、わかりやすくしています。 アプリケーション プールの下で実行する場合の考慮事項については、「[アプリケーション プール](xref:publishing/iis#application-pools)」を参照してください。

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a>Nano Server で .NET Core CLI を実行したときに発生する既知の問題と回避策
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a>アプリケーションの実行

公開された Web アプリにはブラウザーでアクセスできます (`http://192.168.1.10:8000`)。 「[Log creation and redirection](xref:hosting/aspnet-core-module#log-creation-and-redirection)」 (ログの作成とリダイレクト) の説明に基づいてログを設定している場合、ログを *C:\PublishedApps\AspNetCoreSampleForNano\logs* で閲覧できます。
