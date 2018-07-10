---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: ASP.NET MVC ビュー概要 (c#) |Microsoft Docs
author: StephenWalther
description: ASP.NET MVC ビューし、HTML ページから違う方法でしょうか。 このチュートリアルでは、Stephen Walther がビューを紹介し、t を行う方法を示します.
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: d2fc96f7e991dd7c4e0b3e9ff5c589c1075010ac
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833660"
---
<a name="aspnet-mvc-views-overview-c"></a>ASP.NET MVC ビュー概要 (c#)
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> ASP.NET MVC ビューし、HTML ページから違う方法でしょうか。 このチュートリアルでは、Stephen Walther がビューを紹介し、データの表示と HTML ヘルパーをビュー内の利用の実行方法を示します。


このチュートリアルの目的は、ASP.NET MVC ビュー、データの表示、および HTML ヘルパーの概要を提供するためにです。 このチュートリアルの目的は、新しいビューを作成、コント ローラーからビューにデータを渡すし、HTML ヘルパーを使用して、ビューでコンテンツを生成する方法を理解する必要があります。

## <a name="understanding-views"></a>ビューについて

ASP.NET または Active Server Pages、ASP.NET MVC は含まれません、ページに直接対応するものです。 ASP.NET MVC アプリケーションでページがあるないブラウザーのアドレス バーに入力した URL のパスに対応するディスク上。 ASP.NET MVC アプリケーションのページに最も近いものは何かと呼ばれる、*ビュー*します。

ASP.NET MVC はコント ローラー アクションへのアプリケーション、受信ブラウザー要求がマップされます。 コント ローラー アクションには、ビューを返す可能性があります。 ただし、コント ローラーのアクションは、他の種類を別のコント ローラー アクションへのリダイレクトなどのアクションを実行する場合があります。

1 を一覧表示するには、シンプルなコント ローラー、HomeController という名前が含まれます。 HomeController は Index() Details() という 2 つのコント ローラー アクションを公開します。

**1 - HomeController.cs を一覧表示します。**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

ブラウザーのアドレス バーに次の URL を入力して、最初のアクションで、Index() アクションを呼び出すことができます。

/ホーム/インデックス

お使いのブラウザーにこのアドレスを入力し、2 番目のアクション、Details() アクションを呼び出すことができます。

/ホーム/詳細

Index() アクションでは、ビューを返します。 ほとんどのアクションを作成するには、ビューが返されます。 ただし、アクションは、他の種類のアクションの結果を返すことができます。 たとえば、Details() アクションは、Index() アクションに受信要求をリダイレクトする RedirectToActionResult を返します。

Index() アクションには、次の 1 行コードにはが含まれています。

View();

次のコードでは、web サーバーで次のパスに配置する必要があるビューが返されます。

\Views\Home\Index.aspx

ビューへのパスは、コント ローラーの名前とコント ローラー アクションの名前から推測されます。

場合は、ビューの明示的に指定できます。 次のコード行には、Fred という名前のビューが返されます。

View( Fred );

次のコードが実行されたときに、次のパスからビューが返されます。

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> ASP.NET MVC アプリケーション用の単体テストを作成する場合は、ビューの名前について明示的に指定することをお勧めです。 これにより、必要なビューがコント ローラーのアクションによって返されたことを確認する単体テストを作成できます。


## <a name="adding-content-to-a-view"></a>コンテンツは、ビューを追加します。

ビューは、(X) スクリプトを含むことのできる HTML ドキュメントの標準です。 スクリプトを使用して、動的なコンテンツをビューに追加します。

たとえば、リスト 2 でのビューには、現在の日付と時刻が表示されます。

**リスト 2 - \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

リスト 2 で HTML ページの本文が次のスクリプトを含むことに注意してください。

&lt;%Response.write(datetime.now); %&gt;

スクリプトの区切り記号を使用する&lt;% と %&gt;先頭とスクリプトの末尾をマークします。 このスクリプトは、c# で記述されます。 ブラウザーにコンテンツをレンダリングする Response.Write() メソッドを呼び出して、現在の日付と時刻を表示します。 スクリプトの区切り記号&lt;% と %&gt; 1 つまたは複数のステートメントを実行するために使用できます。

Response.Write() を呼び出すことが多いためので、Microsoft でショートカット Response.Write() メソッドを呼び出すため。 リスト 3 のビューで使用する区切り記号&lt;% =、%&gt; Response.Write() を呼び出すためのショートカットとして。

**3 - Views\Home\Index2.aspx を一覧表示します。**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

任意の .NET 言語を使用して、ビューでの動的なコンテンツを生成することができます。 通常、読んだ後を使用して、Visual Basic .NET または c# のいずれか、コント ローラーとビューを記述します。

## <a name="using-html-helpers-to-generate-view-content"></a>HTML ヘルパーを使用して、コンテンツの表示を生成するには

コンテンツ ビューを追加する容易にできるようにと呼ばれるものの利用を行う、 *HTML ヘルパー*します。 通常、HTML ヘルパーは、文字列を生成する方法です。 HTML ヘルパーを使用して、テキスト ボックス、リンク、ドロップダウン リスト、リスト ボックスなどの標準の HTML 要素を生成することができます。

例では、リスト 4 - 3 つの HTML ヘルパーを利用で、ビューのログインを生成する--BeginForm()、TextBox() および Password() ヘルパーは、(図 1 参照) を形成します。

**Listing 4 -- \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


[![[新しいプロジェクト] ダイアログ ボックス](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**図 01**: 標準的なログイン フォーム ([フルサイズの画像を表示する をクリックします](asp-net-mvc-views-overview-cs/_static/image2.png))。


ビューの Html プロパティでは、すべての HTML ヘルパー メソッドが呼び出されます。 たとえば、テキスト ボックスを表示するには Html.TextBox() メソッドを呼び出してなります。

スクリプトの区切り記号を使用することに注意してください&lt;% =、%&gt; Html.TextBox() と Html.Password() ヘルパーを呼び出すときにします。 これらのヘルパーは、単に文字列を返します。 Response.Write() をブラウザーに文字列を表示するために呼び出す必要があります。

HTML ヘルパー メソッドを使用することは省略可能です。 生活を楽 HTML とスクリプトを記述する必要があるの量を減らすことで。 ビュー リスト 5 では、HTML ヘルパーを使用せず、リスト 4 ビューとまったく同じ形式をレンダリングします。

**Listing 5 -- \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

また、独自の HTML ヘルパーの作成オプションがあります。 たとえば、HTML テーブルの一連のデータベース レコードを自動的に表示する GridView() ヘルパー メソッドを作成できます。 このトピックで、このチュートリアルで説明**カスタム HTML ヘルパーの作成**です。

## <a name="using-view-data-to-pass-data-to-a-view"></a>データの表示を使用して、ビューにデータを渡す

データの表示を使用して、コント ローラーからビューにデータを渡します。 電子メールで送信するパッケージと同様にデータの表示と考えてください。 このパッケージを使用して、コント ローラーからビューに渡されるすべてのデータを送信する必要があります。 たとえば、6 の一覧で、コント ローラーは、データを表示するメッセージを追加します。

**6 - ProductController.cs を一覧表示します。**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

コント ローラーの ViewData プロパティは、名前と値のペアのコレクションを表します。 6 の一覧表示するのには、Index() メソッドは、値 Hello World メッセージをという名前のビュー データ コレクションに項目を追加しました。 ビューが Index() メソッドによって返されると、データの表示は自動的に、ビューに渡されます。

リスト 7 でのビューでは、データの表示から、メッセージを取得し、ブラウザーにメッセージをレンダリングします。

**リスト 7.--\Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

Html.Encode() HTML ヘルパーの方法の利点をメッセージを表示するときに、表示にかかることに注意してください。 Html.Encode() の HTML ヘルパーなどの特殊文字をエンコード&lt;と&gt;に安全に web ページに表示される文字。 ユーザーが web サイトに送信されるコンテンツをレンダリングするときに、JavaScript インジェクション攻撃を防ぐためにコンテンツをエンコードする必要があります。

(T ない作成したので、メッセージ自分たちの「productcontroller」で、実際にメッセージをエンコードする必要があります。 ビュー内のデータをビューから取得したコンテンツを表示するときに常に Html.Encode() メソッドを呼び出すことを推奨します。)

リスト 7 では、単純な文字列メッセージをコント ローラーからビューに渡すデータの表示の利点をしました。 データの表示を使用して、他の種類のビュー コント ローラーからのデータベース レコードのコレクションなどのデータを渡すことができますも。 たとえば、データベースのコレクションを渡す場合、ビューでは、製品のデータベース テーブルの内容を表示したい場合表示データ記録します。

また、コント ローラーからビューに厳密に型指定されたビュー データを渡すことがあります。 このトピックで、このチュートリアルで説明**について厳密に型指定されたデータの表示とビュー**します。

## <a name="summary"></a>まとめ

このチュートリアルでは、ASP.NET MVC ビュー、データの表示、および HTML ヘルパーの概要を提供します。 最初のセクションでは、プロジェクトに新しいビューを追加する方法について説明しました。 ビューを特定のコント ローラーから呼び出すために適切なフォルダーに追加する必要がありますを学習しました。 次に、HTML ヘルパーのトピックを説明します。 HTML ヘルパーを使用すると、標準の HTML コンテンツを簡単に生成する方法を学習しました。 最後に、コント ローラーからビューにデータを渡すデータの表示を活用する方法を学習しました。

> [!div class="step-by-step"]
> [次へ](creating-custom-html-helpers-cs.md)
