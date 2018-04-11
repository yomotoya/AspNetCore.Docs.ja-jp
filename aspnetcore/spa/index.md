---
title: ASP.NET Core での単一ページ アプリケーション テンプレートの使用
author: SteveSandersonMS
description: ASP.NET Core の単一ページ アプリケーション (SPA) プロジェクト テンプレートをインストールして、作業を開始する方法を説明します。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: eda4817de007f3c3184b2ba6ed6c97989ff17da5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>ASP.NET Core での単一ページ アプリケーション テンプレートの使用

> [!NOTE]
> リリースされた .NET Core 2.0.x SDK には、Angular、React、および Redux と組み合わせた React 向けの古いプロジェクト テンプレートが含まれています。 これは、これらの古いプロジェクト テンプレートに関するドキュメントではありません。 これは、最新の Angular、React、Redux と組み合わせた React 向けのテンプレートについてのドキュメントです。これらのテンプレートは、手動で ASP.NET Core 2.0 にインストールできます。 テンプレートは既定で ASP.NET Core 2.1 に含まれています。

## <a name="prerequisites"></a>必須コンポーネント

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org) (バージョン 6 以降)

## <a name="installation"></a>インストール

ASP.NET Core 2.0 を使用している場合は、次のコマンドを実行して、Angular、React、Redux と組み合わせた React 向けの、更新された ASP.NET Core テンプレートをインストールします。

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a>テンプレートの使用

- [Angular プロジェクト テンプレートの使用](xref:spa/angular)
- [React プロジェクト テンプレートの使用](xref:spa/react)
- [Redux と組み合わせた React プロジェクト テンプレートの使用](xref:spa/react-with-redux)
