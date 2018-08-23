---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: コント ローラーからモデルのデータへのアクセス |Microsoft Docs
author: shanselman
description: これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 76dc324134dc93c9552741fea9f1136abdc9184a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831361"
---
<a name="accessing-your-models-data-from-a-controller"></a>コント ローラーからモデルのデータにアクセスします。
====================
によって[Scott Hanselman](https://github.com/shanselman)

> これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルとサンプルは、その他の ASP.NET MVC を検索します。


このセクションでは、新しい MoviesController クラスを作成し、映画のデータを取得し、ビュー テンプレートを使用して、ブラウザーに表示するコードを作成するでしょう。

Controllers フォルダーを右クリックし、新しい MoviesController を作成します。

[![コント ローラーを追加します。](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

これにより、プロジェクト内で、\Controllers フォルダーの下に新しい"MoviesController.cs"ファイルが作成されます。 新しく設定されたデータベースからムービーの一覧を取得する MovieController を更新してみましょう。

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

1984 年の夏の後にリリースされた映画のみ取得できるように LINQ クエリを実行しています。 ビュー テンプレートの映画に戻るには、この一覧を表示、そこでメソッドで右クリックし、それを作成するには、ビューの追加を選択する必要があります。

ビューの追加 ダイアログ ボックスで指定するリストを渡している&lt;Movies.Models.Movie&gt;テンプレートの表示にします。 前の時間ビューの追加 ダイアログを使用して、「空」のテンプレートを作成することを選択とは異なりこの時間を指定する Visual Studio で自動的に「スキャフォールディング」ビュー テンプレートの既定のコンテンツとします。 "ビューのコンテンツのドロップダウン メニュー内の"List"項目を選択してこれは。

ただしがある場合、作成された新しいクラス ビューの追加 ダイアログに表示するアプリケーションをコンパイルする必要があります。

![ビューを追加します。](getting-started-with-mvc-part5/_static/image3.png)

追加 をクリックし、システムはムービーの一覧を表示するためビューのコードを自動的に生成されます。 これは、変更すると良い、 &lt;h2&gt; Hello World ビューで以前に行ったように"マイ Movie List"のように向かっています。

[![ビデオ - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

アプリケーションを実行し、アドレス バーに/Movies を参照してください。 ここで、コント ローラー内での基本的なクエリを使用してデータベースからデータを取得したし、映画について認識しているビューにデータが返されます。 そのビューは、ムービーのリストをループ処理し、私たちのデータのテーブルを作成します。

[![ムービーの一覧 - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

私たちはありませんする機能を実装する編集、詳細、削除 - このアプリケーションでのスキャフォールディング テンプレートが作成された既定のリンクしない必要があります。 /Movies/Index.aspx ファイルを開き、それらを削除します。

次に、更新されたテンプレートの表示がどのようにこれらの変更を行った後のソース コードを示します。

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

この例には削除されますので必要はありませんが、あるリンクを作成します。 次へ を、新規作成 が保持されます。 削除する列で、アプリがどのようにを次に示します。

[![ムービーの一覧 - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

ムービー データの単純なリストがあるようになりました。 ただし、"新規作成 リンクをクリックすると場合、そのように接続されていないエラーが発生しましたします! 作成アクション メソッドを実装して、データベースに新しい映画を入力するユーザーを有効にします。

> [!div class="step-by-step"]
> [前へ](getting-started-with-mvc-part4.md)
> [次へ](getting-started-with-mvc-part6.md)
