---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: '繰り返し #3 – フォーム検証を追加する (VB) |Microsoft Docs'
author: microsoft
description: 3 番目のイテレーションでは、基本的なフォーム検証を追加します。 ユーザーは、必要なフォームのフィールドを完了しなくても、フォームを送信できないようにようにします。 私たちも emai を検証しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d4a1806742caf13f4778ef3588710136814c993
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368720"
---
<a name="iteration-3--add-form-validation-vb"></a>繰り返し #3 – フォーム検証を追加する (VB)
====================
によって[Microsoft](https://github.com/microsoft)

[コードをダウンロードします。](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> 3 番目のイテレーションでは、基本的なフォーム検証を追加します。 ユーザーは、必要なフォームのフィールドを完了しなくても、フォームを送信できないようにようにします。 また、電子メール アドレスと電話番号を検証しました。


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>ASP.NET MVC 連絡先管理アプリケーション (VB) の構築
  

このチュートリアル シリーズでは、開始から終了に全体連絡先管理アプリケーションを構築します。 Contact Manager アプリケーションは、ユーザーの一覧については店舗連絡先情報の名前、電話番号、電子メール アドレスにするようにことができます。

複数のイテレーションにおける、アプリケーションを構築します。 反復処理ごとに、アプリケーション徐々 に向上します。 この複数のイテレーションのアプローチの目的は、各変更の理由を理解するためです。

- 繰り返し #1 - は、アプリケーションを作成します。 最初のイテレーションを作成、連絡先マネージャー最も簡単な方法で考えられるします。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。

- 繰り返し #2 - は、素敵に見えるアプリケーションを作成します。 このイテレーションで、アプリケーションの見た目を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。

- 繰り返し #3 - フォーム検証を追加します。 3 番目のイテレーションでは、基本的なフォーム検証を追加します。 ユーザーは、必要なフォームのフィールドを完了しなくても、フォームを送信できないようにようにします。 また、電子メール アドレスと電話番号を検証しました。

- 繰り返し #4 - は、アプリケーションを疎結合を作成します。 この 3 番目のイテレーションで、保守し、Contact Manager アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかの利点を実行します。 たとえば、Repository パターンと依存関係の注入パターンを使用するようにアプリケーションをリファクタリングします。

- 繰り返し #5 - 単体テストを作成します。 5 番目のイテレーションでアプリケーションと簡単に維持単体テストを追加して変更します。 データ モデル クラスの模擬テストを実行し、コント ローラーと検証ロジックの単体テストをビルドします。

- 繰り返し #6 - は、テスト駆動開発を使用します。 この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加には、まず単体テストの記述、単体テストに対してコードを記述します。 このイテレーションは、連絡先グループを追加します。

- 繰り返し #7 - Ajax 機能を追加します。 7 番目のイテレーションで改良、応答性と、アプリケーションのパフォーマンスの Ajax のサポートを追加します。


## <a name="this-iteration"></a>このイテレーション

Contact Manager アプリケーションの 2 番目のイテレーションでは、基本的なフォーム検証を追加します。 により、ユーザーが必要なフォーム フィールドの値を入力しなくても、連絡先を送信できないようにします。 また、電話番号、電子メール アドレスが (図 1 参照) を検証しました。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**図 01**: 検証を使用して、フォーム ([フルサイズの画像を表示する をクリックします](iteration-3-add-form-validation-vb/_static/image2.png))。


このイテレーションでは、コント ローラー アクションに直接検証ロジックを追加します。 一般に、これは、ASP.NET MVC アプリケーションに検証を追加することをお勧めの方法ではありません。 別のアプリケーションの検証ロジックを配置するほうが効果的[サービス層](http://martinfowler.com/eaaCatalog/serviceLayer.html)します。 次のイテレーションは、アプリケーションを保守しやすくなりますように Contact Manager アプリケーションをリファクタリングします。

このイテレーションで、簡単に記述するすべての検証コードを手動で。 自分たちの検証コードを記述する、する代わりに、検証フレームワークの利点を取得できます。 します。 たとえば、ASP.NET MVC アプリケーションの検証ロジックを実装するために、Microsoft エンタープライズ ライブラリ検証アプリケーション ブロック (VAB) を使用できます。 詳細については、検証アプリケーション ブロックを参照してください。

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>ビューを作成する検証の追加

検証ロジックを作成ビューに追加することで開始 s を使用できます。 さいわい、Visual Studio を使用したビューを作成する、生成されたため、ビューを作成する既に含まれていますすべての検証メッセージを表示するために必要なユーザー インターフェイス ロジック。 リスト 1 で、作成ビューが含まれています。

**Listing 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

フィールドの検証エラー クラスは、Html.ValidationMessage() ヘルパーによってレンダリングされる出力のスタイル設定に使用されます。 入力検証エラー クラスは、Html.TextBox() ヘルパーによって表示されるテキスト ボックス (入力) のスタイル設定に使用されます。 検証の概要-エラー クラスは、Html.ValidationSummary() ヘルパーによって表示される順序なしリストをスタイル設定に使用されます。

> [!NOTE] 
> 
> 検証エラー メッセージの外観をカスタマイズするには、このセクションで説明されているスタイル シートのクラスを変更することができます。


## <a name="adding-validation-logic-to-the-create-action"></a>検証ロジックを追加する、アクションの作成

現在のところ、作成ビューは検証エラー メッセージをすべてのメッセージを生成するためのロジックを作成していないために表示されません。 検証エラー メッセージを表示するためには、ModelState にエラー メッセージを追加する必要があります。

> [!NOTE] 
> 
> UpdateModel() メソッドのエラー メッセージに追加 ModelState 自動的にフォーム フィールドの値をプロパティに割り当てエラーがある場合にします。 たとえば、"apple"の文字列を DateTime 値を受け入れる BirthDate プロパティに割り当てるしようとした場合、UpdateModel() メソッドに追加しますエラー ModelState。


リスト 2 で修正された Create() メソッドには、新しい連絡先は、データベースに挿入される前に、Contact クラスのプロパティを検証する新しいセクションが含まれています。

**2 - Controllers\ContactController.vb (検証で作成) を一覧表示します。**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

検証のセクションでは、次の 4 つの個別の検証規則を適用します。

- FirstName プロパティ、長さが 0 より大きい必要があります (およびスペースのみが構成することができません)
- LastName プロパティの長さが 0 より大きい必要があります (およびスペースのみが構成することができません)
- 電話プロパティに値が設定されている場合 (長さが 0 より大きい値を持つ) 電話プロパティ正規表現と一致する必要があります。
- 電子メール プロパティに値が設定されている場合 (長さが 0 より大きい値を持つ) し、電子メールのプロパティが正規表現に一致する必要があります。

検証規則違反がある場合に、エラー メッセージが AddModelError() メソッドのヘルプで ModelState に追加されます。 ModelState にメッセージを追加する場合は、プロパティの名前と、検証エラー メッセージのテキストを提供します。 このエラー メッセージは、Html.ValidationSummary() および Html.ValidationMessage() ヘルパー メソッドによって、ビューに表示されます。

検証規則が実行された後は、ModelState の IsValid プロパティがチェックします。 ModelState に検証エラー メッセージが追加されたときに、IsValid プロパティが false を返します。 検証に失敗した場合は、エラー メッセージを使用したフォームを作成するが再表示されます。

> [!NOTE] 
> 
> 正規表現のリポジトリから電話番号、電子メール アドレスを検証するための正規表現が表示されました [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>編集の操作に検証ロジックを追加します。

Edit() アクションでは、連絡先を更新します。 Edit() アクションは、Create() アクションと正確に同じ検証を実行する必要があります。 同じ検証コードを複製するのではなく Create() と Edit() の両方のアクションが同じ検証メソッドを呼び出せるように、連絡先のコント ローラーをリファクタリングする必要があります。

変更後にお問い合わせくださいコント ローラー クラスは、リスト 3 に含まれます。 このクラスには、Create() と Edit() の両方のアクション内で呼び出される新しい ValidateContact() メソッドがあります。

**3 - Controllers\ContactController.vb を一覧表示します。**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>まとめ

このイテレーションでは、Contact Manager アプリケーションを基本的なフォーム検証を追加しました。 この検証ロジックでは、ユーザーが新しい連絡先を送信したり、FirstName および LastName プロパティの値を指定せず、既存の連絡先を編集できないようにします。 さらに、ユーザーは、有効な電話番号と電子メール アドレスを指定する必要があります。

このイテレーションで最も簡単な方法で、Contact Manager アプリケーションに検証ロジックをしました。 ただしに、コント ローラー ロジック、検証ロジックを混在させる問題が発生私たちにとって長期的にします。 アプリケーションの保守し、時間の経過と共に変更をより困難になります。

次のイテレーションで、検証ロジックとデータベース アクセス ロジックをコント ローラーからにリファクタリングされます。 柔軟に結合し、保守性がアプリケーションを作成することを有効にするソフトウェア設計の原則をいくつかの利点がご説明しましょう。

> [!div class="step-by-step"]
> [前へ](iteration-2-make-the-application-look-nice-vb.md)
> [次へ](iteration-4-make-the-application-loosely-coupled-vb.md)
