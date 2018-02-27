---
title: "単一ページ アプリケーション テンプレートの使用"
author: SteveSandersonMS
description: "ASP.NET Core の単一ページ アプリケーション (SPA) プロジェクト テンプレートをインストールして、作業を開始する方法を説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: 63b56de101199e9ea0d66d89d2dd7288e47902f6
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-single-page-application-templates"></a>単一ページ アプリケーション テンプレートの使用

> [!NOTE]
> リリースされた .NET Core 2.0.x SDK には、Angular、React、および Redux と組み合わせた React 向けの古いプロジェクト テンプレートが含まれています。 これは、これらの古いプロジェクト テンプレートに関するドキュメントではありません。 これは、最新の Angular、React、Redux と組み合わせた React 向けのテンプレートについてのドキュメントです。これらのテンプレートは、手動で ASP.NET Core 2.0 にインストールできます。 テンプレートは既定で ASP.NET Core 2.1 に含まれています。

## <a name="prerequisites"></a>必須コンポーネント

* [.NET Core SDK](https://www.microsoft.com/net/download) (バージョン 2.0.0 以降)
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
