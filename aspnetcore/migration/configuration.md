---
title: "構成を移行します。"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 8468d859-ff32-4a92-9e62-08c4a9e36594
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/configuration
ms.openlocfilehash: 62660f7e58467a69f540966df188747b6fde57fe
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-configuration"></a>構成を移行します。

によって[Steve Smith](https://ardalis.com/)と[Scott Addie](https://scottaddie.com)

開始前の記事で[ASP.NET Core MVC に ASP.NET MVC プロジェクトを移行する](mvc.md)です。 この記事では、構成を移行します。

[サンプル コードを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples)

## <a name="setup-configuration"></a>セットアップの構成

ASP.NET Core を使用しない、 *Global.asax*と*web.config* ASP.NET の以前のバージョンを使用するファイル。 ASP.NET の以前のバージョンではアプリケーションのスタートアップ ロジックに格納された、`Application_StartUp`メソッド内で*Global.asax*です。 その後、ASP.NET MVC では、 *Startup.cs*ファイルがプロジェクトのルートに含まれているし、アプリケーションの起動時に呼び出されました。 ASP.NET Core を採用していますこのアプローチ完全にすべてのスタートアップ ロジックを配置することによって、 *Startup.cs*ファイル。

*Web.config* ASP.NET Core でのファイルが置き換えられてもいます。 説明されているアプリケーションのスタートアップ プロシージャの一部として、構成自体を構成ようになりましたことができます*Startup.cs*です。 構成は、XML ファイルにも引き続き使用できますが、通常 ASP.NET Core プロジェクトが配置構成値 JSON 形式のファイルになど*される appsettings.json*です。 ASP.NET Core の構成システムは、環境変数は、環境固有の値より安全かつ堅牢な場所を指定することができますをも簡単にアクセスできます。 これは、接続文字列およびソース管理にチェックインする必要がありますいない API キーなどの機密情報に特に当てはまります。 参照してください[構成](../fundamentals/configuration.md)ASP.NET Core の構成に関する詳細についてはします。

この記事では、私たちは、開始から ASP.NET Core プロジェクトを部分的に移行[前の記事](mvc.md)です。 構成をセットアップで、次のコンス トラクターとプロパティを追加する、 *Startup.cs*プロジェクトのルートにあるファイル。

[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

この時点で、メモ、 *Startup.cs*ファイルはコンパイルされず、次を追加する必要があります、`using`ステートメント。

```csharp
using Microsoft.Extensions.Configuration;
```

追加、*される appsettings.json*適切な項目テンプレートを使用して、プロジェクトのルートにファイル。

![AppSettings の JSON を追加します。](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Web.config ファイルから構成設定を移行します。

ASP.NET MVC プロジェクトに必要なデータベース接続文字列が含まれている*web.config*で、`<connectionStrings>`要素。 プロジェクトでは、ASP.NET Core、この情報を格納するつもりが、*される appsettings.json*ファイル。 開いている*される appsettings.json*、および次既に含まれていることに注意してください。

[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


強調表示された行に上に示されているからデータベースの名前を変更する**_CHANGE_ME**データベースの名前にします。

## <a name="summary"></a>概要

ASP.NET Core は、単一のファイルを順番に必要なサービスと依存関係の定義し、構成に、アプリケーションのすべてのスタートアップ ロジックを配置します。 置き換えられます、 *web.config*ファイルのさまざまな環境変数と同様に、JSON などのファイル形式を利用できる柔軟な構成機能を使用します。
