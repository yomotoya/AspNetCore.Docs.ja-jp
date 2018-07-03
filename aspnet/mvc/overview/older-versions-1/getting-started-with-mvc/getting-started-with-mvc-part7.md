---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: モデル検証の追加 |Microsoft Docs
author: shanselman
description: これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 1a8c186d5a6b00aaf1061bb4025f4f062203a7df
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402982"
---
<a name="adding-validation-to-the-model"></a>モデル検証の追加
====================
によって[Scott Hanselman](https://github.com/shanselman)

> これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルとサンプルは、その他の ASP.NET MVC を検索します。


このセクションでは、アプリケーション内での入力の検証を有効にするために必要なサポートを実装するでしょう。 しますが、データベースの内容が正しい常として無効なムービー データを入力するときに役に立つエラー メッセージをエンドユーザーに提供できます。 まず、ムービー クラスに少しの検証ロジックを追加します。

モデル フォルダーを右クリックし、クラスの追加を選択します。 ムービー、クラスの名前を付けます。

前に、ムービー エンティティ モデルを作成したときに、IDE はムービー クラスを作成します。 実際には、ムービー クラスの一部は、1 つのファイルおよび別の部分で指定できます。 これは、部分クラスと呼ばれます。 別のファイルからムービー クラスを拡張しようとしています。

システムに検証のヒントを与えるいくつかの属性を持つムービーの部分クラスを指す「buddy クラス」を作成します。 タイトルと、必要に応じて、価格のマークがされも、価格が特定の範囲内にあると主張します。 Models フォルダーを右クリックし、クラスの追加を選択します。 ムービー、クラスの名前し、[ok] ボタンをクリックします。 どのような部分的なムービー クラスのようになります次に示します。

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

アプリケーションを再実行して、価格が 100 を超えるビデオを入力しようとしてください。 フォームを送信した後、エラーが発生します。 エラーは、サーバー側で検出され、フォームが投稿された後に発生します。 ASP.NET MVC の組み込み HTML ヘルパーが、エラー メッセージが表示され、テキスト ボックス要素内の値を維持する方法に注意してください。

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

すばらしく、機能しますが、できれば言う場合は、私たちの知る、ユーザー、クライアント側ですぐに、サーバーが関与する前にします。

JavaScript でいくつかのクライアント側検証を有効にしてみましょう。

## <a name="adding-client-side-validation"></a>クライアント側の検証を追加します。

ムービー クラスは、いくつかの検証属性を既に持っているのでだけ Create.aspx ビュー テンプレートにいくつかの JavaScript ファイルを追加し、実行するクライアント側の検証を有効にするコード行を追加する必要あります。

内からは、VWD は、ムービー ビュー/フォルダーを移動し、Create.aspx を開きます。

ソリューション エクスプ ローラーで、Scripts フォルダーを開きに次の 3 つのスクリプト内でのドラッグ、&lt;ヘッド&gt;タグ。

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

これらのスクリプト ファイルをこの順序で表示します。

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

また、この 1 行、Html.BeginForm 上を追加します。

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

IDE 内でのコードを次に示します。

[![ビデオ - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

アプリケーションを実行し、/Movies/Create をもう一度、アクセスしすべてのデータを入力しなくても作成 をクリックします。 エラー メッセージを表示すぐに、ページのデータの送信に関連付けられたことをフラッシュせず、サーバーに至るまでさかのぼって。 これは ASP.NET MVC は、両方の入力の (JavaScript を使用して) クライアントを検証ようになりましたがあるため、サーバー上です。

[![Windows Internet Explorer の作成-](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

これは順調です! データベースに 1 つの列の追加を今すぐ追加してみましょう。

> [!div class="step-by-step"]
> [前へ](getting-started-with-mvc-part6.md)
> [次へ](getting-started-with-mvc-part8.md)
