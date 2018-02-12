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
ms.openlocfilehash: 2017c2ada835eb7206dcfd195f6e2c032909f9ef
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2018
---
# <a name="use-the-single-page-application-templates-release-candidate"></a><span data-ttu-id="613da-103">単一ページ アプリケーション テンプレート (リリース候補) の使用</span><span class="sxs-lookup"><span data-stu-id="613da-103">Use the Single-Page Application templates (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="613da-104">リリースされた .NET Core 2.0.x SDK には、Angular、React、および Redux と組み合わせた React 向けのプロジェクト テンプレートが用意されています。</span><span class="sxs-lookup"><span data-stu-id="613da-104">The released .NET Core 2.0.x SDK includes project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="613da-105">**これは、リリースされたこれらのプロジェクト テンプレートに関するドキュメントではありません。**</span><span class="sxs-lookup"><span data-stu-id="613da-105">**This documentation isn't about those released project templates.**</span></span> <span data-ttu-id="613da-106">このドキュメントは、2018 年前半に提供予定の、Angular、React、および Redux と組み合わせた React の次のバージョンのテンプレートに対応しています。</span><span class="sxs-lookup"><span data-stu-id="613da-106">This documentation is for the next version of the Angular, React, and React with Redux templates, which we hope to ship in early 2018.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="613da-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="613da-107">Prerequisites</span></span>

* <span data-ttu-id="613da-108">[.NET Core SDK](https://www.microsoft.com/net/download) (バージョン 2.0.0 以降)</span><span class="sxs-lookup"><span data-stu-id="613da-108">[.NET Core SDK](https://www.microsoft.com/net/download), version 2.0.0 or later</span></span>
* <span data-ttu-id="613da-109">[Node.js](https://nodejs.org) (バージョン 6 以降)</span><span class="sxs-lookup"><span data-stu-id="613da-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="613da-110">インストール</span><span class="sxs-lookup"><span data-stu-id="613da-110">Installation</span></span>

<span data-ttu-id="613da-111">次のコマンドを実行して、Angular、React、および Redux と組み合わせた React 向けの、ASP.NET Core テンプレートの**リリース候補**をインストールします。</span><span class="sxs-lookup"><span data-stu-id="613da-111">Run the following command to install the **release candidate** of the ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0-rc2-final
```

## <a name="use-the-templates"></a><span data-ttu-id="613da-112">テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="613da-112">Use the templates</span></span>

- [<span data-ttu-id="613da-113">Angular プロジェクト テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="613da-113">Use the Angular project template</span></span>](xref:spa/angular)
- [<span data-ttu-id="613da-114">React プロジェクト テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="613da-114">Use the React project template</span></span>](xref:spa/react)
- [<span data-ttu-id="613da-115">Redux と組み合わせた React プロジェクト テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="613da-115">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
