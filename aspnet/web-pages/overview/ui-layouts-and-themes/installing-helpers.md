---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: ページ (Razor) サイトの ASP.NET web ヘルパーをインストールする |Microsoft ドキュメント
author: tfitzmac
description: この記事では、ASP.NET Web Pages (Razor) の web サイトでヘルパーをインストールする方法について説明します。 コードおよびごとにマークアップを含む再使用可能なコンポーネントをヘルパーには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトにヘルパーをインストールします。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイトでヘルパーをインストールする方法について説明します。 A*ヘルパー*コードと面倒か複雑になる可能性があるタスクを実行するマークアップを含む再使用可能なコンポーネントです。
> 
> 学習する内容。
> 
> - WebMatrix 3 を使用して作成された web サイトでヘルパーをインストールする方法です。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>ヘルパーの概要

多くの場合、ユーザーは web ページで実行するいくつかのタスクは、多くのコードを要求したり、余分な知識が必要があります。 例としては、データのグラフが表示されます。ページで、Twitter [フォロー] ボタンを配置します。web サイト; からメールを送信トリミングまたはイメージのサイズを変更します。サイトの PayPal を使用します。 簡単にこれをこれらの処理を行うには、ASP.NET Web Pages を使用できます*ヘルパー*です。 ヘルパーは、コンポーネントをインストールすること、サイトとできるようにするが、2 の Razor コードの行だけを使用して一般的なタスクを実行します。

ASP.NET Web ページには、いくつかのヘルパーが組み込まれているがあります。 ただし、多くヘルパーは、NuGet パッケージ マネージャーを使用して提供されているパッケージ (アドインの場合) で使用できます。 インストールの詳細をすべてを管理しと NuGet を使用してをインストールするパッケージを選択できます。

## <a name="installing-a-helper-in-webmatrix-3"></a>3 の webmatrix ヘルパーをインストールします。

1. WebMatrix 3 でをクリックして、 **NuGet**ボタンをクリックします。

    ![WebMatrix で NuGet ギャラリー ダイアログ ボックス](installing-helpers/_static/image1.png)
2. これは、NuGet パッケージ マネージャーを起動し、利用可能なパッケージを表示します。 [検索] ボックスをインストールするヘルパーのキーワードを入力します。

    ![WebMatrix で NuGet ギャラリー ダイアログ ボックス](installing-helpers/_static/image2.png)
3. パッケージを選択し、をクリックして**インストール**です。 をクリックして**はい**されたら、パッケージをインストールし、条項に同意することを指定するかどうか。

     初めてヘルパーをインストールした場合は、NuGet は、ヘルパーを構成するコードの web サイトにフォルダーを作成します。
4. ヘルパーをアンインストールするには、クリックして、**ギャラリー**ボタンをクリックし、**インストール**タブをクリックしをアンインストールするパッケージを選択します。

## <a name="installing-the-twitter-helper"></a>Twitter helper をインストールします。

Twitter API の最新バージョンは、NuGet をインストールする Twitter ヘルパーと互換性がありません。 代わりを参照してください、 [webmatrix Twitter Helper](twitter-helper.md)については、プロジェクトの Twitter helper をセットアップする方法に関するトピック。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


[紹介の ASP.NET Web Pages 2 のプログラミングの基礎](../getting-started/introducing-razor-syntax-c.md)

[Twitter の webmatrix ヘルパー](twitter-helper.md)
