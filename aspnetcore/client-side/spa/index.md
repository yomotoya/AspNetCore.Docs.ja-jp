---
title: ASP.NET Core での単一ページ アプリケーション テンプレートの使用
author: SteveSandersonMS
description: ASP.NET Core の単一ページ アプリケーション (SPA) プロジェクト テンプレートをインストールして、作業を開始する方法を説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/index
ms.openlocfilehash: ab164ae5d2df47739ec04b32cd21835ffdf9f87f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291445"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>ASP.NET Core での単一ページ アプリケーション テンプレートの使用

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> リリースされた .NET Core 2.0.x SDK には、Angular、React、および Redux と組み合わせた React 向けの古いプロジェクト テンプレートが含まれています。 これは、これらの古いプロジェクト テンプレートに関するドキュメントではありません。 これは、最新の Angular、React、Redux と組み合わせた React 向けのテンプレートについてのドキュメントです。これらのテンプレートは、手動で ASP.NET Core 2.0 にインストールできます。 テンプレートは既定で ASP.NET Core 2.1 に含まれています。

::: moniker-end

## <a name="prerequisites"></a>必須コンポーネント

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org) (バージョン 6 以降)

## <a name="installation"></a>インストール

::: moniker range=">= aspnetcore-2.1"

ASP.NET Core 2.1 には、テンプレートが既にインストールされています。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ASP.NET Core 2.0 を使用している場合は、次のコマンドを実行して、Angular、React、Redux と組み合わせた React 向けの、更新された ASP.NET Core テンプレートをインストールします。

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a>テンプレートの使用

* [Angular プロジェクト テンプレートの使用](xref:spa/angular)
* [React プロジェクト テンプレートの使用](xref:spa/react)
* [Redux と組み合わせた React プロジェクト テンプレートの使用](xref:spa/react-with-redux)
