---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: ASP.NET MVC - パート 2 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用 |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MV で jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84112316a9ace732cb7d75d7cbaeb071c72de822
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875449"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>ASP.NET MVC - パート 2 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。


## <a name="adding-an-automatic-datetime-template"></a>自動の DateTime テンプレートを追加します。

このチュートリアルの最初の部分では、書式設定を明示的に指定するモデルに属性を追加する方法と、モデルを表示するために使用されるテンプレートを明示的に指定する方法を説明しました。 たとえば、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)明示的に次のコード内の属性の書式を指定する、`ReleaseDate`プロパティです。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

次の例で、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性を使用して、`Date`列挙体は、モデルを表示する日付テンプレートを使用することを指定します。 プロジェクトで日付テンプレートがない場合は、組み込み日付テンプレートが使用されます。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

ただし、ASP です。MVC では、型の名前に一致するテンプレートを参照して規則を介した-構成を使用した型の照合を実行できます。 これにより、属性やコードをまったく使用せず、データを自動的に書式設定テンプレートを作成できます。 チュートリアルのこの部分の型のモデルのプロパティを自動的に適用するテンプレートを作成するを[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)です。 属性またはその他の構成を使用して、型のすべてのモデルのプロパティを表示するために、テンプレートを使用することを指定する必要はありません[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)です。

また、個々 のプロパティまたは個別のフィールドの表示をカスタマイズする方法についても学習します。

最初に、既存の書式情報を削除し、アプリケーションで完全な日時を表示してみましょう。

開く、 *Movie.cs*ファイルおよびコメント アウト、`DataType`属性を`ReleaseDate`プロパティ。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

注意して、`ReleaseDate`の書式情報を指定しない場合、既定値であるため、日付と時刻、今すぐプロパティが表示されます。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>新しいテンプレートをテストするための CSS スタイルを追加します。

日付の書式設定テンプレートを作成する前に、新しいテンプレートに適用できるいくつかの CSS スタイル規則を追加します。 表示されたページが、新しいテンプレートを使用していることを確認することができます。

開く、 *Content\Site.cs*s ファイルおよびファイルの末尾に次の CSS 規則を追加します。

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>DateTime の表示テンプレートを追加します。

ここで、新しいテンプレートを作成できます。 *Views\Movies*フォルダーを作成、 *DisplayTemplates*フォルダーです。

*\Shared*フォルダーを作成、 *displaytemplates という*フォルダーおよび*EditorTemplates*フォルダーです。

内の表示テンプレート、 *views \shared\displaytemplates*フォルダーは、すべてのコント ローラーによって使用されます。 内の表示テンプレート、 *Views\Movie\DisplayTemplates*フォルダーがでのみ使用する、`Movie`コント ローラー。 (両方のフォルダー内のテンプレートに同じ名前のテンプレートが表示された場合、 *Views\Movie\DisplayTemplates*フォルダー — より特定のテンプレートは、— ビューによって返されるが優先、`Movie`コント ローラーです)。

**ソリューション エクスプ ローラー**、展開、*ビュー*フォルダーで、展開、 *Shared*フォルダー、およびしを右クリック、 *views \shared\displaytemplates*フォルダーです。

をクリックして**追加** をクリックし、**ビュー**です。 **ビューの追加** ダイアログ ボックスが表示されます。

**ビュー名**ボックスに、入力`DateTime`です。 (この名前は、型の名前と一致するために使用する必要があります)。

選択、**部分ビューとして作成**チェック ボックスをオンします。 確認して、**レイアウトまたはマスター ページを使用して**と**厳密に型指定されたビューを作成する**チェック ボックスが選択されていません。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

**[追加]** をクリックします。 A *DateTime.cshtml*でテンプレートを作成、 *views \shared\displaytemplates*です。

次の図は、*ビュー*フォルダー**ソリューション エクスプ ローラー**後、`DateTime`表示およびエディターのテンプレートを作成します。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

開く、 *Views\Shared\DisplayTemplates\DateTime.cshtml*ファイルし、を使用して、次のマークアップを追加、 [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx)メソッド、プロパティを時刻のない日付として書式設定をします。 (、`{0:d}`形式を短い日付形式を指定します)。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

