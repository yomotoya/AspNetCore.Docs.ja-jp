---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: "モデルの検証の追加 |Microsoft ドキュメント"
author: shanselman
description: "これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 5616c3c3bc77be0a770540d04cc2ae48ba9eedff
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
<a name="adding-validation-to-the-model"></a>モデルの検証の追加
====================
によって[Scott Hanselman](https://github.com/shanselman)

> これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みを単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。


このセクションで行う、アプリケーション内で入力検証を有効にするために必要なサポートを実装します。 おすることを確認、データベースの内容が正しいこと、常を再試行してください、無効なムービー データを入力すると、エンドユーザーに有用なエラー メッセージを提供します。 まず、ムービー クラスに少しの検証ロジックを追加します。

モデル フォルダーを右クリックし、クラスの追加を選択します。 ムービー、クラスの名前を付けます。

ムービー Entity Model を以前に作成したときに、IDE はムービー クラスを作成します。 実際には、ムービー クラスの一部は、1 つのファイルおよび他の部分で指定できます。 これは、部分クラスで呼び出されます。 別のファイルからムービー クラスを拡張しようとしています。

システムに検証のヒントを与えるいくつかの属性を持つムービーの部分クラスを指す「関連クラス」を作成します。 タイトルと価格を必須とマークを付けるうまくし、価格が特定の範囲内にあることを要求します。 モデル フォルダーを右クリックし、クラスの追加を選択します。 ムービー、クラスの名前し、[ok] ボタンをクリックします。 ここではどのような部分的なムービー クラスのようになります。

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

アプリケーションを再実行して、価格が 100 以上とするムービーを入力しようとしてください。 フォームを送信した後にエラーが表示されます。 エラーは、サーバー側で検出され、フォームを送信した後に発生します。 ASP.NET MVC の組み込み HTML ヘルパーがあるため、エラー メッセージが表示され、ご利用の米国 textbox 要素内の値を維持する方法に注意してください。

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

これはとても便利が、できれば便利なことが通知され、ユーザー、クライアント側で、すぐに、サーバーが関与する前にします。

JavaScript でのいくつかのクライアント側検証を有効にしてみましょう。

## <a name="adding-client-side-validation"></a>クライアント側の検証を追加します。

ムービー クラスは、いくつかの検証属性を既に持っているのでおするだけで済みます、Create.aspx ビュー テンプレートに、いくつかの JavaScript ファイルを追加しを実行するクライアント側の検証を有効にするコードの行を追加します。

内からは、VWD はムービー ビュー/フォルダーを移動し、Create.aspx を開きます。

ソリューション エクスプ ローラーで、Scripts フォルダーを開きに次の 3 つのスクリプト内でドラッグして、&lt;ヘッド&gt;タグ。

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

これらのスクリプト ファイルをこの順序で表示します。

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

また、この単一行、Html.BeginForm 上を追加します。

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

IDE 内でのコードを次に示します。

[![映画 - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

アプリケーションを実行し、/Movies/Create をもう一度を参照してください、すべてのデータを入力することがなく作成 をクリックします。 エラー メッセージを表示すぐにデータの送信と関連付けることができましたフラッシュ ページなしサーバーに至るまでさかのぼってです。 これはため、ASP.NET MVC は今すぐ入力を検証する両方で JavaScript を使用して)、クライアントとサーバーです。

[![Windows Internet Explorer の作成-](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

これは、適切なが探して! データベースに 1 つの列の追加を今すぐ追加してみましょう。

>[!div class="step-by-step"]
[前へ](getting-started-with-mvc-part6.md)
[次へ](getting-started-with-mvc-part8.md)
