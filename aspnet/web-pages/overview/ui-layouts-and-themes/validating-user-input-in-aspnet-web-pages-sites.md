---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: ページ (Razor) サイトの ASP.NET Web におけるユーザー入力の検証 |Microsoft ドキュメント
author: tfitzmac
description: ユーザーから取得した情報を検証する方法を説明&mdash;は、確認するユーザー入力を有効な html 形式で情報でのフォーム、名前を付けて.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 34f703e6db70ac79c22f4a50d4cfd4e2326b4c74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899179"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web Pages (Razor) サイトにおけるユーザー入力の検証
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> ユーザーから取得した情報を検証する方法を説明&mdash;は、確認するユーザー入力を有効な html 形式で情報フォームの ASP.NET Web Pages (Razor) サイトにします。
> 
> 学習する内容。
> 
> - ユーザーの入力になっていることを確認する方法では、定義されている検証条件と一致します。
> - すべての検証テストが成功するかどうかを判断する方法。
> - 検証エラーを表示する方法 (およびそれらの書式を設定する方法)。
> - ユーザーから直接取得しないデータを検証する方法。
> 
> これらは、ASP.NET のアーティクルで導入された概念をプログラミングします。
> 
> - `Validation`ヘルパー。
> - `Html.ValidationSummary`と`Html.ValidationMessage`メソッドです。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも動作します。


この記事は、次のセクションで構成されています。

