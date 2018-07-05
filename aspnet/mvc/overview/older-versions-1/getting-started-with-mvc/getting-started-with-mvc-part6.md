---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: 追加するメソッドを作成し、ビューを作成する |Microsoft Docs
author: shanselman
description: これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 976df78ea22c30c094f70a57792d287f15d2c62d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400909"
---
<a name="adding-a-create-method-and-create-view"></a>追加するメソッドを作成し、ビューを作成します。
====================
によって[Scott Hanselman](https://github.com/shanselman)

> これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルとサンプルは、その他の ASP.NET MVC を検索します。


このセクションでは、データベースに新しいムービーを作成するユーザーを有効にするために必要なサポートを実装するでしょう。 映画/作成 URL 動作を実装することでこれを実行します。

映画/作成 URL の実装は、2 ステップのプロセスです。 場合ユーザーに最初のムービーの作成/URL にアクセスすることに、新しいムービーの入力に記入する HTML フォームに表示します。 次に、ユーザーは、フォームと、データをサーバーに投稿を送信するときにするポストされた内容を取得し、データベースに保存します。

2 つの Create() メソッド内でこれら 2 つの手順、MoviesController クラス内で実装します。 表示する 1 つのメソッドは、&lt;フォーム&gt;ユーザーが新しいムービーの作成に記入する必要があります。 2 番目のメソッドは、ユーザーが送信するときに、ポストされたデータの処理を処理する、&lt;フォーム&gt;サーバーに戻すし、データベース内で新しいムービーを保存します。

以下のコードは、これを実装する MoviesController クラスに追加します。

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

上記のコードには、すべての必要があります、コント ローラー内でコードが含まれます。

使用して、フォームをユーザーに表示するビューの作成テンプレートを今すぐ実装してみましょう。 最初の Create メソッドを右クリックして、ムービー、フォーム ビュー テンプレートを作成する「ビューの追加」を選択します。

そのビューのデータ クラスと「ムービー」ビュー テンプレートを渡すことは、テンプレートの「作成」を「スキャフォールディング」することを示すことを選択します。

[![ビューを追加します。](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

[追加] ボタンをクリックした後 \Movies\Create.aspx ビュー テンプレートが作成されます。 "コンテンツの表示 ドロップダウン リストから 作成 を選択したため、ビューの追加 ダイアログに自動的に「スキャフォールディングされた」既定のコンテンツをします。 スキャフォールディングが作成、HTML&lt;フォーム&gt;進むには、メッセージの検証エラーの場所で、クラスの各プロパティのラベルとフィールドを作成して映画のスキャフォールディングを知っている、ため、します。

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

データベースでは、ID、ムービーが自動的には、みましょう削除してそれらのフィールド参照モデル。ビューを作成するの id。 後の 7 行を削除&lt;凡例&gt;フィールド&lt;/legend&gt;されないようにするため、ID フィールドを表示するようにします。

みましょう今すぐ新しいムービーを作成し、データベースに追加します。 これには、アプリケーションを再度実行しているを参照してくださいし、"/映画"「作成」が、新しいムービーを追加するリンクの URL をクリックします。

[![Windows Internet Explorer の作成-](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

[作成] ボタンをクリックしてしましたと版もご利用戻る (HTTP POST 経由) を作成した/Movies/Create メソッドには、このフォーム上のデータ。 ときに、システムは自動的に URL から"numTimes"および"name"パラメーターを要したし、前のメソッドのパラメーターにマップすると同様、システムが自動的に投稿から、フォームのフィールドを取得、オブジェクトにマップされます。 この場合は、"ReleaseDate"や"Title"のような HTML のフィールドの値は、自動的に新しいムービーのインスタンスの適切なプロパティに格納されます。

2 番目の作成方法、MoviesController からもう一度見てみましょう。 「ビデオ」オブジェクトを引数としてかかる方法に注意してください。

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

このムービー オブジェクトは、Create アクション メソッドの [HttpPost] バージョンに渡されたし、データベースに保存し、ムービーの一覧で、保存された結果を表示する Index() アクション メソッドに、ユーザーがリダイレクトされます。

[![ムービーの一覧 - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

私たちは、ムービーが正しいこと、ただし、そのデータベースでタイトルなしでムービーを保存することは許可されませんにチェックインしていません。 私たちの知るユーザー データベースの前にエラーをスローする場合は便利になります。 検証のサポートをアプリケーションに追加することで、次を実行します。

> [!div class="step-by-step"]
> [前へ](getting-started-with-mvc-part5.md)
> [次へ](getting-started-with-mvc-part7.md)
