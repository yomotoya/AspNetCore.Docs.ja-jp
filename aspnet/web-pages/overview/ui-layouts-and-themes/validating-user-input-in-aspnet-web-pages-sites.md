---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: ページ (Razor) サイトを ASP.NET Web でユーザー入力の検証 |Microsoft Docs
author: Rick-Anderson
description: この記事では、ユーザーから取得した情報を検証する方法をについて説明します&mdash;は、ユーザーが入力されることを確認して有効する HTML 内の情報でのフォーム、名前を付けて.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 8f049adce33e452896b5e2a444635ff30d18e480
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021522"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web Pages (Razor) サイトでユーザー入力の検証
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ユーザーから取得した情報を検証する方法をについて説明します&mdash;は、ユーザーが入力されることを確認して有効する HTML 内の情報フォームの ASP.NET Web Pages (Razor) サイトでします。
> 
> 学習内容。
> 
> - ユーザーの入力になっていることを確認する方法では、定義した条件と一致します。
> - すべての検証テストに合格したかどうかを判断する方法。
> - 検証エラーを表示する方法 (およびそれらの書式を設定する方法)。
> - ユーザーから直接提供しないデータを検証する方法。
> 
> これらは、プログラミングの概念は、情報の記事で導入された ASP.NET:
> 
> - `Validation`ヘルパー。
> - `Html.ValidationSummary`と`Html.ValidationMessage`メソッド。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも機能します。


この記事は、次のセクションで構成されています。

