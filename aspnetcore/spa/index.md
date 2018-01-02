---
title: "単一ページ アプリケーション テンプレートの使用"
author: SteveSandersonMS
description: "ASP.NET Core の単一ページ アプリケーション (SPA) プロジェクト テンプレート (プレビュー) をインストールして、作業を開始する方法を説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: 59031d79a9558bb8fc94e55ac04e70876618dc02
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2017
---
# <a name="use-the-single-page-application-templates-preview"></a>単一ページ アプリケーション テンプレート (プレビュー) の使用

> [!NOTE]
> リリースされた .NET Core 2.0.x SDK には、Angular、React、および Redux と組み合わせた React 向けのプロジェクト テンプレートが用意されています。 **これは、リリースされたこれらのプロジェクト テンプレートに関するドキュメントではありません。** このドキュメントは、2018 年前半に提供予定の、Angular、React、および Redux と組み合わせた React の次のバージョンのテンプレートに対応しています。

## <a name="prerequisites"></a>必須コンポーネント

* [.NET Core SDK](https://www.microsoft.com/net/download) (バージョン 2.0.0 以降)
* [Node.js](https://nodejs.org) (バージョン 6 以降)

## <a name="installation"></a>インストール

次のコマンドを実行して、Angular、React、および Redux と組み合わせた React 向けの、ASP.NET Core テンプレートの**プレビュー** リリースをインストールします。

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0-preview1-final
```

## <a name="use-the-templates"></a>テンプレートの使用

- [Angular プロジェクト テンプレートの使用](xref:spa/angular)
- [React プロジェクト テンプレートの使用](xref:spa/react)
- [Redux と組み合わせた React プロジェクト テンプレートの使用](xref:spa/react-with-redux)
