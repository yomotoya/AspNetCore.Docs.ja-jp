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
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>ASP.NET Core で React-with-Redux プロジェクト テンプレートを使用する

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> このドキュメントには、ASP.NET Core 2.0 に含まれる、React-と-(続編) プロジェクト テンプレートはに関するものです。 これは手動で更新できる新しい React-と-Redux テンプレートに関するものです。 テンプレートは、ASP.NET Core 2.1 で使用できる、またはそれ以降。

::: moniker-end

更新された React-with-Redux プロジェクト テンプレートは、React、Redux、および [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) 規則を使用してリッチなクライアント側ユーザー インターフェイス (UI) を実装する ASP.NET Core アプリを開発する場合に、便利な開始点として利用できます。

プロジェクト作成コマンドを除き、React-with-Redux テンプレートに関するすべての情報は React テンプレートと同じです。 このプロジェクトの種類を作成するには、`dotnet new react` の代わりに `dotnet new reactredux` を実行します。 両方の React ベースのテンプレートに共通する機能の詳細については、[React テンプレートのドキュメント](xref:spa/react)を参照してください。

IIS で React-と-Redux サブアプリケーションを構成する方法の詳細については、次を参照してください。 [ReactRedux テンプレート 2.1。IIS で SPA を使用することができません (aspnet/テンプレート&num;555)](https://github.com/aspnet/Templating/issues/555)します。
