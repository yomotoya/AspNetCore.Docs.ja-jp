---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: ページ (Razor) サイトを ASP.NET web ヘルパーをインストールする |Microsoft Docs
author: Rick-Anderson
description: この記事では、ASP.NET Web Pages (Razor) の web サイトでヘルパーをインストールする方法について説明します。 コードとごとにマークアップを含む再利用可能なコンポーネントをヘルパーには.
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 5ad717cd7c64e830ce66d5e1361d0eb6ef3cbbec
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021366"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>ASP.NET Web ページ (Razor) サイトでヘルパーをインストールします。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイトでヘルパーをインストールする方法について説明します。 A*ヘルパー*はコードと面倒か複雑になる可能性があるタスクを実行するためのマークアップを含む再利用可能なコンポーネントです。
> 
> 学習内容。
> 
> - WebMatrix 3 を使用して作成された web サイトでヘルパーをインストールする方法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>ヘルパーの概要

多くの場合、ユーザーは web ページで実行するいくつかのタスクは、多くのコードが必要または余分な知識が必要です。 例としては、データ用のグラフを表示します。ページで、Twitter の「フォロー」ボタンを配置します。web サイトからメールを送信トリミングまたはのイメージをサイズ変更サイトの PayPal を使用します。 簡単にこれらの種類の項目を実行する ASP.NET Web Pages を使用できます*ヘルパー*します。 ヘルパーは、コンポーネントをインストールすること、サイトとできるようにする、Razor コードの 2 行だけを使用して一般的なタスクを実行します。

ASP.NET Web ページが、いくつかのヘルパーが組み込まれています。 ただし、多くのヘルパーは、NuGet パッケージ マネージャーを使用して提供されているパッケージ (アドインの場合) で利用できます。 インストールの詳細をすべて処理しと NuGet を使用してをインストールするパッケージを選択できます。

## <a name="installing-a-helper-in-webmatrix-3"></a>WebMatrix 3 でヘルパーをインストールします。

1. WebMatrix 3 をクリックして、 **NuGet**ボタンをクリックします。

    ![WebMatrix での NuGet ギャラリー ダイアログ ボックス](installing-helpers/_static/image1.png)
2. これにより、NuGet パッケージ マネージャーを起動し、利用可能なパッケージを表示します。 検索ボックスには、インストールするヘルパーのキーワードを入力します。

    ![WebMatrix での NuGet ギャラリー ダイアログ ボックス](installing-helpers/_static/image2.png)
3. パッケージを選択し、をクリックし、**インストール**します。 クリックして**はい**されたら、パッケージをインストールし、条項に同意することを指定するかどうか。

     初めてのヘルパーをインストールした場合は、NuGet では、ヘルパーを構成するコードの web サイトのフォルダーが作成されます。
4. ヘルパーをアンインストールするには、クリックして、**ギャラリー**ボタンをクリックし、**インストール済み**タブをクリックしをアンインストールするパッケージを選択します。

## <a name="installing-the-twitter-helper"></a>Twitter ヘルパーをインストールします。

Twitter API の最新バージョンは、NuGet からインストールする Twitter ヘルパーと互換性がありません。 代わりを参照してください、 [WebMatrix を使用した Twitter Helper](twitter-helper.md)プロジェクトでの Twitter ヘルパーを設定する方法の詳細については、トピック。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


[概要の ASP.NET Web ページ 2 のプログラミングの基礎](../getting-started/introducing-razor-syntax-c.md)

[WebMatrix を使用した twitter ヘルパー](twitter-helper.md)
