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
ms.openlocfilehash: 71a8702b9f6e9a2b40f150026f1955450e0fa7a3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-react-with-redux-project-template-release-candidate"></a><span data-ttu-id="a04fe-103">反応するので-(続編) プロジェクト テンプレート (リリース候補) を使用します。</span><span class="sxs-lookup"><span data-stu-id="a04fe-103">Use the React-with-Redux project template (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="a04fe-104">このドキュメントは、リリースの反応するので-(続編) プロジェクト テンプレートではありません。</span><span class="sxs-lookup"><span data-stu-id="a04fe-104">This documentation isn't about the released React-with-Redux project template.</span></span> <span data-ttu-id="a04fe-105">**このドキュメントは、React-で-(続編) テンプレートのリリース候補です。**</span><span class="sxs-lookup"><span data-stu-id="a04fe-105">**This documentation is about the release candidate of the React-with-Redux template.**</span></span> <span data-ttu-id="a04fe-106">初期 2018 でリリースされたバージョンを出荷する予定です。</span><span class="sxs-lookup"><span data-stu-id="a04fe-106">We hope to ship the released version in early 2018.</span></span>

<span data-ttu-id="a04fe-107">更新の対応-Redux とプロジェクト テンプレートは、便利な開始点を使用して ASP.NET Core アプリに対応し、Redux、および[作成対応アプリ](https://github.com/facebookincubator/create-react-app)豊富なクライアント側のユーザー インターフェイス (UI) を実装する (CRA) 規則。</span><span class="sxs-lookup"><span data-stu-id="a04fe-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="a04fe-108">プロジェクト作成コマンドを除くすべてテンプレートについては、React-で-(続編) は、対応のテンプレートとして同じです。</span><span class="sxs-lookup"><span data-stu-id="a04fe-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="a04fe-109">このプロジェクトの種類を作成する実行`dotnet new reactredux`の代わりに`dotnet new react`です。</span><span class="sxs-lookup"><span data-stu-id="a04fe-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="a04fe-110">両方の反応するベース テンプレートに共通の機能の詳細については、次を参照してください。[テンプレートのドキュメントを React](xref:spa/react)です。</span><span class="sxs-lookup"><span data-stu-id="a04fe-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
