---
title: "単一ページ アプリケーション テンプレートの使用"
author: SteveSandersonMS
description: "ASP.NET Core の単一ページ アプリケーション (SPA) のリリース候補プロジェクト テンプレートをインストールして、作業を開始する方法を説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: 0ac3803aabdc148401b9d5b614645a8560c9a089
ms.sourcegitcommit: fc98e93464ccf37d9904e89a71cdddbd4bbdb86a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/05/2018
---
# <a name="use-the-single-page-application-templates-release-candidate"></a>単一ページ アプリケーション テンプレート (リリース候補) の使用

> [!NOTE]
> リリースされた .NET Core 2.0.x SDK には、Angular、React、および Redux と組み合わせた React 向けのプロジェクト テンプレートが用意されています。 **これは、リリースされたこれらのプロジェクト テンプレートに関するドキュメントではありません。** このドキュメントは、2018 年前半に提供予定の、Angular、React、および Redux と組み合わせた React の次のバージョンのテンプレートに対応しています。

## <a name="prerequisites"></a>必須コンポーネント

* [.NET Core SDK](https://www.microsoft.com/net/download) (バージョン 2.0.0 以降)
* [Node.js](https://nodejs.org) (バージョン 6 以降)

## <a name="installation"></a>インストール

次のコマンドを実行して、Angular、React、および Redux と組み合わせた React 向けの、ASP.NET Core テンプレートの**リリース候補**をインストールします。

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0-rc1-final
```

## <a name="use-the-templates"></a>テンプレートの使用

- [Angular プロジェクト テンプレートの使用](xref:spa/angular)
- [React プロジェクト テンプレートの使用](xref:spa/react)
- [Redux と組み合わせた React プロジェクト テンプレートの使用](xref:spa/react-with-redux)