作成するには、この手順を繰り返して、`DateTime`でテンプレート、 *Views\Movie\DisplayTemplates*フォルダーです。 次のコードを使用して、 *Views\Movie\DisplayTemplates\DateTime.cshtml*ファイル。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` CSS クラスが赤い太字で表示する日付。 追加する、`loud-1`この特定のテンプレートが使用されているときに簡単に確認できるように一時的な措置と同様の CSS クラス。

終了したらが作成され、日付を表示する ASP.NET を使用してテンプレートをカスタマイズします。 一般的なテンプレート (で、 *views \shared\displaytemplates*フォルダー) 単純な短い形式の日付が表示されます。 具体的には、テンプレート、`Movie`コント ローラー (で、 *Views\Movies\DisplayTemplates*フォルダー) 赤い太字のテキストとしても書式設定されている短い日付を表示します。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 ブラウザーでは、アプリケーションのインデックス ビューを表示します。

`ReleaseDate`プロパティは、時刻のない赤い太字の日付を表示するようになりました。こうことを確認する、`DateTime`テンプレート ヘルパーで、 *Views\Movies\DisplayTemplates*経由でフォルダーが選択されている、 `DateTime` 、共有フォルダーにテンプレート ヘルパー (*Views\Shared\DisplayTemplates*)。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

名前を変更、 *Views\Movies\DisplayTemplates\DateTime.cshtml*ファイルの名前を*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*です。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

この時間、`ReleaseDate`プロパティには、時間したりせずに赤い太字の日付が表示されます。 これは、データの名前を持つテンプレート型であることを示しています (このケースで`DateTime`) をその型のすべてのモデルのプロパティの表示が自動的に使用します。 名前を変更した後、 *DateTime.cshtml*ファイルの名前を*LoudDateTime.cshtml*、ASP.NET でテンプレートを見つからなくなった、 *Views\Movies\DisplayTemplates*フォルダーで、使用*DateTime.cshtml*テンプレートから、* Views\Movies\Shared\*フォルダーです。

(一致するテンプレートは大文字小文字を区別しない、任意の大文字と小文字テンプレート ファイルの名前を作成した可能性があります。 たとえば、 *DATETIME.chstml、datetime.cshtml*、および*DaTeTiMe.cshtml*すべて一致は、`DateTime`型です)。

確認する: この時点で、`ReleaseDate`を使用してフィールドが表示されている、 *Views\Movies\DisplayTemplates\DateTime.cshtml*テンプレートでは、短い日付形式を使用してデータが表示されますが、それ以外の場合、特殊な形式は追加されません。

### <a name="using-uihint-to-specify-a-display-template"></a>UIHint を使用して、表示用のテンプレートを指定するには

場合は、web アプリケーションがある多く`DateTime`フィールドおよび日付専用の形式で、それらのほとんどすべてを表示する既定では、 *DateTime.cshtml*テンプレートは、有効なアプローチです。 ある場合は、いくつかの日付、完全な日付と時刻を表示するか。 問題はありません。 その他のテンプレートを作成して使用することができます、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)した完全な日付と時刻の書式を指定する属性。 そのテンプレートを選択的に適用できます。 使用することができます、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)モデル レベルで属性がビュー内のテンプレートを指定します。 このセクションで使用する方法が表示されます、`UIHint`選択的に書式を変更する、日時フィールドの一部のインスタンスの属性です。

開く、 *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*ファイルを開き、次に、既存のコードを置き換えます。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

これを表示する完全な日付と時刻とテキストは、緑、および大規模な CSS クラスを追加します。

開く、 *Movie.cs*ファイルを追加、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)属性を`ReleaseDate`プロパティ、次の例で示すようにします。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

これは、ことを通知 ASP.NET MVC を表示するとき、`ReleaseDate`プロパティ (具体的とだけ`DateTime`オブジェクト)、それを使用する必要があります、 *LoudDateTime.cshtml*テンプレート。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

注意して、`ReleaseDate`プロパティが表示されます、日付と時刻を大きい緑フォントでします。

戻り、`UIHint`属性、 *Movie.cs*ファイルおよびコメント アウトため、 *LoudDateTime.cshtml*テンプレートは使用されません。 アプリケーションをもう一度実行します。 リリース日は大きく、緑色は表示されません。 あることを確認、 *Views\Shared\DisplayTemplates\DateTime.cshtml*テンプレートは、インデックスおよび詳細ビューで使用します。

前述のように、ビューでは、一部のデータの個々 のインスタンスに、テンプレートを適用するテンプレートを適用することもできます。 開く、 *Views\Movies\Details.cshtml*ビュー。 追加`"LoudDateTime"`の 2 番目のパラメーターとして、 [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)を呼び出して、`ReleaseDate`フィールドです。 完成したコードは次のようになります。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

これを指定する、`LoudDateTime`テンプレートを使用して、モデルにどのような属性を適用に関係なく、モデルのプロパティを表示する必要があります。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

ムービーのインデックス ページを使用していることを確認、 *Views\Shared\DisplayTemplates\DateTime.cshtml*テンプレート (赤い太字) および*Movie\Details*ページを使用して、 *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*テンプレート (大きなおよび緑)。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

次のセクションでは、複合型のテンプレートを作成します。

> [!div class="step-by-step"]
> [前へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [次へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