- [ユーザー入力の検証の概要](#Overview_of_User_Input_Validation)
- [ユーザー入力の検証](#Validating_User_Input)
- [クライアント側の検証を追加します。](#Adding_Client-Side_Validation)
- [書式設定の検証エラー](#Formatting_Validation_Errors)
- [ユーザーから直接提供しないデータの検証](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>ユーザー入力の検証の概要

ページの情報を入力するユーザーを依頼するかどうか: などをフォームに: が入力する値が有効であることを確認することが重要です。 たとえば、重要な情報が不足しているフォームを処理したくないです。

ユーザーは、HTML フォームに値を入力、入力値は文字列になります。 多くの場合、必要な値は整数や日付などの他のデータ型です。 そのため、また、ユーザーが入力した値を正しく、適切なデータ型に変換できるかどうかを確認する必要があります。

特定の制限は、値にもあります。 場合でも、ユーザーが正しく、整数の入力はたとえば、値が特定の範囲内にあるかどうかを確認する必要があります。

![CSS スタイル クラスを使用する検証エラー](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **重要な**セキュリティの重要なもユーザー入力を検証します。 ユーザーがフォームに入力できる値を制限する場合は、サイトのセキュリティを脅かす可能性のある値を入力だれかができます可能性を低くします。


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>ユーザー入力の検証

ASP.NET Web ページ 2 では、使用することができます、`Validator`ユーザー入力をテストするためのヘルパー。 基本的な方法は、次の操作です。

1. 検証する要素 (フィールド) を入力するかを決定します。

    通常の値を検証する`<input>`フォーム内の要素。 ただし、制約付きの要素のように由来するすべての入力の検証は、入力もをお勧めは、`<select>`一覧。 これにより、ユーザーがページ上のコントロールをバイパスおよびフォームを送信しないことを確認します。
2. ページのコードでのメソッドを使用して要素を入力ごとに個別の検証チェックを追加、`Validation`ヘルパー。

    必須フィールドを確認するには、使用`Validation.RequireField(field, [error message])`(個々 のフィールド) に対するまたは`Validation.RequireFields(field1, field2, ...))`(のフィールドの一覧)。 その他の種類の検証では、使用`Validation.Add(field, ValidationType)`します。 `ValidationType`、これらのオプションを使用することができます。

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
3. ページが送信されたときにチェックして、検証に合格するかどうかを確認`Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    検証エラーがある場合は、通常のページの処理をスキップします。 たとえば、ページの目的は、データベースを更新する場合に、しないするまで、すべての検証エラーが修正されました。
4. 検証エラーがある場合はエラーでメッセージを表示、ページのマークアップを使用して`Html.ValidationSummary`または`Html.ValidationMessage`、またはその両方です。

次の例では、次の手順を説明するページを示します。

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

検証の動作を確認するには、このページを実行し、間違いを意図的にします。 たとえば、ここでは、ページがどのように入力すると、コース名を入力する忘れた場合、無効な日付を入力するとします。

![レンダリングされたページの検証エラー](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>クライアント側の検証を追加します。

ユーザーがページを送信した後、既定でユーザー入力の検証-は、検証は、サーバー コードで実行します。 このアプローチの欠点は、ユーザーがページを送信した後までエラーを作成したことを知ってしないいるです。 フォームが長であるか、または複雑な場合、ページが送信された後にのみ、エラーを報告できますをユーザーに便利です。

クライアント スクリプトで検証を実行するサポートを追加することができます。 その場合は、検証は、ユーザーがブラウザーで作業を実行します。 たとえば、値が整数を指定する必要がありますを指定するとします。 整数以外の値を入力すると、ユーザーが、入力フィールドを離れると、すぐに、エラーが報告されます。 ユーザーにとって便利なは、即時のフィードバックを取得します。 クライアント ベースの検証をユーザーが持つ複数のエラーを修正するフォームを送信する時間の数を減らすことができます。

> [!NOTE]
> クライアント側の検証を使用する場合でも、検証はサーバー コードでも、常に実行されます。 ユーザーがクライアント ベースの検証をバイパスする場合に、セキュリティ対策はサーバー コードで検証を実行します。


1. ページで、次の JavaScript ライブラリを登録します。  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   ライブラリの 2 つの方法は、コンピューターまたはサーバー上にしなくても、コンテンツ配信ネットワーク (CDN) から読み込み可能なです。 ただしのローカル コピーが必要*jquery.validate.unobtrusive.js*します。 かどうかを既に使用していない WebMatrix テンプレート (など**スターター サイト**) ライブラリが含まれているに基づいている Web ページ サイトを作成する**スターター サイト**します。 コピーし、 *.js*ファイルを現在のサイト。
2. マークアップでは、次のように検証しているの各要素に対して呼び出しを追加`Validation.For(field)`します。 このメソッドは、クライアント側の検証で使用される属性を出力します。 (実際の JavaScript コードの出力ではなく、メソッドはなどの属性を出力`data-val-...`します。 これらの属性控えめなクライアントの検証をサポートする jQuery を使用して作業を行います。)

次のページでは、前の例にクライアントの検証機能を追加する方法を示します。

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

すべての検証チェックがクライアントで実行します。 具体的には、データ型の検証 (整数、日付、およびなど) は、クライアントで実行しないでください。 次のチェックは、クライアントとサーバーの両方で機能します。

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

この例では有効な日付のテストは、クライアント コードで機能しません。 ただし、サーバー コードで、テストが実行されます。

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>書式設定の検証エラー

予約済みの名前は次の CSS クラスを定義することで検証エラーを表示する方法を制御できます。

- `field-validation-error`。 出力を定義、`Html.ValidationMessage`メソッドのエラーを表示している場合。
- `field-validation-valid`。 出力を定義、`Html.ValidationMessage`メソッドのエラーがない場合。
- `input-validation-error`。 定義する方法`<input>`エラーがある場合に要素が表示されます。 (の背景色を設定するこのクラスを使用するなど、&lt;入力&gt;要素を別の色を使用している場合、その値が無効です)。この CSS クラスは、(ASP.NET Web ページ 2) でクライアントの検証中にのみ使用されます。
- `input-validation-valid`。 外観を定義`<input>`要素エラーがない場合。
- `validation-summary-errors`。 出力を定義、`Html.ValidationSummary`メソッドがエラーの一覧を表示することができます。
- `validation-summary-valid`。 出力を定義、`Html.ValidationSummary`メソッドのエラーがない場合。

次`<style>`ブロックには、エラー条件のルールが表示されます。

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

このスタイルのブロックを含めるには、記事の前半で例のページで、エラーの表示は次の図のようになります。

![CSS スタイル クラスを使用する検証エラー](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> に対して、CSS クラスを ASP.NET Web ページ 2 で、クライアントの検証を使用していない場合、`<input>`要素 (`input-validation-error`と`input-validation-valid`影響はありません。


### <a name="static-and-dynamic-error-display"></a>静的および動的なエラーの表示

CSS 規則などはペアになって`validation-summary-errors`と`validation-summary-valid`します。 これらのペアを使用して、両方の条件の規則を定義できます。 エラー状態と、"normal"(エラーではない) 条件。 エラーがない場合でも、エラーの表示のマークアップが常に表示されることを理解しておく必要があります。 たとえば、次のページがある、`Html.ValidationSummary`メソッドのマークアップで、ページのソースには、次のマークアップ、ページが初めて要求された場合でも。

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

つまり、`Html.ValidationSummary`メソッドは常に表示、`<div>`要素と一覧は、エラーの一覧が空の場合でも。 同様に、`Html.ValidationMessage`メソッドは常に表示、`<span>`要素エラーがない場合でも、個々 のフィールド エラーのプレース ホルダーとして。

場合によっては、エラー メッセージを表示するページのリフローして内を移動するページの要素が発生することができます。 CSS 規則で終わる`-valid`この問題を回避するのに役立つレイアウトを定義することができます。 たとえば、定義`field-validation-error`と`field-validation-valid`両方に同じ固定サイズです。 これにより、フィールドの表示領域は静的であり、エラー メッセージが表示されている場合、ページ フローは変更されません。

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>ユーザーから直接提供しないデータの検証

HTML フォームから直接取得しない情報を検証するがある場合があります。 典型的な例では、値が次の例のように、クエリ文字列で渡されるページを示します。

`http://server/myapp/EditClassInformation?classid=1022`

確認するこの例では、ページに渡される値 (ここでは、の値の 1022 `classid`) は有効です。 直接使用することはできません、`Validation`この検証を実行するためのヘルパー。 ただし、検証エラー メッセージを表示する機能など、検証システムの他の機能を使用することができます。

> [!NOTE] 
> 
> **重要な**から取得した値を常に検証*任意*フォーム フィールドの値、クエリ文字列値、cookie の値を含むソース。 (おそらくの悪意のある目的で) これらの値を変更するユーザーを簡単になります。 したがって、アプリケーションを保護するためにこれらの値を確認する必要があります。


次の例では、クエリ文字列で渡される値を検証する方法を示します。 コードでは、値が空でないことと、これは、整数であることをテストします。

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

要求はフォームの送信時に、テストの実行に注意してください (`if(!IsPost)`)。 このテストは、ページが要求されると、初めてを渡す場合しますが、ない場合、要求フォームの送信です。

このエラーを表示することができますを追加するエラーの検証エラーの一覧に`Validation.AddFormError("message")`します。 ページへの呼び出しが含まれている場合、`Html.ValidationSummary`メソッド、エラーがユーザー入力の検証エラーと同じように、表示されます。

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>その他のリソース

[ASP.NET Web Pages サイトでの HTML フォームの操作](https://go.microsoft.com/fwlink/?LinkID=202892)
