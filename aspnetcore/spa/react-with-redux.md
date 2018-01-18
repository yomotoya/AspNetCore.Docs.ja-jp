---
title: "反応するので-(続編) プロジェクト テンプレートを使用します。"
author: SteveSandersonMS
description: "Redux と作成対応アプリとの対応の ASP.NET Core 単一ページ アプリケーション (SPA) リリース候補プロジェクト テンプレートを使って開始する方法を説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: c92aa070a307129afd91c432739625b5e0f8b29e
ms.sourcegitcommit: fc98e93464ccf37d9904e89a71cdddbd4bbdb86a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/05/2018
---
# <a name="use-the-react-with-redux-project-template-release-candidate"></a><span data-ttu-id="da4a8-103">反応するので-(続編) プロジェクト テンプレート (リリース候補) を使用します。</span><span class="sxs-lookup"><span data-stu-id="da4a8-103">Use the React-with-Redux project template (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="da4a8-104">このドキュメントは、リリースの反応するので-(続編) のプロジェクト テンプレートはありません。</span><span class="sxs-lookup"><span data-stu-id="da4a8-104">This documentation is not about the released React-with-Redux project template.</span></span> <span data-ttu-id="da4a8-105">**このドキュメントは、React-で-(続編) テンプレートのリリース候補です。**</span><span class="sxs-lookup"><span data-stu-id="da4a8-105">**This documentation is about the release candidate of the React-with-Redux template.**</span></span> <span data-ttu-id="da4a8-106">初期 2018 でリリースされたバージョンを出荷する予定です。</span><span class="sxs-lookup"><span data-stu-id="da4a8-106">We hope to ship the released version in early 2018.</span></span>

<span data-ttu-id="da4a8-107">更新の対応-Redux とプロジェクト テンプレートは、便利な開始点を使用して ASP.NET Core アプリに対応し、Redux、および[作成対応アプリ](https://github.com/facebookincubator/create-react-app)豊富なクライアント側のユーザー インターフェイス (UI) を実装する (CRA) 規則。</span><span class="sxs-lookup"><span data-stu-id="da4a8-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="da4a8-108">プロジェクト作成コマンドを除くすべてテンプレートについては、React-で-(続編) は、対応のテンプレートとして同じです。</span><span class="sxs-lookup"><span data-stu-id="da4a8-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="da4a8-109">このプロジェクトの種類を作成する実行`dotnet new reactredux`の代わりに`dotnet new react`です。</span><span class="sxs-lookup"><span data-stu-id="da4a8-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="da4a8-110">両方の反応するベース テンプレートに共通の機能の詳細については、次を参照してください。[テンプレートのドキュメントを React](xref:spa/react)です。</span><span class="sxs-lookup"><span data-stu-id="da4a8-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
