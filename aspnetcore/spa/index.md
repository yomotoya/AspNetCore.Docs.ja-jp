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
# <a name="use-the-single-page-application-templates"></a><span data-ttu-id="1fe44-103">単一ページ アプリケーション テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="1fe44-103">Use the Single Page Application templates</span></span>

> [!NOTE]
> <span data-ttu-id="1fe44-104">リリースされた .NET Core 2.0.x SDK には、Angular、React、および Redux と組み合わせた React 向けの古いプロジェクト テンプレートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1fe44-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="1fe44-105">これは、これらの古いプロジェクト テンプレートに関するドキュメントではありません。</span><span class="sxs-lookup"><span data-stu-id="1fe44-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="1fe44-106">これは、最新の Angular、React、Redux と組み合わせた React 向けのテンプレートについてのドキュメントです。これらのテンプレートは、手動で ASP.NET Core 2.0 にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="1fe44-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="1fe44-107">テンプレートは既定で ASP.NET Core 2.1 に含まれています。</span><span class="sxs-lookup"><span data-stu-id="1fe44-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1fe44-108">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="1fe44-108">Prerequisites</span></span>

* <span data-ttu-id="1fe44-109">[.NET Core SDK](https://www.microsoft.com/net/download) (バージョン 2.0.0 以降)</span><span class="sxs-lookup"><span data-stu-id="1fe44-109">[.NET Core SDK](https://www.microsoft.com/net/download), version 2.0.0 or later</span></span>
* <span data-ttu-id="1fe44-110">[Node.js](https://nodejs.org) (バージョン 6 以降)</span><span class="sxs-lookup"><span data-stu-id="1fe44-110">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="1fe44-111">インストール</span><span class="sxs-lookup"><span data-stu-id="1fe44-111">Installation</span></span>

<span data-ttu-id="1fe44-112">ASP.NET Core 2.0 を使用している場合は、次のコマンドを実行して、Angular、React、Redux と組み合わせた React 向けの、更新された ASP.NET Core テンプレートをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1fe44-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a><span data-ttu-id="1fe44-113">テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="1fe44-113">Use the templates</span></span>

- [<span data-ttu-id="1fe44-114">Angular プロジェクト テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="1fe44-114">Use the Angular project template</span></span>](xref:spa/angular)
- [<span data-ttu-id="1fe44-115">React プロジェクト テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="1fe44-115">Use the React project template</span></span>](xref:spa/react)
- [<span data-ttu-id="1fe44-116">Redux と組み合わせた React プロジェクト テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="1fe44-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
