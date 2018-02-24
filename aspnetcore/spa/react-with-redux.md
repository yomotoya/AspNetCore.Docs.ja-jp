---
title: "反応するので-(続編) プロジェクト テンプレートを使用します。"
author: SteveSandersonMS
description: "Redux と作成対応アプリとの対応の ASP.NET Core 単一ページ アプリケーション (SPA) プロジェクト テンプレートを使って開始する方法を説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: f52f4a866b8bb2adeee19548df32f3e7a91761d1
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-react-with-redux-project-template"></a><span data-ttu-id="9a0fe-103">反応するので-(続編) プロジェクト テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="9a0fe-103">Use the React-with-Redux project template</span></span>

> [!NOTE]
> <span data-ttu-id="9a0fe-104">このドキュメント React-で-(続編) のプロジェクト テンプレートに含まれていない ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="9a0fe-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="9a0fe-105">手動で更新できる新しい React-で-(続編) テンプレートことです。</span><span class="sxs-lookup"><span data-stu-id="9a0fe-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="9a0fe-106">既定では ASP.NET Core 2.1 では、テンプレートが含まれます。</span><span class="sxs-lookup"><span data-stu-id="9a0fe-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="9a0fe-107">更新の対応-Redux とプロジェクト テンプレートは、便利な開始点を使用して ASP.NET Core アプリに対応し、Redux、および[作成対応アプリ](https://github.com/facebookincubator/create-react-app)豊富なクライアント側のユーザー インターフェイス (UI) を実装する (CRA) 規則。</span><span class="sxs-lookup"><span data-stu-id="9a0fe-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="9a0fe-108">プロジェクト作成コマンドを除くすべてテンプレートについては、React-で-(続編) は、対応のテンプレートとして同じです。</span><span class="sxs-lookup"><span data-stu-id="9a0fe-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="9a0fe-109">このプロジェクトの種類を作成する実行`dotnet new reactredux`の代わりに`dotnet new react`です。</span><span class="sxs-lookup"><span data-stu-id="9a0fe-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="9a0fe-110">両方の反応するベース テンプレートに共通の機能の詳細については、次を参照してください。[テンプレートのドキュメントを React](xref:spa/react)です。</span><span class="sxs-lookup"><span data-stu-id="9a0fe-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
