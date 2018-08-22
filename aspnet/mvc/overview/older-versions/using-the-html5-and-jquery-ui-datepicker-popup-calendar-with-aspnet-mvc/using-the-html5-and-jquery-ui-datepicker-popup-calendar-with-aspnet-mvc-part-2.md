---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: ASP.NET MVC - 第 2 部での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、エディターのテンプレート、表示のテンプレートと、ASP.NET MV の jQuery UI datepicker ポップアップ カレンダーを操作する方法の基本を説明しています.
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 572185507016afc26fc2e06ede55361b2daed277
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831900"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>ASP.NET MVC - 第 2 部での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。


## <a name="adding-an-automatic-datetime-template"></a>自動の DateTime のテンプレートを追加します。

このチュートリアルの最初の部分では、書式設定を明示的に指定するモデルに属性を追加する方法と、モデルを表示するために使用するテンプレートを明示的に指定する方法を説明しました。 たとえば、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性を明示的に次のコードでは、書式を指定します、`ReleaseDate`プロパティ。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

次の例では、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性を使用して、`Date`列挙型では、モデルを表示する日付テンプレートを使用することを指定します。 プロジェクトで日付テンプレートがない場合は、組み込みの日付のテンプレートが使用されます。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

ただし、ASP します。MVC では、規則上の構成を使用して、型の名前に一致するテンプレートを探すことによって型の照合を実行できます。 これにより、属性やコードをまったく使用せず、データを自動的に書式設定テンプレートを作成できます。 このチュートリアルのこの部分では、型のモデルのプロパティが自動的に適用するテンプレートを作成します[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)します。 属性またはその他の構成を使用して、型のすべてのモデルのプロパティを表示するために、テンプレートを使用することを指定する必要はありません[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)します。

また、個々 のプロパティまたはさらには個々 のフィールドの表示をカスタマイズする方法についても学習します。

を開始するには、既存の書式設定情報を削除し、アプリケーションの完全な日付を表示してみましょう。

開く、 *Movie.cs*ファイルし、コメント アウト、`DataType`属性を`ReleaseDate`プロパティ。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

なお、`ReleaseDate`の書式情報が指定されていないとき、既定であるため、日付と時刻の両方にプロパティが表示されます。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>新しいテンプレートをテストするための CSS スタイルを追加します。

日付の書式設定テンプレートを作成する前に、新しいテンプレートに適用できるいくつかの CSS スタイル規則を追加します。 レンダリングされたページで、新しいテンプレートを使用していることを確認することができます。

開く、 *Content\Site.cs*s ファイルを開き、次の CSS 規則をファイルの末尾に追加します。

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>DateTime の表示のテンプレートの追加

これで新しいテンプレートを作成することができます。 *Views \movies*フォルダーを作成、 *DisplayTemplates*フォルダー。

*Views \shared*フォルダーを作成、 *DisplayTemplates*フォルダーと*EditorTemplates*フォルダー。

内の表示テンプレート、 *views \shared\displaytemplates*フォルダーがすべてのコント ローラーによって使用されます。 内の表示テンプレート、 *Views\Movie\DisplayTemplates*フォルダーがでのみ使用する、`Movie`コント ローラー。 (両方のフォルダー内のテンプレートで同じ名前のテンプレートが表示された場合、 *Views\Movie\DisplayTemplates*フォルダー-特定のテンプレートは、-ビューによって返されるが優先されます、`Movie`コント ローラー)。

**ソリューション エクスプ ローラー**、展開、*ビュー*フォルダー、展開、 *Shared*フォルダー、およびし右クリック、 *views \shared\displaytemplates*フォルダー。

クリックして**追加** をクリックし、**ビュー**します。 **ビューの追加** ダイアログ ボックスが表示されます。

**ビュー名**ボックスに「`DateTime`します。 (この名前は、型の名前を一致させるために使用する必要があります)。

選択、**部分ビューとして作成**チェック ボックスをオンします。 確認、**レイアウトまたはマスター ページを使用して**と**厳密に型指定されたビューを作成する**チェック ボックスが選択されていません。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

**[追加]** をクリックします。 A *DateTime.cshtml*でテンプレートを作成、 *views \shared\displaytemplates*します。

次の図は、*ビュー*フォルダー**ソリューション エクスプ ローラー**後、`DateTime`表示し、エディターのテンプレートが作成されます。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

開く、 *Views\Shared\DisplayTemplates\DateTime.cshtml*ファイルを開きを使用して、次のマークアップを追加、 [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx)プロパティの時間を除いた日付として書式設定メソッドです。 (、`{0:d}`形式は、短い日付形式を指定します)。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

