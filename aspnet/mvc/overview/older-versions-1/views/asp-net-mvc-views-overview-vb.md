---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: "ASP.NET MVC ビューの概要 (VB) |Microsoft ドキュメント"
author: StephenWalther
description: "ASP.NET MVC ビューとどのように違う HTML ページについて このチュートリアルでは、Stephen Walther はビューについて説明し、t する方法を示しています。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: c85b969aa4457d0326b4a16da218db9e11d01e10
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-views-overview-vb"></a>ASP.NET MVC ビュー (VB) の概要
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> ASP.NET MVC ビューとどのように違う HTML ページについて このチュートリアルでは、Stephen Walther はビューについて説明しを利用できるかのデータの表示と、ビュー内の HTML ヘルパーを示します。


このチュートリアルの目的は、ASP.NET MVC ビュー、データの表示、および HTML ヘルパーの概要を説明します。 このチュートリアルの目的は、新しいビューを作成、コント ローラーからビューにデータを渡すし、HTML ヘルパーを使用して、ビューのコンテンツを生成する方法を理解する必要があります。

## <a name="understanding-views"></a>ビューについて

ASP.NET または Active Server Pages とは異なり ASP.NET MVC は含まれませんページに直接対応しているもの。 ASP.NET MVC アプリケーションであるページではありません、ブラウザーのアドレス バーに入力した URL のパスに対応するディスク上。 ASP.NET MVC アプリケーションのページに最も近いものは何かと呼ばれる、*ビュー*です。

ASP.NET MVC アプリケーションで受信ブラウザーの要求は、コント ローラーのアクションにマップされます。 コント ローラーのアクションは、ビューを返す可能性があります。 ただし、コント ローラーのアクションは、他のなんらかのアクションを別のコント ローラー アクションへのリダイレクトなどを実行する可能性があります。

1 を一覧表示するには、という名前のテンプレートを使用する単純なコント ローラーが含まれます。 HomeController は、Index() および Details() という 2 つのコント ローラーのアクションを公開します。

**1 - HomeController.vb を一覧表示します。**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

ブラウザーのアドレス バーに次の URL を入力して、最初のアクションで Index() アクションを呼び出すことができます。

/Home/index

お使いのブラウザーにこのアドレスを入力して、2 つ目の操作を Details() アクションを呼び出すことができます。

/ホーム/詳細

Index() アクションは、ビューを返します。 ほとんどのアクションを作成するには、ビューが返されます。 ただし、アクションは、他の種類のアクションの結果を返すことができます。 たとえば、Details() アクションには、Index() アクションに着信要求をリダイレクトする RedirectToActionResult が返されます。

Index() アクションには、次の 1 行コードにはが含まれています。

View()

次のコード行には、web サーバーで次のパスに配置する必要があるビューが返されます。

\Views\Home\Index.aspx

ビューへのパスは、コント ローラーの名前とコント ローラー アクションの名前から推測されます。

場合は、ビューの明示的に指定できます。 次のコード行には、Fred をという名前のビューが返されます。

表示 (Fred)

次のコード行が実行されると、次のパスからビューが返されます。

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> ASP.NET MVC アプリケーションの単体テストを作成する場合は、ビュー名は明示的に指定することをお勧めです。 このように、必要なビューがコント ローラーのアクションによって返されることを確認する単体テストを作成できます。


## <a name="adding-content-to-a-view"></a>コンテンツは、ビューを追加します。

ビューは、標準の (X) スクリプトを含めることができる HTML ドキュメントです。 ビューに動的なコンテンツを追加するのにには、スクリプトを使用します。

たとえば、2 の一覧表示するビューには、現在の日付と時刻が表示されます。

**2 - を一覧表示する \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

2 を一覧表示する HTML ページの本文に次のスクリプトが含まれていることを確認します。

&lt;Response.Write(DateTime.Now) %&gt;

スクリプトの区切り記号を使用する&lt;% と %&gt;先頭とスクリプトの末尾をマークします。 このスクリプトは、Visual basic で記述されました。 ブラウザーにコンテンツを表示するために Response.Write() メソッドを呼び出して、現在の日付と時刻を表示します。 スクリプトの区切り記号&lt;% と %&gt; 1 つまたは複数のステートメントを実行するために使用できます。

Response.Write() を頻繁に呼び出すため Microsoft が提供ショートカット Response.Write() メソッドを呼び出すためです。 ビューを一覧表示する 3 で、区切り記号を使用して&lt;% = %&gt; Response.Write() を呼び出すためのショートカットとして。

**3 - Views\Home\Index2.aspx を一覧表示します。**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

ビューでの動的なコンテンツを生成するのに任意の .NET 言語を使用することができます。 通常、する ll を使用して Visual Basic .NET や C# の場合、コント ローラーとビューを記述します。

## <a name="using-html-helpers-to-generate-view-content"></a>HTML ヘルパーを使用して、コンテンツの表示を生成するには

