---
title: Web API アナライザーを使用する
author: pranavkm
description: Microsoft.AspNetCore.Mvc.Api.Analyzers の Web API アナライザーについて説明します。
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 89424d89ec2b3125fd3c6b7c86fed2d292b153e6
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635390"
---
# <a name="use-web-api-analyzers"></a>Web API アナライザーを使用する

ASP.NET Core 2.2 で、Web API のアナライザーが含まれる [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet パッケージが導入されています。 アナライザーは、[API 規約](xref:web-api/advanced/conventions)の設定中に <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> の注釈が付けられたコントローラーで動作します。

## <a name="package-installation"></a>パッケージ インストール

`Microsoft.AspNetCore.Mvc.Api.Analyzers` は、次のいずれかの方法で追加できます。

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **[パッケージ マネージャー コンソール]** ウィンドウから:
  * **[ビュー]** > **[Other Windows]** \(その他の Windows\) > **[パッケージ マネージャー コンソール]** に移動します。
  * *ApiConventions.csproj* ファイルが存在するディレクトリに移動します。
  * 次のコマンドを実行します。

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* **[NuGet パッケージの管理]** ダイアログ ボックスから:
  * **[ソリューション エクスプローラー]** > **[NuGet パッケージの管理]** でプロジェクトを右クリックします。
  * **パッケージ ソース**を "nuget.org" に設定します。
  * 検索ボックスに「Microsoft.AspNetCore.Mvc.Api.Analyzers」と入力します。
  * **[参照]** タブから "Microsoft.AspNetCore.Mvc.Api.Analyzers" パッケージを選択して、**[インストール]** をクリックします。

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* **[Solution Pad]** > **[パッケージを追加...]** で [*パッケージ*] フォルダーを右クリックします。
* **[パッケージを追加]** ウィンドウの **[ソース]** ドロップダウンを "nuget.org" に設定します。
* 検索ボックスに「Microsoft.AspNetCore.Mvc.Api.Analyzers」と入力します。
* 結果ウィンドウから "Microsoft.AspNetCore.Mvc.Api.Analyzers" パッケージを選択して、**[パッケージを追加]** をクリックします。

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

**統合端末**からから次のコマンドを実行します。

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

次のコマンドを実行します。

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a>API 規約のアナライザー

Open API ドキュメントには、アクションによって返される可能性のある状態コードと応答の種類が含まれます。 ASP.NET Core MVC では、<xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> や <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> などの属性がアクションの文書化に使用されます。 <xref:tutorials/web-api-help-pages-using-swagger> では、API の文書化についてさらに詳しく説明しています。

パッケージのいずれかのアナライザーが、<xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> の注釈が付けられたコントローラーを検査し、応答全体を文書化していないアクションを特定します。 次に例を示します。

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

前のアクションでは、HTTP 200 の成功の戻り値の型は文書化していますが、HTTP 404 のエラー状態コードは文書化していません。 アナライザーは、HTTP 404 状態コードの文書化の不備を警告として報告します。 問題を解決するためのオプションが提供されます。
