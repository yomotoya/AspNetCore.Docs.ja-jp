---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: ASP.NET Web Pages の概要 - データベースのデータの削除 |Microsoft Docs
author: tfitzmac
description: このチュートリアルでは、個々 のデータベース エントリを削除する方法を示します。 ASP.NET Web Pa. 内のデータベース データの更新をシリーズを完了したと想定して.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 45cd3ed7fdcede05823ef28d7cc6c8da3922dad7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362091"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>ASP.NET Web ページの概要 - データベースのデータを削除します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルでは、個々 のデータベース エントリを削除する方法を示します。 を通じてシリーズを完了したと想定して[データベースのデータを更新する ASP.NET Web Pages で](updating-data.md)します。
> 
> 学習内容。
> 
> - レコードの一覧から個々 のレコードを選択する方法。
> - データベースから 1 つのレコードを削除する方法。
> - フォームで、特定のボタンがクリックしてされたことを確認する方法。
>   
> 
> 説明した機能/テクノロジ:
> 
> - `WebGrid`ヘルパー。
> - SQL`Delete`コマンド。
> - `Database.Execute` SQL を実行するメソッドを`Delete`コマンド。


## <a name="what-youll-build"></a>構築します

前のチュートリアルでは、既存のデータベース レコードを更新する方法について説明しました。 このチュートリアルは、似ていますが、レコードを更新するには、代わりに、それを削除します。 プロセスは、このチュートリアルは、短縮されるためにより単純に、削除がある点がほぼ同じです。

*映画*を更新します ページで、`WebGrid`ヘルパーを表示できるように、**削除**に付属する各ムービーの横にあるリンク、**編集**先ほど追加したリンク。

![各ムービーの削除 リンクを示すビデオ ページ](deleting-data/_static/image1.png)

編集をクリックすると、**削除**リンクに移動する別のページでムービー情報が既にフォームで。

![ムービー表示ビデオ ページを削除します。](deleting-data/_static/image2.png)

レコードを完全に削除するボタンをクリックできます。

## <a name="adding-a-delete-link-to-the-movie-listing"></a>ムービーの一覧に、[削除] リンクを追加します。

追加することから始めます、**削除**へのリンク、`WebGrid`ヘルパー。 このリンクと似ています、**編集**リンクが前のチュートリアルで追加されました。

開く、 *Movies.cshtml*ファイル。

変更、`WebGrid`列を追加してページの本文内のマークアップ。 変更後のマークアップを次に示します。

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

新しい列では、この 1 つです。

[!code-html[Main](deleting-data/samples/sample2.html)]

グリッドの構成方法、**編集**列がグリッドの左端と**削除**列は最も右にあります。 (コンマの後に、`Year`列、いることを確認していない場合)。これらのリンク列の移動先に関する特別なものを使用する必要があるし、互いの横にあるように簡単に配置する可能性があります。 この場合は、混同を取得するは困難にする別です。

![編集と詳細のリンクを含む動画ページのマークを表示して相互の横にあるいません。](deleting-data/_static/image3.png)

新しい列には、リンクが表示されます (`<a>`要素)"Delete"と表示されました。 リンクのターゲット (その`href`属性) で、この URL のように最終的に解決されるコードは、`id`各ムービーのさまざまな値。

[!code-css[Main](deleting-data/samples/sample3.css)]

このリンクをという名前のページを呼び出す*DeleteMovie*を選択したムービーの ID を渡します。

このチュートリアルには触れませんが、このリンクの構築方法の詳細とほぼ同じである、**編集**、前のチュートリアルからのリンク ([データベースのデータを更新する ASP.NET Web Pages で](updating-data.md))。

## <a name="creating-the-delete-page"></a>Delete ページの作成

対象となるページを作成できるようになりました、**削除**グリッドのリンク。

> [!NOTE] 
> 
> **重要な**最初を削除するレコードを選択して、プロセスを確認する別のページとボタンを使用しての手法はセキュリティのきわめて重要です。 前のチュートリアルで読んだとを行う*任意*web サイトに変更する必要があります*常に*行うフォームを使用して&mdash;は、HTTP POST 操作を使用します。 した場合 (つまり、GET 操作を使用) のリンクをクリックするだけで、サイトを変更することも場合、人でした、サイトに対して単純な要求を行うし、データを削除します。 でも、サイトのインデックス作成は、検索エンジン クローラーでは、次のリンクをするだけでデータを誤って削除する可能性があります。
> 
> アプリでは、ユーザーがレコードを変更することができます、ときにも編集するため、ユーザーに、レコードを提示するがあります。 レコードを削除するのには、この手順を省略したくなる場合があります。 ただし、その手順をスキップするはありません。 (これはも、レコードをことを目的とするレコードを削除しようとしていることを確認します。 ユーザーに便利です)。
> 
> 後続のチュートリアル セットでは、ユーザーがレコードを削除する前にログインする必要がありますので、ログイン機能を追加する方法を確認します。


という名前のページを作成する*DeleteMovie.cshtml*と新機能を次のマークアップ ファイルで置換します。

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

このマークアップは似ています、 *EditMovie*テキスト ボックスを使用する代わりに、ページ (`<input type="text">`)、マークアップを含む`<span>`要素。 編集するのには、ここ何があります。 できるようにするため、適切なムービーを削除しようとしていることを確認して、映画の詳細を表示するだけです。

