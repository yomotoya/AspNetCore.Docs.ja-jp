---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: ASP.NET Web Pages の概要 - データベースのデータの削除 |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルでは、個々 のデータベース エントリを削除する方法を示します。 ASP.NET Web Pa. 内のデータベース データの更新で、系列を修了を想定しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 146199e862cd6fa2607671d31633476b1cb67021
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>ASP.NET Web ページの概要 - データベースのデータを削除します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルでは、個々 のデータベース エントリを削除する方法を示します。 を通じてシリーズを完了すると想定[データベースのデータを更新する ASP.NET Web Pages で](updating-data.md)です。
> 
> 学習する内容。
> 
> - レコードの一覧から個々 のレコードを選択する方法です。
> - データベースから 1 つのレコードを削除する方法です。
> - フォームで、特定のボタンがクリックされたことを確認する方法です。
>   
> 
> 説明されている機能/テクノロジ:
> 
> - `WebGrid`ヘルパー。
> - SQL`Delete`コマンド。
> - `Database.Execute` SQL を実行するメソッドを`Delete`コマンド。


## <a name="what-youll-build"></a>新機能のビルドします。

前のチュートリアルでは、既存のデータベース レコードを更新する方法について説明しました。 このチュートリアルは、似ていますが、レコードを更新するには、代わりに、それを削除します。 プロセスは、このチュートリアルに短くなるように単純になりますが削除する点を除いて、ほぼ同じです。

*映画*を更新します ページで、`WebGrid`ヘルパーを表示できるように、**削除**に付随する各ムービーの横にあるリンク、**編集**以前追加したリンクします。

![各ムービーの削除 リンクを表示するビデオ ページ](deleting-data/_static/image1.png)

同様に編集するをクリックすると、**削除**リンクに移動する異なるページでは、ムービー情報が既にフォームで。

![表示ムービーでビデオ ページを削除します。](deleting-data/_static/image2.png)

レコードを完全に削除するボタンをクリックすることができますし、します。

## <a name="adding-a-delete-link-to-the-movie-listing"></a>ムービーの一覧に、[削除] リンクを追加します。

追加することで開始、**削除**へのリンク、`WebGrid`ヘルパー。 このリンクがに似ていますが、**編集**リンク前のチュートリアルで追加します。

開く、 *Movies.cshtml*ファイル。

変更、`WebGrid`列を追加して、ページの本文内のマークアップ。 変更後のマークアップを次に示します。

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

新しい列では、この 1 つです。

[!code-html[Main](deleting-data/samples/sample2.html)]

グリッドの構成方法、**編集**列は、グリッドで一番左と**削除**列は右端にあります。 (後にコンマがあります、`Year`ここで、列ことに注意していない場合にします)。これらのリンク列の移動先に関して特別なところを使用する必要があるし、互いの横にある同様の容易に配置する可能性があります。 この場合、これらは個別に取得を混在させることが困難にします。

![編集と詳細へのリンクをムービー ページがいるいない並べて表示するマーク](deleting-data/_static/image3.png)

新しい列は、リンクを示しています (`<a>`要素)"Delete"と表示されました。 リンクのターゲット (その`href`属性) は、コードでこの URL のようなものに最終的に解決される、`id`各ムービーのさまざまな値。

[!code-css[Main](deleting-data/samples/sample3.css)]

このリンクをという名前のページを呼び出す*DeleteMovie*を選択したムービーの ID を渡します。

このチュートリアルはありませんについて詳しくは、このリンクの構築方法とほぼ同じになっているため、**編集**リンク前のチュートリアル ([データベースのデータを更新する ASP.NET Web Pages で](updating-data.md))。

## <a name="creating-the-delete-page"></a>ページの削除 を作成します。

対象となるページを作成できるようになりました、**削除**グリッド内のリンク。

> [!NOTE] 
> 
> **重要な**最初を削除するレコードを選択して、プロセスを確認する別のページとボタンを使用しての手法がセキュリティの非常に重要です。 前のチュートリアルで読んだとして行う*任意*web サイトへの変更の種類にする必要があります*常に*行うフォームを使用して&mdash;は、HTTP POST 操作を使用します。 を加えた場合に (つまり、GET 操作を使用) のリンクをクリックするだけで、サイトを変更すること人でした、サイトに対して単純な要求を行うし、データを削除します。 でも、検索エンジンのクローラー サイト インデックス作成を行うことは、次のリンクだけでデータを誤って削除可能性があります。
> 
> アプリでは、ユーザー レコードを変更することができます、ときにする必要があるかを編集するためのユーザーにレコードを表示します。 レコードを削除するのには、この手順をスキップしたくなる場合があります。 ただしその手順をスキップしません。 (これはもユーザーのレコードを確認し、これらの目的は、レコードを削除していることを確認するために役立ちます)。
> 
> 後続のチュートリアルのセットでは、ユーザーがレコードを削除する前にログインする必要がありますので、ログインの機能を追加する方法が表示されます。


という名前のページを作成する*DeleteMovie.cshtml*が次のマークアップ ファイルを置き換えます。

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

同様には、このマークアップ、 *EditMovie*ページ、テキスト ボックスを使用する代わりにことを除いて (`<input type="text">`)、マークアップを含む`<span>`要素。 何もここで編集するのには。 行う必要があるすべては、ことができるようにするため、右のムービーを削除していることを確認して、ムービーの詳細を表示します。

