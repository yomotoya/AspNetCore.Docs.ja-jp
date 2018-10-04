---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: ASP.NET MVC - パート 3 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、エディターのテンプレート、表示のテンプレートと、ASP.NET MV の jQuery UI datepicker ポップアップ カレンダーを操作する方法の基本を説明しています.
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: baaca74d584a6d5028824e877c561494cdff044e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577224"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>ASP.NET MVC - パート 3 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

> このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。


## <a name="working-with-complex-types"></a>複雑な型の使用

このセクションでは、address クラスを作成し、それを表示するテンプレートを作成する方法について説明します。

*モデル*フォルダーという新しいクラス ファイルを作成*Person.cs* 2 つの型を配置します。`Person`クラスおよび`Address`クラス。 `Person`クラスとして型指定されたプロパティが含まれます`Address`します。 `Address`型が複合型のような組み込み型のいずれかがいない`int`、 `string`、または`double`します。 代わりに、いくつかのプロパティがあります。 新しいクラスのコードは、次のようになります。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

`Movie`コント ローラーで、次のコードを追加する`PersonDetail`person のインスタンスを表示するアクション。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

次のコードを追加し、`Movie`を設定するコント ローラー、`Person`サンプル データ モデル。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

開く、 *Views\Movies\PersonDetail.cshtml*ファイルを開き、次のマークアップを追加、`PersonDetail`ビュー。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Ctrl + f5 キーを押してアプリケーションを実行しに移動します*映画/PersonDetail*します。

`PersonDetail`ビューが含まれていない、`Address`複合型は、ことがわかります。 このスクリーン ショット。 (アドレスは表示されません。)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address`のため、複雑な型では、モデル データは表示されません。 アドレス情報を表示するには、開く、 *Views\Movies\PersonDetail.cshtml*もう一度ファイルを開き、次のマークアップを追加します。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

完全なマークアップを`PersonDetail`ビューは次のようになりますようになりました。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

アプリケーションを再度実行し、表示、`PersonDetail`ビュー。 アドレス情報が表示されます。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>複合型のテンプレートの作成

このセクションでは、表示するために使用されるテンプレートを作成します、`Address`複合型。 テンプレートを作成する場合、`Address`型、ASP.NET MVC に自動的に使用できるアプリケーションで任意の場所のアドレス モデルの書式を設定します。 これによりのレンダリングを制御する方法、`Address`アプリケーション内の 1 つの場所からの型。

*Views \shared\displaytemplates*フォルダー、という名前の厳密に型指定された部分ビューを作成する**アドレス**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

クリックして**追加**を開き、新しい*Views\Shared\DisplayTemplates\Address.cshtml*ファイル。 新しいビューには、次の生成されたマークアップが含まれています。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

アプリケーションを実行し、表示、`PersonDetail`ビュー。 今回は、`Address`作成したテンプレートを使用して、表示、`Address`複合型の表示は次の次のようになります。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>モデルの表示形式とテンプレートを指定する方法は概要:

次の方法を使用して形式またはモデル プロパティのテンプレートを指定できますを見てきました。

- 適用、`DisplayFormat`属性をモデルのプロパティ。 たとえば、次のコードは、時間を除いた表示される日付をなります。

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- 適用する、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性をモデルとデータ型を指定するプロパティ。 たとえば、次のコードでは、時間を除いた表示される日付がなります。

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    アプリケーションが含まれている場合、 *date.cshtml*テンプレート、 *views \shared\displaytemplates*フォルダーまたは*Views\Movies\DisplayTemplates*フォルダー、そのテンプレートレンダリングに使用される、`DateTime`プロパティ。 それ以外の場合、組み込みの ASP.NET テンプレート システムでは、日付としてプロパティを表示します。
- 表示テンプレートの作成、 *views \shared\displaytemplates*フォルダーまたは*Views\Movies\DisplayTemplates*フォルダーの名前には、書式を設定するデータ型が一致します。 たとえば、ことを説明する、 *Views\Shared\DisplayTemplates\DateTime.cshtml*表示するために使用された`DateTime`モデルに属性を追加して、マークアップをビューに追加することがなく、モデルのプロパティ。
- 使用して、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)モデルのプロパティを表示するテンプレートを指定するには、モデルの属性。
- 表示テンプレート名を明示的に追加する、 [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)ビューで呼び出します。

使用する方法によって異なります、アプリケーションで行う必要があります。 これらの必要な形式の種類だけを取得する方法を混在させることは珍しくありません。

次のセクションで切り替えて少し変えて、入力する方法をカスタマイズするデータを表示する方法をカスタマイズから移動します。 編集ビューの日付を指定する優れた方法を提供するために、アプリケーションで jQuery datepicker フックします。

> [!div class="step-by-step"]
> [前へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [次へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
