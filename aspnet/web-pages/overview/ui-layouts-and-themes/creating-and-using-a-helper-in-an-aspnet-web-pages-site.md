---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: 作成して、ヘルパーを使用して ASP.NET web Pages (Razor) サイト |Microsoft ドキュメント
author: tfitzmac
description: この記事では、ASP.NET Web Pages (Razor) の web サイトにヘルパーを作成する方法について説明します。 コードとパフォーマンスにマークアップを含む再使用可能なコンポーネントをヘルパーには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530001"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトで作成およびヘルパー
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイトにヘルパーを作成する方法について説明します。 A*ヘルパー*コードと面倒か複雑になる可能性があるタスクを実行するマークアップを含む再使用可能なコンポーネントです。
> 
> **学習する内容。** 
> 
> - 作成して単純なヘルパーを使用する方法。
> 
> アーティクルで導入された ASP.NET 機能を次に示します。
> 
> - `@helper`構文です。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも動作します。


## <a name="overview-of-helpers"></a>ヘルパーの概要

サイト内のさまざまなページで、同じタスクを実行する必要がある場合は、ヘルパーを使用することができます。 ASP.NET Web ページには、ヘルパーの多くが含まれていて、ダウンロードしてインストールすることが多くがあります。 (ASP.NET Web Pages での組み込みのヘルパーの一覧が記載されて、 [ASP.NET API クイック リファレンス](https://go.microsoft.com/fwlink/?LinkId=202907))。お客様のニーズを満たしていない既存のヘルパーの場合は、独自のヘルパーを作成できます。

ヘルパーを使用して、複数のページにわたって共通コード ブロックを使用できます。 ページに多くの場合とは別に標準の段落に設定されている注項目を作成するとします。 としてメモを作成するなど、`<div>`要素をボックスに罫線のスタイルがします。 メモを表示するたびに、ページにこの同じマークアップを追加するのではなく、ヘルパーとしてマークアップをパッケージ化することができます。 メモに 1 行のコードを挿入することができますし、必要な任意の場所。

次のようなヘルパーの使用は、ページの各により、コードと簡素化されを読みやすくします。 これもやすく、サイトを維持するために、ノートを検索する方法を変更する必要がある場合は、1 か所でマークアップを変更できるためです。

## <a name="creating-a-helper"></a>ヘルパーの作成

この手順では、上記のメモを作成するヘルパーを作成する方法を示します。 これは単純な例ですが、カスタム ヘルパーは、マークアップとする必要がある ASP.NET コードを含めることができます。

1. という名前のフォルダーを作成、web サイトのルート フォルダーに*アプリ\_コード*です。 これは、ヘルパーなどのコンポーネントのコードを配置できる ASP.NET で予約されたフォルダーの名前です。
2. *アプリ\_コード*フォルダーを作成、新しい *.cshtml*ファイルし、名前を付けます*MyHelpers.cshtml*です。
3. 次のように、既存のコンテンツを置き換えます。

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    コードを使用して、`@helper`という名前の新しいヘルパーを宣言する構文`MakeNote`です。 この特定のヘルパーには、という名前のパラメーターを渡すことができます`content`テキストとマークアップの組み合わせを含めることができます。 ヘルパーが注本文を使用して、文字列を挿入、`@content`変数。

    ファイルの名前はことに注意してください。 *MyHelpers.cshtml*、ヘルパーがという名前が、`MakeNote`です。 1 つのファイルに複数のカスタム ヘルパーを配置することができます。
4. ファイルを保存して閉じます。

## <a name="using-the-helper-in-a-page"></a>ページで、ヘルパーの使用

1. ルート フォルダーと呼ばれる新しい空のファイルを作成する*TestHelper.cshtml*です。
2. 次のコードをファイルに追加します。

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    作成するヘルパーを呼び出すには、次のように使用します。`@`ここで、ヘルパーは、ドット、ファイル名および順ヘルパー名。 (複数のフォルダーが含まれていた、*アプリ\_コード*フォルダー、構文を使用する可能性があります`@FolderName.FileName.HelperName`内でいずれかのヘルパーを呼び出す入れ子フォルダー レベル)。 かっこで囲まれた引用符で追加したテキストは、ヘルパーは、ノートの web ページの一部として表示されるテキストです。
3. ページを保存し、ブラウザーで実行します。 ヘルパーはヘルパーが呼び出されている右注項目を生成します。 2 つの段落です。

    ![ブラウザーとヘルパーが指定されたテキストを囲むボックスを配置するマークアップを生成する方法でページを示すスクリーン ショット。](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>その他のリソース


[Razor ヘルパーとしての水平方向のメニュー](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341)です。 Mike 教皇によるブログ エントリでは、マークアップ、CSS、およびコードを使用するヘルパーと水平方向のメニューを作成する方法を示します。

[For WebMatrix and ASP.NET MVC3 ページ ヘルパーを活用する ASP.NET Web での HTML5](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx)です。 Sam 西によるブログ エントリは、HTML5 をレンダリングするヘルパーを示しています。`Canvas`要素。

[違い@Helpersと@FunctionsWebMatrix で](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix)です。 Mike Brind によるブログ エントリについて説明します`@helper`構文と`@function`構文と、それぞれを使用します。
