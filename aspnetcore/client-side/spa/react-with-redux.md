---
title: ASP.NET Core で React-with-Redux プロジェクト テンプレートを使用する
author: SteveSandersonMS
description: React with Redux と create-react-app 用の ASP.NET Core シングル ページ アプリケーション (SPA) プロジェクト テンプレートの使用を開始する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: c1aedcf1e14a9e7b339b60dd02c4267cd5945a49
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341621"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="81ac5-103">ASP.NET Core で React-with-Redux プロジェクト テンプレートを使用する</span><span class="sxs-lookup"><span data-stu-id="81ac5-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="81ac5-104">このドキュメントには、ASP.NET Core 2.0 に含まれる、React-と-(続編) プロジェクト テンプレートはに関するものです。</span><span class="sxs-lookup"><span data-stu-id="81ac5-104">This documentation doesn't pertain to the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="81ac5-105">これは手動で更新できる新しい React-と-Redux テンプレートに関するものです。</span><span class="sxs-lookup"><span data-stu-id="81ac5-105">It pertains to the newer React-with-Redux template that you can update manually.</span></span> <span data-ttu-id="81ac5-106">テンプレートは、ASP.NET Core 2.1 で使用できる、またはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="81ac5-106">The template is available in ASP.NET Core 2.1 or later.</span></span>

::: moniker-end

<span data-ttu-id="81ac5-107">更新された React-with-Redux プロジェクト テンプレートは、React、Redux、および [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) 規則を使用してリッチなクライアント側ユーザー インターフェイス (UI) を実装する ASP.NET Core アプリを開発する場合に、便利な開始点として利用できます。</span><span class="sxs-lookup"><span data-stu-id="81ac5-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="81ac5-108">プロジェクト作成コマンドを除き、React-with-Redux テンプレートに関するすべての情報は React テンプレートと同じです。</span><span class="sxs-lookup"><span data-stu-id="81ac5-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="81ac5-109">このプロジェクトの種類を作成するには、`dotnet new react` の代わりに `dotnet new reactredux` を実行します。</span><span class="sxs-lookup"><span data-stu-id="81ac5-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="81ac5-110">両方の React ベースのテンプレートに共通する機能の詳細については、[React テンプレートのドキュメント](xref:spa/react)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="81ac5-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>

<span data-ttu-id="81ac5-111">IIS で React-と-Redux サブアプリケーションを構成する方法の詳細については、次を参照してください。 [ReactRedux テンプレート 2.1。IIS で SPA を使用することができません (aspnet/テンプレート&num;555)](https://github.com/aspnet/Templating/issues/555)します。</span><span class="sxs-lookup"><span data-stu-id="81ac5-111">For information on configuring a React-with-Redux sub-application in IIS, see [ReactRedux Template 2.1: Unable to use SPA on IIS (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).</span></span>
