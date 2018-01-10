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
# <a name="use-the-single-page-application-templates-release-candidate"></a><span data-ttu-id="e245f-103">単一ページ アプリケーション テンプレート (リリース候補) の使用</span><span class="sxs-lookup"><span data-stu-id="e245f-103">Use the Single-Page Application templates (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="e245f-104">リリースされた .NET Core 2.0.x SDK には、Angular、React、および Redux と組み合わせた React 向けのプロジェクト テンプレートが用意されています。</span><span class="sxs-lookup"><span data-stu-id="e245f-104">The released .NET Core 2.0.x SDK includes project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="e245f-105">**これは、リリースされたこれらのプロジェクト テンプレートに関するドキュメントではありません。**</span><span class="sxs-lookup"><span data-stu-id="e245f-105">**This documentation is not about those released project templates.**</span></span> <span data-ttu-id="e245f-106">このドキュメントは、2018 年前半に提供予定の、Angular、React、および Redux と組み合わせた React の次のバージョンのテンプレートに対応しています。</span><span class="sxs-lookup"><span data-stu-id="e245f-106">This documentation is for the next version of the Angular, React, and React with Redux templates, which we hope to ship in early 2018.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e245f-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="e245f-107">Prerequisites</span></span>

* <span data-ttu-id="e245f-108">[.NET Core SDK](https://www.microsoft.com/net/download) (バージョン 2.0.0 以降)</span><span class="sxs-lookup"><span data-stu-id="e245f-108">[.NET Core SDK](https://www.microsoft.com/net/download), version 2.0.0 or later</span></span>
* <span data-ttu-id="e245f-109">[Node.js](https://nodejs.org) (バージョン 6 以降)</span><span class="sxs-lookup"><span data-stu-id="e245f-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="e245f-110">インストール</span><span class="sxs-lookup"><span data-stu-id="e245f-110">Installation</span></span>

<span data-ttu-id="e245f-111">次のコマンドを実行して、Angular、React、および Redux と組み合わせた React 向けの、ASP.NET Core テンプレートの**リリース候補**をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e245f-111">Run the following command to install the **release candidate** of the ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0-rc1-final
```

## <a name="use-the-templates"></a><span data-ttu-id="e245f-112">テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="e245f-112">Use the templates</span></span>

- [<span data-ttu-id="e245f-113">Angular プロジェクト テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="e245f-113">Use the Angular project template</span></span>](xref:spa/angular)
- [<span data-ttu-id="e245f-114">React プロジェクト テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="e245f-114">Use the React project template</span></span>](xref:spa/react)
- [<span data-ttu-id="e245f-115">Redux と組み合わせた React プロジェクト テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="e245f-115">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
