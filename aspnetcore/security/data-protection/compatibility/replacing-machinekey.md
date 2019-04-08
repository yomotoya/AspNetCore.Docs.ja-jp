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
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a>ASP.NET Core での ASP.NET machineKey を置き換えます

<a name="compatibility-replacing-machinekey"></a>

実装、 `<machineKey>` ASP.NET 内の要素[は置き換え可能](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)します。 これにより、新しいデータ保護システムを含む、交換用のデータ保護メカニズムを通じてルーティングされるようにする ASP.NET 暗号化ルーチンにほとんどの呼び出しができます。

## <a name="package-installation"></a>パッケージ インストール

> [!NOTE]
> 新しいデータ保護システムは、.NET 4.5.1 を対象とする既存の ASP.NET アプリケーションにインストールされている以降にのみできます。 インストールが失敗する場合は、アプリケーションが .NET 4.5 を対象とまたは削減します。

既存の ASP.NET 4.5.1+ プロジェクトには、新しいデータ保護システムをインストールするには、Microsoft.AspNetCore.DataProtection.SystemWeb パッケージをインストールします。 データ保護システムを使用して、これはインスタンス化、[既定の構成](xref:security/data-protection/configuration/default-settings)設定します。

パッケージをインストールするときに行を挿入します*Web.config*ことを示すために使用する ASP.NET[暗号化操作を最も](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)(フォーム認証、ビューステートへの呼び出しなど)。MachineKey.Protect します。 挿入される行は次のとおりです。

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> などのフィールドを調べることによって、新しいデータ保護システムがアクティブなかどうかがわかる`__VIEWSTATE`、次の例のように"CfDJ8"から始まる必要があります。 "CfDJ8"は、データ保護システムによって保護されているペイロードを識別するマジック"09 F0 C9 F0"ヘッダーの base64 表現です。

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a>パッケージの構成

データ保護システムは、既定のゼロ セットアップ構成でインスタンス化されます。 ただし、既定では、キーはローカル ファイル システムに永続化、ため、ファームにデプロイされているアプリケーションのこの機能しません。 これを解決するには、一種の DataProtectionStartup サブクラスを作成して構成を指定でき、その ConfigureServices メソッドをオーバーライドします。

キーが永続化し、保存時暗号化している方法の両方を構成するカスタム データ保護のスタートアップの種類の例を次に示します。 独自のアプリケーション名を指定して、アプリの既定の分離ポリシーもオーバーライドします。

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
> 使用することも`<machineKey applicationName="my-app" ... />`SetApplicationName を明示的に呼び出す代わりにします。 これは、開発者がアプリケーション名の設定を構成すると、すべてが場合 DataProtectionStartup 派生型を作成しないようにする便利なメカニズムです。

このカスタム構成を有効にする Web.config に戻るし、探して、`<appSettings>`構成ファイルに追加するパッケージをインストールする要素。 次のマークアップのようになります。

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

作成した DataProtectionStartup から派生した型のアセンブリ修飾名を空白の値を入力します。 アプリケーションの名前が DataProtectionDemo の場合は、これのようになりますが、下。

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

新しく構成されたデータ保護システムは、アプリケーション内部で使用する準備ができました。
