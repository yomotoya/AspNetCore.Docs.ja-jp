---
title: ASP.NET Core で React-with-Redux プロジェクト テンプレートを使用する
author: SteveSandersonMS
description: React with Redux と create-react-app 用の ASP.NET Core シングル ページ アプリケーション (SPA) プロジェクト テンプレートの使用を開始する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: dab3d20865250aae548bff4614e631dd7c73b46f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291485"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="69cce-103">ASP.NET Core で React-with-Redux プロジェクト テンプレートを使用する</span><span class="sxs-lookup"><span data-stu-id="69cce-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="69cce-104">このドキュメントは、ASP.NET Core 2.0 に含まれている React-with-Redux プロジェクト テンプレートについて説明するものではありません。</span><span class="sxs-lookup"><span data-stu-id="69cce-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="69cce-105">手動で更新できる新しい React-with-Redux テンプレートについて説明するものです。</span><span class="sxs-lookup"><span data-stu-id="69cce-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="69cce-106">このテンプレートは、ASP.NET Core 2.1 に既定で含まれています。</span><span class="sxs-lookup"><span data-stu-id="69cce-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="69cce-107">更新された React-with-Redux プロジェクト テンプレートは、React、Redux、および [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) 規則を使用してリッチなクライアント側ユーザー インターフェイス (UI) を実装する ASP.NET Core アプリを開発する場合に、便利な開始点として利用できます。</span><span class="sxs-lookup"><span data-stu-id="69cce-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="69cce-108">プロジェクト作成コマンドを除き、React-with-Redux テンプレートに関するすべての情報は React テンプレートと同じです。</span><span class="sxs-lookup"><span data-stu-id="69cce-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="69cce-109">このプロジェクトの種類を作成するには、`dotnet new react` の代わりに `dotnet new reactredux` を実行します。</span><span class="sxs-lookup"><span data-stu-id="69cce-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="69cce-110">両方の React ベースのテンプレートに共通する機能の詳細については、[React テンプレートのドキュメント](xref:spa/react)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="69cce-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
