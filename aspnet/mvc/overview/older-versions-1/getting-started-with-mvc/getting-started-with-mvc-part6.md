---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: 追加するメソッドを作成し、ビューを作成 |Microsoft ドキュメント
author: shanselman
description: これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 48e656a0c394b9db5baaec9c557ec38c4020d41b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-create-method-and-create-view"></a>追加するメソッドを作成し、ビューを作成します。
====================
によって[Scott Hanselman](https://github.com/shanselman)

> これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みを単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。


このセクションで行う、データベースに新しいムービーを作成するユーザーを有効にするために必要なサポートを実装します。 映画/作成 URL アクションを実装することによってこれを実行します。

/作成の映画の URL を実装することは、次の 2 ステップ プロセスです。 ユーザーはまず/作成の映画の URL を訪問したときに、新しいムービーの入力を記入できる HTML フォームに表示するとします。 次に、ユーザーは、フォームや、データをサーバーに戻すの投稿を送信するときにポストされた内容を取得し、データベース内に保存します。

MoviesController クラス内で 2 つの Create() メソッド内でこれら 2 つの手順を実装します。 表示する 1 つのメソッドは、&lt;フォーム&gt;ユーザーが新しいムービーの作成に記入する必要があります。 2 番目のメソッドは、ユーザーが送信するときに、ポストされたデータの処理を処理する、&lt;フォーム&gt;サーバーにバックアップし、データベース内で新しいムービーを保存します。

以下のコードは、これを実装する、MoviesController クラスに追加されます。

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

上記のコードには、すべてのしなければならない、コント ローラー内でコードが含まれます。

使用して、ユーザーに、フォームを表示するビューの作成テンプレートを実装してみましょう。 おによってを最初の Create メソッドを右クリックし、ムービー フォーム用のビュー テンプレートを作成する「ビューの追加」を選択します。

おしようとしてビュー テンプレート「ムービー」を渡すと、そのビューのデータ クラスとは、「作成」テンプレートを「スキャフォールディング」することを指定することを選択します。

[![ビューを追加します。](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

[追加] ボタンをクリックすると、\Movies\Create.aspx ビュー テンプレートが作成されます。 "コンテンツを表示 ドロップダウン リストから、作成 を選択すると、ためビューの追加 ダイアログに自動的に「スキャフォールディングされました」既定のコンテンツをいくつかご利用の米国。 スキャフォールディング作成 HTML&lt;フォーム&gt;、進むには、メッセージの検証エラーの場所で、クラスの各プロパティのラベルとフィールドを作成してムービーのスキャフォールディングを認識、ためです。

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

データベースに自動的に利用できますが、ムービー、ID、ためみましょうモデルは削除それらのフィールドを参照。このビューを作成するの id。 削除後に 7 行&lt;凡例&gt;フィールド&lt;/legend&gt;されないようにするため、ID フィールドが表示されます。

みましょう今すぐ新しいムービーを作成し、データベースに追加します。 これには、アプリケーションを再実行している、うまくし、参照してください、、"/映画"「作成」に新しいビデオを追加するリンクの URL をクリックします。

[![Create - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

作成ボタンをクリックしておある投稿するときに戻る (HTTP POST) を介してこのフォームを作成したばかり/Movies/Create メソッド上のデータ。 まったく同様にシステムに自動的に URL から"numTimes"と"name"パラメーターを要したし、前のメソッドのパラメーターにマップして、システムは自動的に投稿からはフォームのフィールドを取得してオブジェクトにマップします。 この場合、"ReleaseDate"や"Title"のような HTML のフィールドの値は、ムービーの新しいインスタンスの正しいプロパティを自動的に配置されます。

2 番目の作成方法、MoviesController からもう一度見てみましょう。 「ムービー」オブジェクトを引数としてかかる方法に注意してください。

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

このムービー オブジェクトが、作成するアクション メソッドの [HttpPost] バージョンに渡された、およびデータベースに保存し、ムービーの一覧で、保存された結果を表示する Index() アクション メソッドに、ユーザーをリダイレクトします。

[![ムービーの一覧]-[Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

おチェックされないかどうか、映画が正しくただし、および、データベースがタイトルなしでムービーを保存することを許可しません。 説明し、ユーザー データベースの前に、エラーをスローした場合は便利になります。 アプリケーションに検証のサポートを追加することで、次を実行します。

> [!div class="step-by-step"]
> [前へ](getting-started-with-mvc-part5.md)
> [次へ](getting-started-with-mvc-part7.md)
