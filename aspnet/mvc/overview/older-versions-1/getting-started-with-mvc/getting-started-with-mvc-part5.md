---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: コント ローラーから、モデルのデータにアクセス |Microsoft ドキュメント
author: shanselman
description: これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
ms.locfileid: "30876437"
---
<a name="accessing-your-models-data-from-a-controller"></a>コント ローラーから、モデルのデータにアクセスします。
====================
によって[Scott Hanselman](https://github.com/shanselman)

> これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みを単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。


このセクションの内容はここを新しい MoviesController クラスを作成し、ムービー データを取得し、ビュー テンプレートを使用してブラウザーに表示するいくつかのコードを記述します。

Controllers フォルダーを右クリックし、新しい MoviesController を作成します。

[![コント ローラーを追加します。](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

これにより、プロジェクト内で当社 \Controllers フォルダーの下に新しい"MoviesController.cs"ファイルが作成されます。 新しく作成されたデータベースからムービーの一覧を取得する MovieController を更新してみましょう。

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

お 1984 年の夏の後に発売された映画のみ取得されるように、LINQ クエリを実行します。 映画に戻るには、この一覧を表示、そこでメソッドで右クリックし、それを作成する追加のビューを選択するビュー テンプレートが必要です。

ビューの追加 ダイアログ ボックスでおを示す一覧を渡している&lt;Movies.Models.Movie&gt;テンプレートの表示にします。 前の時間ビューの追加 ダイアログを使用して、"Empty"のテンプレートを作成することを選択とは異なりおすることを示すため、この時点で作成する Visual Studio で自動的に「スキャフォールディング」ビュー テンプレートの既定のコンテンツを。 それでは、"ビューのコンテンツのドロップダウン メニュー内の"List"項目を選択しています。

ただしがある場合、作成された新しいクラスをビューの追加 ダイアログ表示するのには、アプリケーションをコンパイルする必要があります。

![ビューを追加します。](getting-started-with-mvc-part5/_static/image3.png)

追加 をクリックし、システムはムービーの一覧を表示するうえでビューのコードを自動的に生成されます。 これは、変更する絶好のタイミング、 &lt;h2&gt;前、Hello World ビューと同じように、「マイ ムービー リスト」のようなものに見出し。

[![映画 - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

アプリケーションを実行し、アドレス バーに/Movies を参照してください。 これで、コント ローラー内の基本的なクエリを使用して、データベースからデータを取得および映画について認識しているビューにデータが返されますしました。 そのビューは、ムービーの一覧をループし、ご利用の米国のデータのテーブルを作成します。

[![ムービーの一覧 - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

おされませんを実装するこのアプリケーションの編集、詳細、および Delete の機能のためご利用の米国 scaffold テンプレートが作成された既定のリンクを必要はありません。 /Movies/Index.aspx ファイルを開き、それらを削除します。

次に、更新されたテンプレートの表示がどのようにしたら、これらの変更を行う場合のソース コードを示します。

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

お必要はありません、リンクの作成ため、この例に削除されます。 いきますの新規作成、次へ であるため! 新機能とその列は削除されて、アプリの外観を次に示します。

[![ムービーの一覧 - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

ムービー データの単純なリスト表示があるようになりました。 ただし、「新規作成」のリンクをクリックしておが表示されますエラー接続されていないようです。 作成アクション メソッドを実装し、ユーザーにデータベースに新しいムービーの入力を有効にしてみましょう。

> [!div class="step-by-step"]
> [前へ](getting-started-with-mvc-part4.md)
> [次へ](getting-started-with-mvc-part6.md)
