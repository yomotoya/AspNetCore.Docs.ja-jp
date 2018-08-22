---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: モデル バインディングと web フォームで JQuery UI Datepicker の統合 |Microsoft Docs
author: tfitzmac
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 81cc5f010f2c6c7707223679717e34108a5a7aa3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826876"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>モデル バインディングと web フォームで JQuery UI Datepicker の統合
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。 このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。
> 
> このチュートリアルは、JQuery UI を追加する方法を示します[Datepicker ウィジェット](http://jqueryui.com/datepicker/)Web フォーム、およびモデルを使用するバインドを選択して、データベースを更新します。
> 
> このチュートリアルで作成したプロジェクトで、[最初](retrieving-data.md)と[2 番目](updating-deleting-and-creating-data.md)系列の部分。
> 
> できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)c# または VB. で完全なプロジェクト ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。 これは、このチュートリアルで示すように Visual Studio 2013 テンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。


## <a name="what-youll-build"></a>構築します

このチュートリアルではあります。

1. 学生の加入契約日を記録する、モデルにプロパティを追加します。
2. JQuery UI Datepicker ウィジェットを使用して、登録日付を選択するユーザーを有効にします。
3. 登録日の検証規則を実施します。

JQuery UI Datepicker ウィジェットでは、簡単に、フィールドと、ユーザーが対話するときにポップアップ表示されるカレンダーから日付を選択することができます。 このウィジェットを使用すると、日付を手動で入力するよりも、ユーザーのより便利なことができます。 データ操作のモデル バインドを使用するページへの Datepicker ウィジェットの統合には、少量の追加の作業のみが必要です。

## <a name="add-a-new-property-to-the-model"></a>モデルに新しいプロパティを追加します。

最初に、追加は、 **Datetime**プロパティ、学生をモデルし、その変更をデータベースに移行します。 開いている**UniversityModels.cs**、Student モデルを強調表示されたコードを追加します。

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute**プロパティの検証規則の実施が含まれます。 このチュートリアルでは、Contoso University は、2013 年 1 月 1 日に設立された、したがって以前の登録日が無効ですとします。

パッケージの管理 ウィンドウで、コマンドを実行して移行を追加**追加移行 AddEnrollmentDate**します。 移行コードが Student テーブルに新しい Datetime 列を追加することに注意してください。 RangeAttribute で指定した値を一致させるのには、以下の強調表示されたコードに示すように、新しい列の既定値を追加します。

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

移行ファイルに変更を保存します。

もう一度データをシードする必要はありません。 そのため、開く**Configuration.cs** 、Migrations フォルダーに削除するかでコードをコメント アウト、**シード**メソッド。 ファイルを保存して閉じます。

ここで、コマンドを実行**更新データベース**します。 EnrollmentDate の既定値である列データベースとすべての既存のレコードに存在することに注意してください。

## <a name="add-dynamic-controls-for-enrollment-date"></a>登録日のダイナミック コントロールを追加します。

表示と編集の加入契約日にコントロールを追加します。 この時点では、値を編集して、テキスト ボックスを使用します。 チュートリアルの後半では、JQuery の日付選択ウィジェットをテキスト ボックスが変わります。

最初に、何も変更する必要がないことに注意してください。 が、 **AddStudent.aspx**ファイル。 DynamicEntity コントロールは、新しいプロパティを自動的に表示されます。

開いている**Students.aspx**、次の強調表示されたコードを追加します。

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

アプリケーションを実行し、日付を入力して、登録日の値を設定できることに注意してください。 新しい学生を追加する場合。

![日付を設定](integrating-jquery-ui/_static/image1.png)

または、既存の値を編集します。

![日付を編集します。](integrating-jquery-ui/_static/image2.png)

日付の動作を入力すると、カスタマー エクスペリエンスを提供したいができない可能性があります。 次のセクションでは、カレンダーから日付の選択を有効になります。

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>JQuery UI を使用する NuGet パッケージをインストールします。

**ジュース UI** NuGet パッケージが JQuery UI ウィジェットの web アプリケーションに簡単に統合できるようにします。 このパッケージを使用するには、NuGet からインストールします。

![ジュース UI を追加します。](integrating-jquery-ui/_static/image3.png)

インストールするジュース UI のバージョンは、アプリケーションで JQuery のバージョンで競合があります。 このチュートリアルで前に、アプリケーションを実行してみてください。 JavaScript エラーが発生した場合は、JQuery バージョンを調整する必要があります。 JQuery の想定されるバージョンをスクリプト フォルダー (のこのチュートリアルの執筆時点ではバージョン 1.8.2) に追加するか、Site.master では、JQuery ファイルへのパスを指定します。

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Datepicker のウィジェットを含める DateTime テンプレートをカスタマイズします。

Datepicker のウィジェットは、datetime 値を編集するための動的なデータ テンプレートに追加されます。 ウィジェットを追加すると、テンプレートに、新しい生徒を追加するため、両方の形式で、編集の学生向けのグリッド ビューで自動的にレンダリングされます。 開いている**DateTime\_Edit.ascx**、次の強調表示されたコードを追加します。

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

分離コード ファイルでは、DatePicker の日付の最小と最大値を設定します。 これらの値を設定してユーザーを防ぐためから無効な日付に移動されます。 最小値と最大値を取得するには、 **RangeAttribute**で提供されている場合は、DateTime プロパティ。 開いている**DateTime\_Edit.ascx.cs**、次の強調表示されているコード ページを追加および\_Load メソッド。

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Web アプリケーションを実行し、AddStudent ページに移動します。 フィールドの値を指定して、登録日付、テキスト ボックスをクリックすると、カレンダーが表示されることに注意してください。

![日付の選択](integrating-jquery-ui/_static/image4.png)

日付を選択し、をクリックして**挿入**します。 RangeAttribute がサーバー上の検証を強制します。 Datepicker の minDate プロパティを設定して適用することも検証、クライアントでします。 予定表では、minDate の値より前の日付に移動し、ユーザーはことはできません。

グリッド ビュー内のレコードを編集するときに、予定表も表示されます。

![GridView の Datepicker](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>まとめ

このチュートリアルでは、モデル バインディングを使用する web フォームに JQuery ウィジェットを組み込む方法について説明しました。

次の[チュートリアル](using-query-string-values-to-retrieve-data.md)データを選択する場合、クエリ文字列値を使用します。

> [!div class="step-by-step"]
> [前へ](sorting-paging-and-filtering-data.md)
> [次へ](using-query-string-values-to-retrieve-data.md)
