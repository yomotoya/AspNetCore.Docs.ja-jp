---
title: ASP.NET Core で React-with-Redux プロジェクト テンプレートを使用する
author: SteveSandersonMS
description: React with Redux と create-react-app 用の ASP.NET Core シングル ページ アプリケーション (SPA) プロジェクト テンプレートの使用を開始する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/13/2019
uid: spa/react-with-redux
ms.openlocfilehash: ed2e9aea449ddb09fef049a391f40f57452786a8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893689"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="cc4dd-103">ASP.NET Core で React-with-Redux プロジェクト テンプレートを使用する</span><span class="sxs-lookup"><span data-stu-id="cc4dd-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

<span data-ttu-id="cc4dd-104">更新された React-with-Redux プロジェクト テンプレートは、React、Redux、および [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) 規則を使用してリッチなクライアント側ユーザー インターフェイス (UI) を実装する ASP.NET Core アプリを開発する場合に、便利な開始点として利用できます。</span><span class="sxs-lookup"><span data-stu-id="cc4dd-104">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="cc4dd-105">プロジェクト作成コマンドを除き、React-with-Redux テンプレートに関するすべての情報は React テンプレートと同じです。</span><span class="sxs-lookup"><span data-stu-id="cc4dd-105">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="cc4dd-106">このプロジェクトの種類を作成するには、`dotnet new react` の代わりに `dotnet new reactredux` を実行します。</span><span class="sxs-lookup"><span data-stu-id="cc4dd-106">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="cc4dd-107">両方の React ベースのテンプレートに共通する機能の詳細については、[React テンプレートのドキュメント](xref:spa/react)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cc4dd-107">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>

<span data-ttu-id="cc4dd-108">IIS で React-と-Redux サブアプリケーションを構成する方法の詳細については、次を参照してください。 [ReactRedux テンプレート 2.1。IIS で SPA を使用することができません (aspnet/テンプレート&num;555)](https://github.com/aspnet/Templating/issues/555)します。</span><span class="sxs-lookup"><span data-stu-id="cc4dd-108">For information on configuring a React-with-Redux sub-application in IIS, see [ReactRedux Template 2.1: Unable to use SPA on IIS (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).</span></span>