マークアップには、リンク、ムービーの一覧ページに戻り、ユーザーにはが既に含まれています。

*EditMovie*  ページで、選択したムービーの ID は非表示フィールドに格納されます。 (として渡されるページに、最初に、クエリ文字列値。)`Html.ValidationSummary`呼び出しする検証エラーが表示されます。 この場合、エラーは、ページにムービー ID が渡されなかったことや、映画 ID が無効である可能性があります。 このページでムービーを選択せずに実行だれかが場合に、このような状況が発生する可能性があります、*映画*ページ。

ボタンのキャプション**削除ムービー**、name 属性に設定し、 `buttonDelete`。 `name`属性は、フォームを送信するボタンを識別するために、コードで使用されます。

1) ページが初めて表示したとき、ムービーの詳細を確認するコードを記述し、ボタンをクリックすると、2) 実際には、ムービーを削除する必要があります。

## <a name="adding-code-to-read-a-single-movie"></a>1 つのムービーを読み取るコードを追加します。

上部にある、 *DeleteMovie.cshtml*ページで、次のコード ブロックを追加します。

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

このマークアップは、対応するコードと同じ、 *EditMovie*ページ。 クエリ文字列からムービー ID を取得し、ID を使用して、データベースからレコードを読み取ります。 コードには検証テストが含まれています (`IsInt()`と`row != null`) ページに渡される映画 ID が有効であるかどうかを確認します。

このコードは、最初に、ページの実行にのみ実行する必要がありますに注意してください。 ユーザーがクリックしたときに、データベースからムービー レコードを再読み込みしたくない、**削除ムービー**ボタンをクリックします。 したがって、ムービーがあることを示すテスト内で読み取るコード`if(!IsPost)`&mdash;つまり*要求が post 操作 (フォームの送信) ではないかどうか*します。

## <a name="adding-code-to-delete-the-selected-movie"></a>選択したムービーを削除するコードを追加します。

ユーザーがボタンをクリックすると、ムービーを削除するには、中かっこの内側は、次のコードを追加、`@`ブロック。

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

このコードは、既存のレコードを更新するためのコードに似ていますが、簡単ですが。 コードは、基本的に、SQL を実行`Delete`ステートメント。

 *EditMovie*  ページで、コードは、`if(IsPost)`ブロックします。 今回は、`if()`条件は、もう少し複雑になります。 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

ここで、2 つの条件があります。 1 つが前に説明したように、ページの送信されていることは&mdash;`if(IsPost)`します。

2 番目の条件が`!Request["buttonDelete"].IsEmpty()`、要求という名前のオブジェクトが含まれることを意味`buttonDelete`します。 確かに、フォームを送信するボタンのテストの間接的な手段になります。 フォームには、複数の submit ボタンが含まれる、要求でクリックしてされたボタンの名前のみが表示されます。 そのため、論理的にはかどうか、特定のボタンの名前は、要求に表示されます。&mdash;コードでは、そのボタンが空でない場合に説明したように、または&mdash;フォームを送信するボタンです。

`&&`演算子手段「と」(論理 AND)。 そのため、全体`if`条件がしています.

*この要求が post (初回要求しない)*  
  
 AND  
  
** `buttonDelete`*ボタンがフォームを送信するボタン。*

そのため (実際には、このページ) でこのフォームにボタンが 1 つが含まれていますの追加のテスト`buttonDelete`は技術的には必要ありません。 ただし、データが完全に削除する操作を実行しようとしています。 これに限り、ユーザーが要求して明示的に場合にのみ、操作を実行していることを確認にします。 たとえば、後からこのページを拡大し、その他のボタンを追加するとします。 場合にのみのムービーを削除するコードの実行後も、`buttonDelete`ボタンがクリックされました。

*EditMovie*  ページで、非表示フィールドから ID を取得し、SQL コマンドを実行します。 構文、`Delete`ステートメントです。

`DELETE FROM table WHERE ID = value`

含めるには必須では、`WHERE`句と ID WHERE 句を省略すると*テーブル内のすべてのレコードが削除されます*します。 説明したように、SQL コマンドにプレース ホルダーを使用して ID 値を渡します。

## <a name="testing-the-movie-delete-process"></a>ムービーの削除プロセスをテストします。

今すぐテストすることができます。 実行、*映画*ページ、およびクリックして**削除**ムービーの横にあります。 ときに、 *DeleteMovie*ページが表示されたら、をクリックして**削除ムービー**します。

![ムービーの削除 ボタンが強調表示されているビデオ ページを削除します。](deleting-data/_static/image4.png)

ボタンをクリックすると、コードは、ムービーを削除し、ムービーの一覧を返します。 削除されたムービーを検索できますが、削除されたことを確認します。

## <a name="coming-up-next"></a>次回について

次のチュートリアルでは、一般的な外観とレイアウト、サイト上のすべてのページを指定する方法を示します。

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>ムービーのページ (更新と削除のリンク) の完全な一覧

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>DeleteMovie ページ全体を示します

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>その他のリソース

- [Razor 構文を使用して ASP.NET Web プログラミングの概要](../introducing-razor-syntax-c.md)
- [SQL の DELETE ステートメント](http://www.w3schools.com/sql/sql_delete.asp)W3Schools サイト

> [!div class="step-by-step"]
> [前へ](updating-data.md)
> [次へ](layouts.md)
