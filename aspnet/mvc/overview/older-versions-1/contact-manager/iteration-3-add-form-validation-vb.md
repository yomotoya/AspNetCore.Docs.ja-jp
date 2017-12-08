---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: "イテレーション 3 – 追加のフォーム検証 (VB) |Microsoft ドキュメント"
author: microsoft
description: "3 番目のイテレーションは、基本フォーム検証を追加します。 おは人が必要なフォームのフィールドを完了しなくても、フォームを送信することを防ぐ。 Emai も検証する."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: ffb502be3037e787d79bbd1e83b93cd0b34dca6a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="iteration-3--add-form-validation-vb"></a>イテレーション 3 – 追加のフォーム検証 (VB)
====================
によって[Microsoft](https://github.com/microsoft)

[コードをダウンロードします。](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> 3 番目のイテレーションは、基本フォーム検証を追加します。 おは人が必要なフォームのフィールドを完了しなくても、フォームを送信することを防ぐ。 私たちも電子メール アドレスと電話番号を検証します。


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>連絡先管理 ASP.NET MVC アプリケーション (VB) のビルド
  

この一連のチュートリアルは、連絡先管理アプリケーション全体が開始されてから完了するを構築します。 お問い合わせのマネージャー アプリケーションでは、人のユーザーの一覧については使用すると、連絡先情報の名前、電話番号、電子メール アドレスを格納できます。

私たちは、複数のイテレーションにおける、アプリケーションを構築します。 各イテレーションで、アプリケーション、徐々 に向上します。 この複数のイテレーション アプローチの目的は、各変更の理由を理解するためです。

- イテレーション 1 には、アプリケーションを作成します。 最初のイテレーションでお連絡先のマネージャー最も簡単な方法で可能なを作成します。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。

- イテレーション 2 では、素敵に見えるアプリケーションを作成します。 このイテレーションで、アプリケーションの外観を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。

- イテレーション 3 - フォーム検証を追加します。 3 番目のイテレーションは、基本フォーム検証を追加します。 おは人が必要なフォームのフィールドを完了しなくても、フォームを送信することを防ぐ。 私たちも電子メール アドレスと電話番号を検証します。

- 4: イテレーションは、疎結合アプリケーションを作成します。 この 3 番目のイテレーションで利用の保守し、連絡先のマネージャー アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかのです。 たとえば、リポジトリ パターンと依存関係の挿入のパターンを使用するようにアプリケーションをリファクターします。

- イテレーション #5 - 単体テストを作成します。 5 番目のイテレーションでおやすく、アプリケーションを維持し、単体テストを追加して変更できます。 データ モデル クラスを模擬表示し、コント ローラーと検証ロジックの単体テストをビルドします。

- イテレーション 6 - テスト駆動開発を使用します。 この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加おには、まず単体テストを記述し、単体テストに対してコードを記述します。 このイテレーションは、連絡先グループを追加します。

- イテレーション #7 - Ajax 機能を追加します。 7 番目のイテレーションでお、応答性およびパフォーマンスの向上、アプリケーションの Ajax のサポートを追加することで。


## <a name="this-iteration"></a>このイテレーション

連絡先のマネージャー アプリケーションの 2 番目のイテレーションは、基本フォーム検証を追加します。 ユーザーは、必要なフォーム フィールドの値を入力することがなく、連絡先を送信してからようにします。 私たちは、電話番号、電子メール アドレス (図 1 を参照してください) にも検証します。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**図 01**: 検証とフォーム ([フルサイズのイメージを表示するをクリックして](iteration-3-add-form-validation-vb/_static/image2.png))


このイテレーションに、コント ローラーのアクションに直接検証ロジックを追加します。 通常、これは、ASP.NET MVC アプリケーションに検証を追加することをお勧めではありません。 適切な方法は、別のアプリケーションの検証ロジックを配置する[サービス層](http://martinfowler.com/eaaCatalog/serviceLayer.html)です。 次のイテレーションでは、アプリケーションの保守が簡単にする連絡先のマネージャー アプリケーションをリファクタリングします。

このイテレーションで煩雑にならないように、記述のすべての検証コードを手動でします。 検証コードを自分で記述する代わりには、検証フレームワークの利点おがかかります。 たとえば、ASP.NET MVC アプリケーションの検証ロジックを実装するのに Microsoft エンタープライズ ライブラリ検証アプリケーション ブロック (VAB) を使用できます。 検証アプリケーション ブロックに関する詳細についてを参照してください。

[*http://msdn.microsoft.com/en-us/library/dd203099.aspx*](https://msdn.microsoft.com/en-us/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>ビューを作成する検証の追加

ビューを作成するに検証ロジックを追加することによって開始 s を使用できます。 さいわい、ビューを作成する Visual Studio で生成されたため、ビューを作成する既に含まれていますすべての検証メッセージを表示するために必要なユーザー インターフェイス ロジック。 ビューを作成するは、1 のリストに含まれます。

**1 - を一覧表示する \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

フィールドの検証エラーのクラスは Html.ValidationMessage() ヘルパーによって表示される出力のスタイル設定に使用されます。 入力の検証エラーのクラスは Html.TextBox() ヘルパーによって表示されるテキスト ボックス (入力) のスタイル設定に使用されます。 検証の概要-エラー クラスを使用して、Html.ValidationSummary() ヘルパーによって表示される順序なしリストのスタイルを設定します。

> [!NOTE] 
> 
> 検証エラー メッセージの外観をカスタマイズするには、このセクションで説明されているスタイル シートのクラスを変更することができます。


## <a name="adding-validation-logic-to-the-create-action"></a>検証ロジックを追加する、操作の作成

今すぐ,、ビューを作成するは、すべてのメッセージを生成するロジックがない書き込まため決して検証エラー メッセージを表示します。 検証エラー メッセージを表示するためには、ModelState にエラー メッセージを追加する必要があります。

> [!NOTE] 
> 
> UpdateModel() メソッドのエラー メッセージに追加 ModelState 自動的に、プロパティに、フォーム フィールドの値を割り当てることでエラーがある場合にします。 たとえば、"apple"の文字列を DateTime 値を受け入れる BirthDate プロパティに割り当てるしようとする場合、UpdateModel() メソッド エラーを追加 ModelState です。


リスト 2 で修正 Create() メソッドには、新しい連絡先は、データベースに挿入される前に、連絡先クラスのプロパティを検証するための新しいセクションが含まれています。

**2 - Controllers\ContactController.vb (検証で作成する) を一覧表示します。**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

検証のセクションでは、次の 4 つの個別の検証規則を適用します。

- FirstName プロパティの長さが 0 より大きい必要があります (とスペースだけので構成されていることはできません)
- LastName プロパティの長さが 0 より大きい必要があります (とスペースだけので構成されていることはできません)
- Phone プロパティの値が (長さが 0 より大きい値を持つ) し、Phone プロパティは、正規表現と一致する必要があります。
- 電子メール プロパティの値が (長さが 0 より大きい値を持つ)、Email プロパティは、正規表現と一致する必要があります。

検証規則の違反がある場合は、エラー メッセージが AddModelError() メソッドを利用して ModelState に追加されます。 ModelState にメッセージを追加する場合は、プロパティの名前と、検証エラー メッセージのテキストを提供します。 このエラー メッセージは、Html.ValidationSummary() および Html.ValidationMessage() ヘルパー メソッドによって、ビューに表示されます。

検証規則が実行された後の ModelState IsValid プロパティがチェックされます。 ModelState に検証エラー メッセージが追加されたときに、IsValid プロパティが false を返します。 検証に失敗した場合は、エラー メッセージにフォームを作成するが再表示されます。

> [!NOTE] 
> 
> 正規表現のリポジトリからの電話番号と電子メール アドレスを検証するための正規表現が[ *http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>編集の操作に検証ロジックを追加します。

Edit() 操作では、連絡先を更新します。 Edit() アクションは、Create() アクションと正確に同じ検証を実行する必要があります。 同じ検証コードを複製するには、代わりに、Create() および Edit() 操作の両方が同じ検証メソッドを呼び出すように連絡先コント ローラーをリファクターお必要があります。

変更された連絡先のコント ローラー クラスは、リスト 3 に含まれます。 このクラスには、Create() と Edit() アクション内で呼び出される新しい ValidateContact() メソッドがあります。

**3 - Controllers\ContactController.vb を一覧表示します。**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>概要

このイテレーションでは、この連絡先のマネージャー アプリケーションに基本的な形式の検証を追加しました。 この検証ロジックでは、ユーザーの新しい連絡先を送信または FirstName および LastName から構成プロパティの値を指定せず、既存の連絡先を編集できなくなります。 さらに、ユーザーは、有効な電話番号と電子メール アドレスを指定する必要があります。

このイテレーションにロジックを追加した、検証、連絡先のマネージャー アプリケーションに最も簡単な方法でします。 ただし、コント ローラー ロジックに、検証ロジックを混在させるによって問題が発生の長期的なです。 アプリケーションは管理や時間の経過と共に変更が困難になります。

次のイテレーションで、検証ロジックとデータベース アクセス ロジックをコント ローラー外にリファクタリングがします。 より柔軟に結合し、保守性がアプリケーションを作成することを有効にするソフトウェア設計の原則をいくつかの利点をみましょう。

>[!div class="step-by-step"]
[前へ](iteration-2-make-the-application-look-nice-vb.md)
[次へ](iteration-4-make-the-application-loosely-coupled-vb.md)
