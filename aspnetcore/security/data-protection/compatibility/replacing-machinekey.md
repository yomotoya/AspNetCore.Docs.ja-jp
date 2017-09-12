---
title: "ASP.NET で '< machineKey >' を置き換える"
author: rick-anderson
description: "ASP.NET で '< machineKey >' を置き換える"
keywords: "ASP.NET Core、セキュリティ、'< machineKey >'"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5ac13589-3837-4b4d-8abe-81f843942120
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 29229b9507ece6aff8278b0ad66169c9e4e7498b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="replacing-machinekey-in-aspnet"></a>置き換える`<machineKey>`asp.net

<a name=compatibility-replacing-machinekey></a>

実装、 `<machineKey>` ASP.NET 内の要素[交換](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)です。 これにより、ほとんどの呼び出しを ASP.NET 暗号化ルーチン、新しいデータ保護システムを含む、交換用のデータ保護メカニズムを経由してルーティングされます。

## <a name="package-installation"></a>パッケージのインストール

> [!NOTE]
> 新しいデータ保護システムには、.NET 4.5.1 を対象とする既存の ASP.NET アプリケーションにインストールされている以上のみを指定できます。 インストールをアプリケーションが .NET 4.5 を対象とする場合は失敗または削減します。

既存の ASP.NET 4.5.1+ プロジェクトに新しいデータ保護システムをインストールするには、Microsoft.AspNetCore.DataProtection.SystemWeb パッケージをインストールします。 データ保護システムを使用して、これがインスタンス化され、[既定の構成](../configuration/default-settings.md#data-protection-default-settings)設定します。

行が挿入、パッケージをインストールするときに*Web.config*のために使用する ASP.NET に指示[暗号化操作を最も](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)(フォーム認証、ビューステートへの呼び出しなど)MachineKey.Protect です。 挿入される行は次のとおりです。

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> かどうか、新しいデータ保護システムはアクティブのようにフィールドを調べることによってわかります`__VIEWSTATE`、次の例のように"CfDJ8"から始まる必要があります。 "CfDJ8"は、データ保護システムによって保護されているペイロードを識別するマジック"09 F0 C9 F0"ヘッダーの base64 表現です。

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a>パッケージの構成

データ保護システムは既定の 0 のセットアップの構成でインスタンス化されます。 ただし、既定では、キーは、ローカル ファイル システムに永続化、ために、ファームに配置するアプリケーションにとってこの機能しません。 これを解決するには、型にどのサブクラス DataProtectionStartup を作成して構成を指定でき、その ConfigureServices メソッドをオーバーライドします。

キーが永続化し、残りの部分で暗号化する方法の両方を構成するカスタム データ保護のスタートアップの種類の例を次に示します。 独自のアプリケーション名を提供することで、既定のアプリの分離ポリシーをオーバーライドします。

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
> 使用することも`<machineKey applicationName="my-app" ... />`SetApplicationName を明示的に呼び出す代わりにします。 これは、開発者はアプリケーション名の設定を構成したいすべてが場合 DataProtectionStartup 派生型を作成しないようにする便利なメカニズムです。

このカスタム構成を有効にする Web.config に戻ってを探して、 `<appSettings>` config ファイルに追加されたパッケージをインストールする要素。 次のマークアップのようになります。

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

作成した DataProtectionStartup から派生した型のアセンブリ修飾名を空白の値を入力します。 ようになりますこのアプリケーションの名前が DataProtectionDemo の場合はの下。

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

新しく構成されたデータ保護システムは、アプリケーション内部で使用する準備ができました。
