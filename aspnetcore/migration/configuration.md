---
title: ASP.NET Core 構成を移行します。
author: ardalis
description: ASP.NET MVC プロジェクトから ASP.NET Core MVC プロジェクトに構成を移行する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: 5a1c4d0cbbdf74a00073c654e78a05f44948caae
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205913"
---
# <a name="migrate-configuration-to-aspnet-core"></a>ASP.NET Core 構成を移行します。

作成者: [Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com)

前の記事では[ASP.NET MVCプロジェクトからASP.NET Core MVCへの移行](xref:migration/mvc)を開始しました。 この記事では、構成を移行します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

## <a name="setup-configuration"></a>セットアップの構成

ASP.NET Core を使用しなく、 *Global.asax*と*web.config*ファイルを以前のバージョンの ASP.NET を使用します。 以前のバージョンの ASP.NET では、アプリケーションの起動ロジックが格納された、`Application_StartUp`メソッド内で*Global.asax*します。 ASP.NET MVC では、以降、 *Startup.cs* ; プロジェクトのルートにファイルが含まれているし、アプリケーションの起動時に呼び出されました。 ASP.NET Core はすべてのスタートアップ ロジックを配置することでこの方法を完全に採用が、 *Startup.cs*ファイル。

*Web.config*ファイルは ASP.NET Core で置き換えられてもします。 説明されているアプリケーションのスタートアップ プロシージャの一部として、構成自体を構成ようになりましたことができます*Startup.cs*します。 構成は、XML ファイルにも引き続き使用できますが、通常 ASP.NET Core プロジェクトが配置の構成値で JSON 形式のファイルなど*appsettings.json*します。 ASP.NET Core の構成システムは、環境変数は、指定できますも簡単にアクセスできます、[よりセキュリティで保護された信頼性の高い場所](xref:security/app-secrets)環境固有の値。 これは、接続文字列とソース管理にチェックインしないでください API キーのような機密情報に特に当てはまります。 参照してください[構成](xref:fundamentals/configuration/index)を ASP.NET Core での構成の詳細を参照してください。

この記事では、部分的に移行した ASP.NET Core プロジェクトで開始します[前の記事](xref:migration/mvc)します。 構成のセットアップ、次のコンス トラクターとプロパティを追加する、 *Startup.cs*プロジェクトのルートにあるファイル。

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

現時点ことに注意してください、 *Startup.cs*ように、次を追加する必要があります、ファイルはコンパイルされません`using`ステートメント。

```csharp
using Microsoft.Extensions.Configuration;
```

追加、 *appsettings.json*適切な項目テンプレートを使用して、プロジェクトのルートにファイル。

![AppSettings の JSON を追加します。](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Web.config ファイルから構成設定を移行します。

ASP.NET MVC プロジェクトに必要なデータベース接続文字列が含まれている*web.config*の`<connectionStrings>`要素。 この ASP.NET Core プロジェクトでこの情報を格納するつもりが、 *appsettings.json*ファイル。 開いている*appsettings.json*、および次既に含まれていることに注意してください。

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

強調表示された行を上に示したでデータベースの名前を変更 **_CHANGE_ME**データベースの名前にします。

## <a name="summary"></a>まとめ

ASP.NET Core では、1 つのファイル、これで、必要なサービスと依存関係を定義および構成できるアプリケーションのすべてのスタートアップ ロジックを配置します。 置き換えられます、 *web.config*ファイルのさまざまな環境変数と同様に、JSON などのファイル形式を利用できる柔軟な構成機能を使用します。
