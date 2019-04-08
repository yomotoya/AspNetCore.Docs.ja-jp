---
title: ASP.NET Core での ASP.NET machineKey を置き換えます
author: rick-anderson
description: ASP.NET には、新しいより安全なデータ保護システムを使用できるように、machineKey を交換する方法を説明します。
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: ff36382d22a218a228b42a31ae4f8ad2eb2d5b5f
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068285"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a><span data-ttu-id="3dc67-103">ASP.NET Core での ASP.NET machineKey を置き換えます</span><span class="sxs-lookup"><span data-stu-id="3dc67-103">Replace the ASP.NET machineKey in ASP.NET Core</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="3dc67-104">実装、 `<machineKey>` ASP.NET 内の要素[は置き換え可能](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)します。</span><span class="sxs-lookup"><span data-stu-id="3dc67-104">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="3dc67-105">これにより、新しいデータ保護システムを含む、交換用のデータ保護メカニズムを通じてルーティングされるようにする ASP.NET 暗号化ルーチンにほとんどの呼び出しができます。</span><span class="sxs-lookup"><span data-stu-id="3dc67-105">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="3dc67-106">パッケージ インストール</span><span class="sxs-lookup"><span data-stu-id="3dc67-106">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="3dc67-107">新しいデータ保護システムは、.NET 4.5.1 を対象とする既存の ASP.NET アプリケーションにインストールされている以降にのみできます。</span><span class="sxs-lookup"><span data-stu-id="3dc67-107">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or later.</span></span> <span data-ttu-id="3dc67-108">インストールが失敗する場合は、アプリケーションが .NET 4.5 を対象とまたは削減します。</span><span class="sxs-lookup"><span data-stu-id="3dc67-108">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="3dc67-109">既存の ASP.NET 4.5.1+ プロジェクトには、新しいデータ保護システムをインストールするには、Microsoft.AspNetCore.DataProtection.SystemWeb パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="3dc67-109">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="3dc67-110">データ保護システムを使用して、これはインスタンス化、[既定の構成](xref:security/data-protection/configuration/default-settings)設定します。</span><span class="sxs-lookup"><span data-stu-id="3dc67-110">This will instantiate the data protection system using the [default configuration](xref:security/data-protection/configuration/default-settings) settings.</span></span>

<span data-ttu-id="3dc67-111">パッケージをインストールするときに行を挿入します*Web.config*ことを示すために使用する ASP.NET[暗号化操作を最も](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)(フォーム認証、ビューステートへの呼び出しなど)。MachineKey.Protect します。</span><span class="sxs-lookup"><span data-stu-id="3dc67-111">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="3dc67-112">挿入される行は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="3dc67-112">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="3dc67-113">などのフィールドを調べることによって、新しいデータ保護システムがアクティブなかどうかがわかる`__VIEWSTATE`、次の例のように"CfDJ8"から始まる必要があります。</span><span class="sxs-lookup"><span data-stu-id="3dc67-113">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="3dc67-114">"CfDJ8"は、データ保護システムによって保護されているペイロードを識別するマジック"09 F0 C9 F0"ヘッダーの base64 表現です。</span><span class="sxs-lookup"><span data-stu-id="3dc67-114">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a><span data-ttu-id="3dc67-115">パッケージの構成</span><span class="sxs-lookup"><span data-stu-id="3dc67-115">Package configuration</span></span>

<span data-ttu-id="3dc67-116">データ保護システムは、既定のゼロ セットアップ構成でインスタンス化されます。</span><span class="sxs-lookup"><span data-stu-id="3dc67-116">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="3dc67-117">ただし、既定では、キーはローカル ファイル システムに永続化、ため、ファームにデプロイされているアプリケーションのこの機能しません。</span><span class="sxs-lookup"><span data-stu-id="3dc67-117">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="3dc67-118">これを解決するには、一種の DataProtectionStartup サブクラスを作成して構成を指定でき、その ConfigureServices メソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="3dc67-118">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="3dc67-119">キーが永続化し、保存時暗号化している方法の両方を構成するカスタム データ保護のスタートアップの種類の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3dc67-119">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="3dc67-120">独自のアプリケーション名を指定して、アプリの既定の分離ポリシーもオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="3dc67-120">It also overrides the default app isolation policy by providing its own application name.</span></span>

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> <span data-ttu-id="3dc67-121">使用することも`<machineKey applicationName="my-app" ... />`SetApplicationName を明示的に呼び出す代わりにします。</span><span class="sxs-lookup"><span data-stu-id="3dc67-121">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="3dc67-122">これは、開発者がアプリケーション名の設定を構成すると、すべてが場合 DataProtectionStartup 派生型を作成しないようにする便利なメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="3dc67-122">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="3dc67-123">このカスタム構成を有効にする Web.config に戻るし、探して、`<appSettings>`構成ファイルに追加するパッケージをインストールする要素。</span><span class="sxs-lookup"><span data-stu-id="3dc67-123">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="3dc67-124">次のマークアップのようになります。</span><span class="sxs-lookup"><span data-stu-id="3dc67-124">It will look like the following markup:</span></span>

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

<span data-ttu-id="3dc67-125">作成した DataProtectionStartup から派生した型のアセンブリ修飾名を空白の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="3dc67-125">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="3dc67-126">アプリケーションの名前が DataProtectionDemo の場合は、これのようになりますが、下。</span><span class="sxs-lookup"><span data-stu-id="3dc67-126">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="3dc67-127">新しく構成されたデータ保護システムは、アプリケーション内部で使用する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="3dc67-127">The newly-configured data protection system is now ready for use inside the application.</span></span>