- [ユーザー入力の検証の概要](#Overview_of_User_Input_Validation)
- [ユーザー入力の検証](#Validating_User_Input)
- [クライアント側の検証を追加します。](#Adding_Client-Side_Validation)
- [検証エラーの書式設定](#Formatting_Validation_Errors)
- [ユーザーから直接取得しないデータの検証](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>ユーザー入力の検証の概要

ページで情報を入力するユーザーを依頼するかどうかは — たとえば、フォームに — が入力する値が有効であるかどうかを確認することが重要です。 たとえば、重要な情報が欠落しているフォームを処理します。

ユーザーは、HTML フォームに値を入力、入力値は文字列です。 多くの場合、必要な値は整数や日付のような他のデータ型です。 したがっても、ユーザーが入力した値を正しく、適切なデータ型に変換できるかどうかを確認する必要があります。

値に特定の制限を設定することもできます。 ユーザーは、整数値を正しく入力して、場合でもはたとえば、値が特定の範囲内にあるかどうかを確認する必要があります。

![CSS スタイル クラスを使用する検証エラー](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **重要な**セキュリティの重要なユーザー入力の検証もします。 ユーザーがフォームに入力できる値を制限すると、サイトのセキュリティを損なう可能性を値を入力できます誰か可能性を低くします。


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>ユーザー入力の検証

ASP.NET Web Pages 2 で使用できます、`Validator`ユーザー入力をテストするためのヘルパー。 基本的な方法は、次の操作です。

1. どの入力を検証する要素 (フィールド) を決定します。

    通常の値を検証する`<input>`フォーム内の要素。 ただし、ことをお勧めも入力をすべての入力を検証するように制約付きの要素から取得した、 `<select>`  ボックスの一覧です。 これにより、ユーザーがページ上のコントロールをバイパスおよびフォームを送信しないことを確認します。
2. ページのコードでのメソッドを使用して要素を入力ごとに個別の検証チェックを追加、`Validation`ヘルパー。

    必須フィールドを確認するには、使用`Validation.RequireField(field, [error message])`(用、個々 のフィールド) または`Validation.RequireFields(field1, field2, ...))`(のフィールドの一覧)。 その他の種類の検証では、使用`Validation.Add(field, ValidationType)`です。 `ValidationType`、これらのオプションを使用することができます。

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. チェックして、検証が渡されるかどうかを確認して、ページが送信されると、 `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    検証エラーがある場合は、通常のページの処理をスキップします。 たとえば、ページの目的がデータベースを更新する場合は、しないをするまでを行うすべての検証エラーが修正されました。
4. 検証エラーがある場合はエラーでメッセージを表示、ページのマークアップを使用して`Html.ValidationSummary`または`Html.ValidationMessage`、またはその両方です。

次の例では、次の手順について説明するページを示します。

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

検証の動作を確認するには、このページを実行し、意図的に間違いを犯さないです。 たとえば、ここでは、ページの外観を忘れた場合、コースの名前を入力する入力した場合、無効な日付を入力するとします。

![表示されたページの検証エラー](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>クライアント側の検証を追加します。

ユーザーがページを送信後に既定では、ユーザー入力の検証: サーバー コードで、検証の実行は、します。 この方法の欠点は、あるユーザーかわからないページを送信した後までエラー活かせることです。 フォームは、long または複合型には、ページの送信後にのみ、エラーを報告できるかどうをユーザーに便利です。

クライアント スクリプトで検証を実行するサポートを追加できます。 その場合は、検証は、ユーザーがブラウザーで作業を実行します。 たとえば、値が整数を指定する必要がありますを指定するとします。 整数以外の値を入力すると、ユーザーが、入力フィールドのままとすぐに、エラーが報告されます。 ユーザーは、それらに便利です、即時のフィードバックを取得します。 クライアント ベースの検証は、複数のエラーを修正するフォームを送信するユーザーが持っている回数を減らすこともできます。

> [!NOTE]
> クライアント側の検証を使用する場合でも検証はサーバー コードでも、常に実行されます。 ユーザー クライアント ベースの検証をバイパスする場合に、サーバー コードの検証を実行するはセキュリティのメジャー、です。


1. ページで、次の JavaScript ライブラリを登録します。  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   ライブラリの 2 つのコンテンツ配信ネットワーク (CDN) から読み込み可能なため、コンピューターまたはサーバー上に必ずしも必要はありません。 ただしのローカル コピーが必要*jquery.validate.unobtrusive.js*です。 既に動作していない WebMatrix テンプレートを使用する (と同様に**スターター サイト**) ライブラリが含まれている、基になっている Web Pages サイトを作成する**スターター サイト**です。 コピーし、 *.js*ファイルを現在のサイトです。
2. マークアップでは、次の情報を検証している、各要素に対して呼び出しを追加して`Validation.For(field)`です。 このメソッドは、クライアント側の検証で使用される属性を出力します。 (実際の JavaScript コードの出力ではなく、メソッドがなどの属性を出力`data-val-...`です。 これらの属性控えめなクライアントの検証をサポートを作業を行うには jQuery を使用します。)

次のページでは、前の例にクライアントの検証機能を追加する方法を示します。

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

すべての検証チェックがクライアント上で実行します。 具体的には、データ型の検証 (整数、日付、およびなど) は、クライアントで実行しないでください。 次のチェックは、クライアントとサーバーの両方で動作します。

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

この例では、有効な日付のテストは、クライアント コードで機能しません。 ただし、テストは、サーバー コードで実行されます。

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>検証エラーの書式設定

予約済みの名前は次の CSS クラスを定義することによって検証エラーを表示する方法を制御できます。

- `field-validation-error`。 出力を定義、`Html.ValidationMessage`メソッドはエラーを表示するいるとします。
- `field-validation-valid`。 出力を定義、`Html.ValidationMessage`メソッドはエラーがないとします。
- `input-validation-error`。 定義する方法`<input>`要素は、エラーがある場合にレンダリングされます。 (このクラスを使用して、背景色を設定するなど、&lt;入力&gt;要素の値が有効でない場合は異なる色にします)。この CSS クラスは、ASP.NET Web Pages 2) の「クライアントの検証中にのみ使用されます。
- `input-validation-valid`。 外観を定義`<input>`要素エラーがない場合。
- `validation-summary-errors`。 出力を定義、`Html.ValidationSummary`メソッドがエラーの一覧を表示します。
- `validation-summary-valid`。 出力を定義、`Html.ValidationSummary`メソッドはエラーがないとします。

次`<style>`ブロックのエラー条件のルールを示しています。

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

記事の前半の例のページにこのスタイル ブロックを含めると、エラーの表示は次の図のようになります。

![CSS スタイル クラスを使用する検証エラー](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> ASP.NET Web Pages 2 でないクライアントの検証を使用している場合、CSS クラス、`<input>`要素 (`input-validation-error`と`input-validation-valid`何も影響がありません。


### <a name="static-and-dynamic-error-display"></a>静的および動的なエラーの表示

CSS 規則には、ペアなど`validation-summary-errors`と`validation-summary-valid`です。 これらのペアを使用して、両方の条件の規則を定義できます: エラーが発生し、"normal"(エラーではない) 状態です。 エラーがない場合でも、エラーの表示のマークアップが常に表示されることを理解しておく必要があります。 たとえば、ページがある場合、`Html.ValidationSummary`マークアップ内のメソッド、ページのソースには、次のマークアップ ページが最初に要求されたときにもします。

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

言い換えると、`Html.ValidationSummary`メソッドは常に表示、`<div>`要素と一覧は、エラー リストが空の場合でもします。 同様に、`Html.ValidationMessage`メソッドは常に表示、`<span>`要素を個々 のフィールド エラー、エラーが発生しなかった場合でものプレース ホルダーとして。

状況によっては、エラー メッセージを表示するページのフローとなる可能性が要素を移動するページにします。 最後に CSS ルール`-valid`この問題を防ぐことができます、レイアウトを定義できます。 たとえば、定義`field-validation-error`と`field-validation-valid`固定するには両方が同じサイズです。 このように、フィールドの表示領域は静的であり、エラー メッセージが表示されている場合、ページのフローは変更されません。

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>ユーザーから直接取得しないデータの検証

HTML フォームから直接取得しない情報を検証がある場合があります。 典型的な例では、次の例のように、クエリ文字列の値が渡される場所のページを示します。

`http://server/myapp/EditClassInformation?classid=1022`

確認するこの例では、ページに渡される値 (ここでは、の値の 1022 `classid`) が有効です。 直接使用することはできません、`Validation`この検証を実行するためのヘルパー。 ただし、検証エラー メッセージを表示する機能など、検証システムの他の機能を使用することができます。

> [!NOTE] 
> 
> **重要な**から取得する値を常に検証*任意*フォーム フィールドの値、クエリ文字列値、および cookie の値を含むソース。 (おそらく悪意のある目的の場合) のこれらの値を変更するユーザーを簡単になります。 したがって、アプリケーションを保護するためにこれらの値を確認する必要があります。


次の例では、クエリ文字列で渡される値を検証する方法を示します。 コードでは、値が空でないことと、整数であることをテストします。

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

要求がフォームの送信ではない場合に、テストが実行されるように注意してください (`if(!IsPost)`)。 このテストは、ページが要求されると、最初の時間を渡す場合しますが、いない場合、要求フォームの送信。

このエラーを表示することができますに追加するエラー検証エラーの一覧を呼び出して`Validation.AddFormError("message")`です。 ページへの呼び出しが含まれている場合、`Html.ValidationSummary`メソッド、エラーがユーザー入力の検証エラーと同じように、表示されます。

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>その他のリソース

[ASP.NET Web Pages サイトでの HTML フォームの使用](https://go.microsoft.com/fwlink/?LinkID=202892)
