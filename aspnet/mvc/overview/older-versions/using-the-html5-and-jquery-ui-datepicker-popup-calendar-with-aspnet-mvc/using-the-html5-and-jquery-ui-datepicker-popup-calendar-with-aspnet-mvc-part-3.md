---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: "ASP.NET MVC - パート 3 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用 |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MV で jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 7d4ed67254c2b0fc2aef748cfab1c8f628b25641
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>ASP.NET MVC - パート 3 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。


## <a name="working-with-complex-types"></a>複合型の使用

このセクションでは、アドレス クラスを作成し、表示するテンプレートを作成する方法について説明します。

*モデル*フォルダー、という新しいクラス ファイルを作成*Person.cs*を配置する 2 つの種類:`Person`クラスおよび`Address`クラスです。 `Person`クラスとして型指定されたプロパティに含まれる`Address`です。 `Address`型は複合型、つまりと同様に、組み込み型のいずれでもない`int`、 `string`、または`double`です。 代わりに、いくつかのプロパティがあります。 新しいクラスのコードは、次のようになります。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

`Movie`コント ローラーで、次のコードを追加する`PersonDetail`ユーザー インスタンスを表示するアクション。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

次のコードを追加、`Movie`を設定するコント ローラー、`Person`サンプル データが含まれるモデル。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

開く、 *Views\Movies\PersonDetail.cshtml*ファイルし、次のマークアップを追加、`PersonDetail`ビュー。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Ctrl キーを押しながら f5 キーを押してアプリケーションを実行しに移動*映画/PersonDetail*です。

`PersonDetail`ビューが含まれていない、`Address`複合型は、することができますを参照してくださいこのスクリーン ショット。 (アドレスは表示されません。)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address`複合型になっているために、モデル データは表示されません。 アドレス情報を表示、開く、 *Views\Movies\PersonDetail.cshtml*ファイルに追加され、次のマークアップを追加します。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

完全なマークアップを`PersonDetail`次のように表示されるようになりました。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

アプリケーションを再実行し、表示、`PersonDetail`ビュー。 アドレス情報が表示されます。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>複合型のテンプレートの作成

このセクションの内容を表示するために使用されるテンプレートを作成、`Address`複合型。 用のテンプレートを作成する場合、`Address`型、ASP.NET MVC に自動的にそのフォーマットに使用できるアプリケーションで任意の場所のアドレス モデル。 こうことのレンダリングを制御する方法、`Address`アプリケーション内の 1 つの場所からの型。

*Views \shared\displaytemplates*フォルダー、という名前の厳密に型指定された部分ビューを作成する**アドレス**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

をクリックして**追加**を開き、新しい*Views\Shared\DisplayTemplates\Address.cshtml*ファイル。 新しいビューには、次の生成されたマークアップが含まれています。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

アプリケーションを実行し、表示、`PersonDetail`ビュー。 このとき、`Address`先ほど作成したテンプレートを使用して、表示、`Address`表示が次のようになるよう、複合型。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>モデルの表示形式およびテンプレートを指定する方法は概要:

次のアプローチを使用して形式またはモデル プロパティのテンプレートを指定できますを見てきました。

- 適用する、`DisplayFormat`属性をモデル内のプロパティです。 たとえば、次のコードでは、時間を除いた表示される日付が発生します。

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- 適用する、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性をモデルとデータ型の指定では、プロパティにします。 たとえば、次のコードが、日付、時刻なしで表示します。

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    アプリケーションが含まれている場合、 *date.cshtml*でテンプレート、 *views \shared\displaytemplates*フォルダーまたは*Views\Movies\DisplayTemplates*フォルダー、そのテンプレートレンダリングに使用される、`DateTime`プロパティです。 それ以外の場合、組み込みの ASP.NET テンプレート システムは、日付としてプロパティを表示します。
- 画面テンプレートを作成する、 *views \shared\displaytemplates*フォルダーまたは*Views\Movies\DisplayTemplates*フォルダーの名前には、書式を設定するデータ型が一致します。 たとえば、ことを説明する、 *Views\Shared\DisplayTemplates\DateTime.cshtml*表示するために使用された`DateTime`モデルに属性を追加してビューをビューにすべてのマークアップを追加することがなく、モデルのプロパティです。
- 使用して、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)モデル プロパティを表示するテンプレートを指定するには、モデルの属性です。
- 明示的に追加するテンプレート名を表示、 [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)ビューで呼び出します。

使用するアプローチは、アプリケーションで実行する必要がありますに依存します。 必要な書式設定の種類だけを取得する方法を混在させることは珍しくことはできません。

次のセクションでは少し変えて、入力する方法をカスタマイズするデータを表示する方法をカスタマイズから移動します。 日付を指定する便利な方法を提供するためにアプリケーションの編集ビューに jQuery datepicker フックされます。

>[!div class="step-by-step"]
[前へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[次へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
