---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: 作成して、ヘルパーを使用して ASP.NET web Pages (Razor) サイト |Microsoft Docs
author: Rick-Anderson
description: この記事では、ASP.NET Web Pages (Razor) の web サイトでヘルパーを作成する方法について説明します。 コードとパフォーマンスにマークアップを含む再利用可能なコンポーネントをヘルパーには.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 3b54ba8a35a354eb0562a47866cd8b5dcb66bc86
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021509"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトで作成およびヘルパー
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイトでヘルパーを作成する方法について説明します。 A*ヘルパー*はコードと面倒か複雑になる可能性があるタスクを実行するためのマークアップを含む再利用可能なコンポーネントです。
> 
> **学習内容。** 
> 
> - 作成して単純なヘルパーを使用する方法。
> 
> この記事で導入された ASP.NET 機能を次に示します。
> 
> - `@helper`構文。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも機能します。


## <a name="overview-of-helpers"></a>ヘルパーの概要

サイト内のさまざまなページで、同じタスクを実行する必要がある場合は、ヘルパーを使用できます。 ASP.NET Web ページには、さまざまなヘルパーが含まれています。 また、ダウンロードしてインストールをさらに多くがあります。 (ASP.NET Web Pages での組み込みのヘルパーの一覧が記載されて、 [ASP.NET API クイック リファレンス](https://go.microsoft.com/fwlink/?LinkId=202907))。ニーズを満たす既存のヘルパーの場合は、独自のヘルパーを作成できます。

ヘルパーを使用して、複数のページ、一般的なコードのブロックを使用できます。 ページに多くの場合する通常の段落とは別に設定されている注項目を作成するとします。 としてメモを作成するなど、`<div>`要素を境界線付きのボックスとしてスタイルが適用されます。 メモを表示するたびに、この同じマークアップをページに追加ではなく、ヘルパーとしてマークアップをパッケージ化することができます。 メモに 1 行のコードを挿入して必要な任意の場所。

このようなヘルパーを使用しては各ページでにより、コードと簡素化され、読みやすくします。 簡単に、サイトを維持するために、ノートを検索する方法を変更する必要がある場合は、1 か所でマークアップを変更できるためです。

## <a name="creating-a-helper"></a>ヘルパーの作成

この手順では、前述のように、メモを作成するヘルパーを作成する方法を示します。 これは単純な例ですが、カスタム ヘルパーは、マークアップと必要な ASP.NET コードを含めることができます。

1. という名前のフォルダーを作成、web サイトのルート フォルダーに*アプリ\_コード*します。 これは、ヘルパーなどのコンポーネントのコードを配置する ASP.NET の予約済みフォルダーの名前です。
2. *アプリ\_コード*フォルダーを作成する新しい *.cshtml*ファイルし、名前を付けます*MyHelpers.cshtml*します。
3. 次のように、既存のコンテンツを置き換えます。

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    コードを使用して、`@helper`という名前の新しいヘルパーを宣言する構文`MakeNote`します。 この特定のヘルパーには、という名前のパラメーターを渡すことができます`content`テキストとマークアップの組み合わせを含めることができます。 ヘルパーにメモの本文を使用した文字列を挿入する、`@content`変数。

    ファイルの名前はことに注意してください。 *MyHelpers.cshtml*、ヘルパーがという名前が`MakeNote`します。 複数のカスタム ヘルパーは、1 つのファイルに配置できます。
4. ファイルを保存して閉じます。

## <a name="using-the-helper-in-a-page"></a>ページで、ヘルパーの使用

1. 呼ばれる新しい空のファイルを作成、ルート フォルダーで*TestHelper.cshtml*します。
2. 次のコードをファイルに追加します。

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    作成したヘルパーを呼び出すには、次のように使用します。`@`ヘルパーの名前と場所、ヘルパーは、ドットのファイル名が続きます。 (複数のフォルダーがある場合、*アプリ\_コード*フォルダー、構文を使用する`@FolderName.FileName.HelperName`入れ子になったフォルダー レベルのいずれかのヘルパーを呼び出す)。 かっこ内の引用符で追加したテキストは、ヘルパーは、web ページの注意事項の一部として表示されるテキストです。
3. ページを保存し、ブラウザーで実行します。 ヘルパー、ヘルパーが呼び出されている右注項目を生成する: 2 つの段落の間。

    ![ヘルパーが、指定したテキストを囲むボックスを配置するマークアップを生成する方法と、ブラウザーでページを示すスクリーン ショット。](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>その他のリソース


[Razor ヘルパーとしての水平メニュー](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341)します。 Mike 教皇によるブログ エントリでは、マークアップ、CSS、およびコードを使用するヘルパーと水平方向のメニューを作成する方法を示します。

[For WebMatrix and ASP.NET MVC3 ページ ヘルパーを ASP.NET Web での HTML5 の活用](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx)します。 Sam Abraham によるブログ エントリは、HTML5 を表示するヘルパーを示しています。`Canvas`要素。

[間の差@Helpersと@FunctionsWebMatrix で](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix)します。 Mike Brind によるブログ エントリについて説明します`@helper`構文と`@function`構文と使用すると、それぞれを使用します。