この手順を繰り返して作成、`DateTime`テンプレート、 *Views\Movie\DisplayTemplates*フォルダー。 次のコードを使用して、 *Views\Movie\DisplayTemplates\DateTime.cshtml*ファイル。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` CSS クラスがこの日付を赤い太字のテキストで表示します。 追加した、 `loud-1` CSS クラスは、この特定のテンプレートが使用されているときに簡単に確認できるように、一時的な措置としてだけです。

実行したが作成され、日付を表示する ASP.NET を使用してテンプレートをカスタマイズします。 一般的なテンプレート (で、 *views \shared\displaytemplates*フォルダー) 単純な短い形式の日付が表示されます。 具体的には、テンプレート、`Movie`コント ローラー (で、 *Views\Movies\DisplayTemplates*フォルダー) 赤い太字のテキストとしても書式設定された短い形式の日付を表示します。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 ブラウザーでは、アプリケーションのインデックス ビューをレンダリングします。

`ReleaseDate`プロパティは、時間を除いた赤い太字の日付を表示するようになりました。こうことを確認する、`DateTime`テンプレート ヘルパーで、 *Views\Movies\DisplayTemplates*経由でフォルダーが選択されている、 `DateTime` 、共有フォルダーにテンプレート化されたヘルパー (*Views\Shared\DisplayTemplates*)。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

名前を変更、 *Views\Movies\DisplayTemplates\DateTime.cshtml*ファイルを*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*します。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

この時間、`ReleaseDate`プロパティは、時間と赤い太字なしで日付を表示します。 これは、データの名前を持つテンプレートを入力することを示しています (この場合`DateTime`) に自動的にその型のすべてのモデル プロパティの表示に使用します。 名前を変更した後、 *DateTime.cshtml*ファイルを*LoudDateTime.cshtml*、ASP.NET でのテンプレートを見つからなくなった、 *Views\Movies\DisplayTemplates*フォルダーを使用して*DateTime.cshtml*テンプレートから、* Views\Movies\Shared\*フォルダー。

(一致するテンプレートは大文字と小文字、大文字小文字の区別でテンプレート ファイルの名前を作成した可能性があるためです。 たとえば、 *DATETIME.chstml、datetime.cshtml*、および*DaTeTiMe.cshtml*はすべてと一致、`DateTime`型です)。

確認する: この時点で、`ReleaseDate`を使用してフィールドが表示されている、 *Views\Movies\DisplayTemplates\DateTime.cshtml*テンプレートでは、短い日付形式を使用してデータが表示されますが、それ以外の場合、特殊な形式は追加されません。

### <a name="using-uihint-to-specify-a-display-template"></a>UIHint を使用して、表示用のテンプレートを指定するには

Web アプリケーションに多くの場合`DateTime`フィールドと、既定では日付のみの形式で、それらのほとんどすべてを表示する、 *DateTime.cshtml*テンプレートは、適切なアプローチです。 しかし、ある場合は、いくつかの日付、完全な日付と時刻を表示するでしょうか。 問題はありません。 その他のテンプレートを作成して使用することができます、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)した完全な日付と時刻の書式を指定する属性。 そのテンプレートを選択的に適用できます。 使用することができます、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)モデル レベルで属性は、ビュー内でテンプレートを指定できます。 このセクションで使用する方法を確認します、`UIHint`選択的に日付/時刻フィールドの一部のインスタンスの書式設定を変更する属性。

開く、 *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*ファイルを開き、次の既存のコードに置き換えます。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

これによって、表示される完全な日付と時刻、緑、および大きなテキストは、CSS クラスを追加します。

開く、 *Movie.cs*追加ファイルを開き、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)属性を`ReleaseDate`プロパティは、次の例に示すように。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

これは、ASP.NET MVC に伝えますを表示するとき、`ReleaseDate`プロパティ (具体的とだけ`DateTime`オブジェクト)、それを使用する必要があります、 *LoudDateTime.cshtml*テンプレート。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

なお、`ReleaseDate`プロパティは日付と時刻を今すぐ表示すると大きい緑色のフォントでします。

戻り、`UIHint`属性、 *Movie.cs*ファイルし、コメント アウト、 *LoudDateTime.cshtml*テンプレートを使用しません。 アプリケーションをもう一度実行します。 リリース日は、大規模で緑色は表示されません。 これにより、 *Views\Shared\DisplayTemplates\DateTime.cshtml* Index と Details ビューでテンプレートを使用します。

前述のように、ビューをいくつかのデータの個々 のインスタンスにテンプレートを適用するためのテンプレートを適用することもできます。 開く、 *Views\Movies\Details.cshtml*ビュー。 追加`"LoudDateTime"`の 2 番目のパラメーターとして、 [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)を呼び出し、`ReleaseDate`フィールド。 完成したコードは次のようになります。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

これは、ことを指定します、`LoudDateTime`モデルにどのような属性が適用に関係なく、モデルのプロパティを表示するテンプレートを使用する必要があります。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

ムービーのインデックス ページを使用していることを確認、 *Views\Shared\DisplayTemplates\DateTime.cshtml*テンプレート (赤い太字)、 *Movie\Details*ページを使用して、 *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*テンプレート (大きなおよび緑)。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

次のセクションでは、複合型のテンプレートを作成します。

> [!div class="step-by-step"]
> [前へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [次へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
