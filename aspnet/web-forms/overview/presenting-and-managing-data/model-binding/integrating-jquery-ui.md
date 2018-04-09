---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: モデル バインディング機能と web フォームと JQuery UI Datepicker の統合 |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線-しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>モデル バインディング機能と web フォームと JQuery UI Datepicker の統合
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインディングは、ObjectDataSource または SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作にします。 この系列は入門資料で始まるし、後のチュートリアルより高度な概念に移動されます。
> 
> このチュートリアルは、JQuery UI を追加する方法を示します[Datepicker ウィジェット](http://jqueryui.com/datepicker/)Web フォーム、および選択した値を持つデータベースを更新するバインドを使用してモデルにします。
> 
> このチュートリアルで作成されたプロジェクトで、[最初](retrieving-data.md)と[2 番目](updating-deleting-and-creating-data.md)シリーズの一部です。
> 
> 実行できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)プロジェクト、完全な c# または vb です。 ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。 これは、このチュートリアルで示すように Visual Studio 2013 のテンプレートよりも若干異なる Visual Studio 2012 テンプレートを使用します。


## <a name="what-youll-build"></a>新機能のビルド

このチュートリアルでは、次の手順を。

1. プロパティ、スチューデントの登録日を記録するモデルを追加します。
2. JQuery UI Datepicker ウィジェットを使用する加入契約日を選択するユーザーを有効にします。
3. 登録日の検証規則を実施します。

JQuery UI Datepicker ウィジェットでは、簡単にフィールドをユーザーが操作するときにポップアップ表示されるカレンダーから日付を選択することができます。 このウィジェットを使用すると、日付を手動で入力よりもユーザーにとって便利です。 データ操作のモデル バインディングを使用するページに Datepicker ウィジェットを統合するには、少量の追加作業だけが必要です。

## <a name="add-a-new-property-to-the-model"></a>モデルに新しいプロパティを追加します。

最初に、追加されます、 **Datetime**プロパティ、受講者をモデル化し、データベースにその変更を移行します。 開いている**UniversityModels.cs**、学生モデルを強調表示されたコードを追加します。

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute**プロパティの検証規則の実施に含まれます。 このチュートリアルではものと Contoso 大学が、2013 年 1 月 1 日の設立されました、したがって以前の登録日が無効です。

パッケージの管理 ウィンドウでコマンドを実行して、移行を追加**追加移行 AddEnrollmentDate**です。 移行コードが Student テーブルに新しい Datetime 列を追加することに注意してください。 RangeAttribute で指定した値と一致するには、次の強調表示されたコードに示すように、新しい列の既定値を追加します。

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

移行ファイルへの変更を保存します。

データをもう一度シードする必要はありません。 そのため、開く**される Configuration.cs** 、Migrations フォルダー内でコードをコメント アウトまたは削除、**シード**メソッドです。 ファイルを保存して閉じます。

これで、コマンドを実行**更新データベース**です。 列がデータベースに存在するようになりましたし EnrollmentDate の既定値があるすべての既存のレコードのことに注意してください。

## <a name="add-dynamic-controls-for-enrollment-date"></a>登録日のダイナミック コントロールを追加します。

表示および登録日を編集するためのコントロールを追加します。 この時点では、テキスト ボックスで値を編集します。 チュートリアルの後半には、JQuery Datepicker ウィジェットがテキスト ボックスが変わります。

最初にすることが重要に変更を加える必要はありません、 **AddStudent.aspx**ファイル。 DynamicEntity コントロールは、新しいプロパティを自動的にレンダリングされます。

開いている**Students.aspx**、し、次の強調表示されたコードを追加します。

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

アプリケーションを実行し、日付を入力して登録日の値を設定することができますに注意してください。 新しい学生を追加するとき。

![日付を設定](integrating-jquery-ui/_static/image1.png)

または、既存の値を編集します。

![日付を編集します。](integrating-jquery-ui/_static/image2.png)

日付動作しますが、それを入力すると、カスタマー エクスペリエンスを提供したいができない可能性があります。 次のセクションでは、カレンダーから日付の選択を有効になります。

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>JQuery UI を使用する NuGet パッケージをインストールします。

**ジュース UI** NuGet パッケージは、JQuery UI ウィジェットの web アプリケーションに簡単に統合を有効にします。 このパッケージを使用するには、NuGet でインストールします。

![ジュース UI を追加します。](integrating-jquery-ui/_static/image3.png)

インストールするジュース UI のバージョンは、アプリケーションで JQuery のバージョンで競合があります。 このチュートリアルの前に、アプリケーションを実行して再試行してください。 JavaScript エラーが発生した場合は、JQuery のバージョンを調整する必要があります。 スクリプト フォルダー (このチュートリアルの書き込み時にバージョン 1.8.2) に JQuery の想定されるバージョンを追加するか、Site.master、JQuery ファイルへのパスを指定します。

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>テンプレートをカスタマイズ DateTime を Datepicker ウィジェットが含まれます

Datetime 値を編集用の動的なデータ テンプレートには、Datepicker ウィジェットを追加します。 ウィジェットをテンプレートに追加すると、それが自動的とレンダリングされます新しい学生を追加するため、両方の形式で編集受講者をグリッド ビューで。 開いている**DateTime\_Edit.ascx**、し、次の強調表示されたコードを追加します。

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

分離コード ファイルでは、DatePicker の日付の最小と最大値を設定します。 これらの値を設定してはされないようにする無効な日付に移動します。 最小値と最大値を取得するには、 **RangeAttribute**が提供されている場合は、DateTime プロパティにします。 開いている**DateTime\_Edit.ascx.cs**、次の強調表示されているコード ページを追加および\_Load メソッドです。

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Web アプリケーションを実行し、AddStudent のページに移動します。 フィールドの値を指定し、登録日に、テキスト ボックスをクリックすると、カレンダーが表示されることに注意してください。

![日付の選択](integrating-jquery-ui/_static/image4.png)

日付を選択し、をクリックして**挿入**です。 RangeAttribute がサーバー上の検証を強制します。 Datepicker で minDate プロパティの設定によって適用することも検証クライアントにします。 予定表では、minDate の値より前の日付に移動し、ユーザーはことはできません。

グリッド ビュー内のレコードを編集するときにも、カレンダーが表示されます。

![GridView で Datepicker](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>まとめ

このチュートリアルでは、モデル バインディングを使用する web フォームに JQuery ウィジェットを組み込む方法について学習しました。

次の[チュートリアル](using-query-string-values-to-retrieve-data.md)データを選択する場合、クエリ文字列の値を使用します。

> [!div class="step-by-step"]
> [前へ](sorting-paging-and-filtering-data.md)
> [次へ](using-query-string-values-to-retrieve-data.md)