マークアップには、ユーザーが、ムービーの一覧ページに戻りますできるリンクが既に含まれています。

同様、 *EditMovie*  ページで、選択したムービーの ID が非表示のフィールドに格納します。 (として渡されるページに、まずクエリ文字列値です。)`Html.ValidationSummary`妥当性確認エラーを表示するための呼び出しです。 ここでは、エラーは、ページに映画の ID が渡されなかったことまたはムービー ID が無効である可能性があります。 この状況でビデオを選択せずこのページのユーザーが実行された場合に発生する可能性があります、*映画*ページ。

ボタンのキャプション**削除ムービー**に設定されている、name 属性と`buttonDelete`です。 `name`属性は、フォームを送信するボタンを識別するコードで使用されます。

1)、ムービーの詳細を読んで、ページが最初に表示されるときにコードを記述し、2) 実際には、ユーザーがボタンをクリックしたときに、ムービーを削除する必要があります。

## <a name="adding-code-to-read-a-single-movie"></a>1 つのムービーの読み取りにコードを追加します。

上部にある、 *DeleteMovie.cshtml*  ページで、次のコード ブロックを追加します。

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

このマークアップは、対応するコードと同じ、 *EditMovie*ページ。 クエリ文字列からムービー ID を取得し、データベースからレコードを読み取るための ID を使用します。 コードには検証テストが含まれています (`IsInt()`と`row != null`) ページに渡されるムービー ID が有効であるかどうかを確認します。

このコードは、最初に実行、ページにのみ実行する必要がありますに注意してください。 ユーザーがクリックしたときに、ムービーのレコードをデータベースからを再読み込みしたくない、**削除ムービー**ボタンをクリックします。 そのため、コードと表示されているテスト内ではビデオを読めない`if(!IsPost)` &mdash; 、*かどうか、要求は post 操作 (フォームの送信)*です。

## <a name="adding-code-to-delete-the-selected-movie"></a>選択したムービーを削除するコードを追加します。

ユーザーがボタンをクリックしたときに、ムービーを削除するには、次のコードの右中かっこの内側を追加、`@`ブロック。

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

このコードは、既存のレコードを更新するためのコードに似た簡単です。 コードは、基本的には、SQL を実行`Delete`ステートメントです。

 同様、 *EditMovie*  ページで、コードは、`if(IsPost)`ブロックします。 このとき、`if()`条件は、もう少し複雑になります。 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

ここで、2 つの条件があります。 1 つあるページが送信される、前に説明したようには、 &mdash; `if(IsPost)`です。

2 番目の条件が`!Request["buttonDelete"].IsEmpty()`、要求という名前のオブジェクトが含まれることを意味`buttonDelete`です。 確かに、どのボタン フォームの送信のテストの間接的な手段を勧めします。 フォームには、複数の submit ボタンが含まれる、要求でクリックしてされたボタンの名前のみが表示されます。 そのため、論理的には、要求で特定のボタンの名前が表示されます&mdash;で述べたように、コードでは、そのボタンが空でない場合、または&mdash;フォームを送信するボタンです。

`&&`演算子手段「と」(論理 AND)。 そのため、全体`if`条件がしています.

*この要求は、post (初回要求されません)*  
  
 AND  
  
** `buttonDelete`*ボタンがフォームを送信するボタンをクリックします。*

そのため (実際には、このページ) でのこのフォームにボタンが 1 つだけが含まれていますの追加のテスト`buttonDelete`は技術的には必要ありません。 それでもは、データが完全に削除する操作を実行しようとしています。 したがって、ユーザーが要求して明示的に場合にのみ、操作を実行することができるだけに必ずたいです。 たとえば、このページを後で展開し、その他のボタンを追加します。 場合にのみのムービーを削除するコードの実行後も、 `buttonDelete` button がクリックしてされました。

同様、 *EditMovie*  ページで、非表示のフィールドから ID を取得して、SQL コマンドを実行します。 構文、`Delete`ステートメントは。

`DELETE FROM table WHERE ID = value`

含めることが重要、`WHERE`句と、id です。 WHERE 句を省略すると*テーブル内のすべてのレコードが削除されます*です。 説明したよう、SQL コマンドにプレース ホルダーを使用して ID 値を渡します。

## <a name="testing-the-movie-delete-process"></a>ムービーの削除プロセスのテスト

これで次のようにテストすることができます。 実行、*映画* ページで、をクリックして**削除**ムービーの横にあります。 ときに、 *DeleteMovie*ページが表示されたら、をクリックして**削除ムービー**です。

![ムービーの削除 ボタンが強調表示されているとムービーのページを削除します。](deleting-data/_static/image4.png)

ボタンをクリックすると、コードは、ムービーを削除し、ムービーの一覧を戻します。 削除したムービーを検索することがありますが削除されたことを確認します。

## <a name="coming-up-next"></a>次へ直近の見通し

次のチュートリアルでは、一般的な外観とレイアウト、サイト上のすべてのページを指定する方法を示します。

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>ムービーのページ (更新と削除のリンク) の完全な一覧

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>DeleteMovie ページの完全な一覧

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>その他のリソース

- [Razor 構文を使用して ASP.NET Web プログラミングの概要](../introducing-razor-syntax-c.md)
- [SQL の DELETE ステートメント](http://www.w3schools.com/sql/sql_delete.asp)W3Schools サイト

> [!div class="step-by-step"]
> [前へ](updating-data.md)
> [次へ](layouts.md)