コンテンツ ビューを追加する容易にできるようにを利用すると呼ばれるものの*HTML ヘルパー*です。 通常は、HTML ヘルパーの場合は、文字列を生成する方法です。 HTML ヘルパーを使用して、テキスト ボックス、リンク、ドロップダウン リスト、およびリスト ボックスなどの標準の HTML 要素を生成することができます。

例については、ビューを一覧表示する 4 - 3 つの HTML ヘルパーの利用には、ログインを生成する--BeginForm() TextBox()、Password() ヘルパーは、(図 1 を参照してください) を形成します。

**リスト 4--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


[![[新しいプロジェクト] ダイアログ ボックス](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)

**図 01**: 標準的なログイン フォーム ([フルサイズのイメージを表示するをクリックして](asp-net-mvc-views-overview-vb/_static/image2.png))


ビューの Html プロパティでは、すべての HTML ヘルパー メソッドが呼び出されます。 たとえば、テキスト ボックスをレンダリングする Html.TextBox() メソッドを呼び出すことです。

スクリプトの区切り記号を使用することに注意してください&lt;% = %&gt; Html.TextBox() と Html.Password() の両方のヘルパーを呼び出すときにします。 これらのヘルパーは、単に文字列を返します。 ブラウザーに文字列を表示するために Response.Write() を呼び出す必要があります。

HTML ヘルパー メソッドを使用することはオプションです。 楽に、HTML および記述する必要があるスクリプトの量を減らすことによってです。 ビューを一覧表示する 5 には、HTML ヘルパーを使用せず、正確なと同じ形式を一覧表示する 4 でビューをレンダリングします。

**5 - を一覧表示する \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

また、独自の HTML ヘルパーの作成オプションがあります。 たとえば、HTML テーブルの一連のデータベース レコードを自動的に表示する GridView() ヘルパー メソッドを作成することができます。 チュートリアルのこのトピックの内容をナビゲート**カスタム HTML ヘルパーの作成**です。

## <a name="using-view-data-to-pass-data-to-a-view"></a>データの表示を使用して、ビューにデータを渡す

データの表示を使用して、コント ローラーからビューにデータを渡します。 電子メールで送信するパッケージと同様にデータの表示と考えてください。 このパッケージを使用して、コント ローラーからビューに渡されるすべてのデータを送信する必要があります。 たとえば、リスト 6 でのコント ローラーは、データを表示するメッセージを追加します。

**6 - ProductController.vb を一覧表示します。**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

コント ローラー ViewData プロパティは、名前と値のペアのコレクションを表します。 6 の一覧表示するのには、Index() メソッドは、Hello World の値を持つメッセージをという名前のビュー データ コレクションに項目を追加しました。 ビューが Index() メソッドによって返されると、データの表示は自動的にビューに渡されます。

ビューを一覧表示する 7 には、ビュー データからのメッセージを取得し、ブラウザーにメッセージを表示します。

**7--を一覧表示する \Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

ビューを活用 Html.Encode() HTML ヘルパー メソッドのメッセージを表示するときに注意してください。 Html.Encode() HTML ヘルパーがなどの特殊文字をエンコード&lt;と&gt;に安全に web ページに表示される文字。 ユーザーが web サイトに送信するコンテンツをレンダリングするときに、JavaScript インジェクション攻撃を防ぐためにコンテンツをエンコードする必要があります。

(T ありませんおで作成されるため、メッセージ自身、ProductController、実際にメッセージをエンコードする必要があります。 ただしは常にメソッドを呼び出す Html.Encode() ビュー内のビュー データから取得されたコンテンツを表示するときに推奨します。)

リスト 7 は、単純な文字列メッセージをコント ローラーからビューに渡すデータの表示の利点をかかりました。 データの表示を使用して、他のビューにコント ローラーからのデータベース レコードのコレクションなどのデータ型を渡すことができますも。 たとえば、データベースのコレクションを渡す場合、ビューでは、製品のデータベース テーブルの内容を表示したい場合ビューでデータ記録されます。

また、コント ローラーからビューを厳密に型指定されたビュー データを渡すオプションがあります。 チュートリアルのこのトピックの内容をナビゲート**Understanding 厳密に型指定されたデータの表示やビュー**です。

## <a name="summary"></a>概要

このチュートリアルでは、ASP.NET MVC ビュー、データの表示、および HTML ヘルパーの概要を提供します。 最初のセクションでは、プロジェクトに新しいビューを追加する方法について学習しました。 ビューを特定のコント ローラーから呼び出すために適切なフォルダーに追加する必要がありますを学習しました。 次に、HTML ヘルパーのトピックを説明します。 HTML ヘルパーを使用すると、標準の HTML コンテンツを簡単に生成する方法を学習しました。 最後に、コント ローラーからビューにデータを渡すデータの表示を活用する方法を学習します。

>[!div class="step-by-step"]
[前へ](passing-data-to-view-master-pages-cs.md)
[次へ](creating-custom-html-helpers-vb.md)
