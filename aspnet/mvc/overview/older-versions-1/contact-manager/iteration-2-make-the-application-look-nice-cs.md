---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: '繰り返し #2 – アプリケーションの外観を便利な (c#) を作成する |Microsoft Docs'
author: microsoft
description: このイテレーションで、アプリケーションの見た目を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: c8bbd20cb64fb27a0a6de2cdc14743f6961f4bf0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832810"
---
<a name="iteration-2--make-the-application-look-nice-c"></a>繰り返し #2 – アプリケーションの外観を便利な (c#) を作成します。
====================
によって[Microsoft](https://github.com/microsoft)

[コードをダウンロードします。](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> このイテレーションで、アプリケーションの見た目を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>連絡先管理 ASP.NET MVC アプリケーション (c#) の構築
  

このチュートリアル シリーズでは、開始から終了に全体連絡先管理アプリケーションを構築します。 Contact Manager アプリケーションは、ユーザーの一覧については店舗連絡先情報の名前、電話番号、電子メール アドレスにするようにことができます。

複数のイテレーションにおける、アプリケーションを構築します。 反復処理ごとに、アプリケーション徐々 に向上します。 この複数のイテレーションのアプローチの目的は、各変更の理由を理解するためです。

- 繰り返し #1 - は、アプリケーションを作成します。 最初のイテレーションを作成、連絡先マネージャー最も簡単な方法で考えられるします。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。

- 繰り返し #2 - は、素敵に見えるアプリケーションを作成します。 このイテレーションで、アプリケーションの見た目を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。

- 繰り返し #3 - フォーム検証を追加します。 3 番目のイテレーションでは、基本的なフォーム検証を追加します。 ユーザーは、必要なフォームのフィールドを完了しなくても、フォームを送信できないようにようにします。 また、電子メール アドレスと電話番号を検証しました。

- 繰り返し #4 - は、アプリケーションを疎結合を作成します。 この 4 番目のイテレーションで、保守し、Contact Manager アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかの利点を実行します。 たとえば、Repository パターンと依存関係の注入パターンを使用するようにアプリケーションをリファクタリングします。

- 繰り返し #5 - 単体テストを作成します。 5 番目のイテレーションでアプリケーションと簡単に維持単体テストを追加して変更します。 データ モデル クラスの模擬テストを実行し、コント ローラーと検証ロジックの単体テストをビルドします。

- 繰り返し #6 - は、テスト駆動開発を使用します。 この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加には、まず単体テストの記述、単体テストに対してコードを記述します。 このイテレーションは、連絡先グループを追加します。

- 繰り返し #7 - Ajax 機能を追加します。 7 番目のイテレーションで改良、応答性と、アプリケーションのパフォーマンスの Ajax のサポートを追加します。

## <a name="this-iteration"></a>このイテレーション

このイテレーションの目的は、Contact Manager アプリケーションの外観を向上させることです。 現時点では、連絡先マネージャーでは、(図 1 参照)、既定の ASP.NET MVC ビュー マスター ページとカスケード スタイル シートを使用します。 不適切な場合は、これらがないが t するその他のすべての ASP.NET MVC web サイトのように、連絡先のマネージャーをしないようにします。 カスタムのファイルでこれらのファイルを置換します。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**図 01**: ASP.NET MVC アプリケーションの既定の外観 ([フルサイズの画像を表示する をクリックします](iteration-2-make-the-application-look-nice-cs/_static/image2.png))。


このイテレーションでは、アプリケーションのビジュアル デ ザインを向上する 2 つの方法を説明します。 最初に、無料の ASP.NET MVC デザイン テンプレートをダウンロードする ASP.NET MVC の設計のギャラリーの利用する方法が説明します。 ASP.NET MVC デザイン ギャラリーでは、何もしないで、プロの web アプリケーションを作成することができます。

Contact Manager アプリケーションの ASP.NET MVC の設計のギャラリーからテンプレートを使用しないことにしました。 代わりに、本格的なデザイン会社によって作成されたカスタム デザインがありました。 このチュートリアルの 2 番目の部分で最終的な ASP.NET MVC の設計を作成する本格的なデザイン会社と一緒に作業する方法を説明します。

## <a name="the-aspnet-mvc-design-gallery"></a>ASP.NET MVC の設計のギャラリー

ASP.NET MVC の設計のギャラリーとは、Microsoft によって提供される無料のリソースです。 ASP.NET MVC のギャラリーにある次のアドレス。

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

ASP.NET MVC の設計のギャラリーでは、ASP.NET MVC プロジェクトで使用するためには、具体的には作成された無料の web サイト デザインのコレクションをホストします。 コミュニティのメンバーでは、設計がアップロードされます。 ギャラリーへの訪問者は、お気に入りの設計に投票できます (図 2 参照)。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**図 02**: [ASP.NET MVC の設計のギャラリー ([フルサイズの画像を表示する] をクリックします](iteration-2-make-the-application-look-nice-cs/_static/image4.png))。


このチュートリアルを執筆ギャラリーで最も人気のある設計、David Hauser 年 10 月を付けた設計です。 次の手順を実行して、ASP.NET MVC プロジェクトのこの設計を使用できます。

1. をクリックして、**ダウンロード**October.zip ファイルをコンピューターにダウンロードするボタンをクリックします。
2. ダウンロードした October.zip ファイルを右クリックし、をクリックして、**ブロック解除**(図 3 を参照してください) ボタンをクリックします。
3. 10 月という名前のフォルダーにファイルを解凍します。
4. 年 10 月のフォルダーに含まれるレイアウト フォルダーからすべてのファイルを選択で、ファイルを右クリックし、メニュー オプションを選択**コピー**します。
5. Visual Studio ソリューション エクスプ ローラー ウィンドウで ContactManager プロジェクト ノードを右クリックし、メニュー オプションを選択**貼り付け**(図 4 参照)。
6. Visual Studio のメニュー オプションを選択**編集の検索し、置換、クイック置換**と置換 *[MyProjectName]* で*ContactManager* (図 5 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**図 03**: web からダウンロードしたファイルのブロックを解除 ([フルサイズの画像を表示する をクリックします](iteration-2-make-the-application-look-nice-cs/_static/image6.png))。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**図 04**: ソリューション エクスプ ローラーでファイルを上書きする ([フルサイズの画像を表示する をクリックします](iteration-2-make-the-application-look-nice-cs/_static/image8.png))。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**図 05**: [ProjectName] に置き換えて ContactManager ([フルサイズの画像を表示する をクリックします](iteration-2-make-the-application-look-nice-cs/_static/image10.png))。


次の手順を完了すると、web アプリケーションは、新しいデザインを使用します。 図 6 ページは、年 10 月の設計と Contact Manager アプリケーションの外観を示しています。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**図 06**: 年 10 月のテンプレートを使用した ContactManager ([フルサイズの画像を表示する をクリックします](iteration-2-make-the-application-look-nice-cs/_static/image12.png))。


## <a name="creating-a-custom-aspnet-mvc-design"></a>カスタム ASP.NET MVC の設計を作成します。

ASP.NET MVC デザイン ギャラリーでは、さまざまなデザイン スタイルの適切な選択範囲には。 ギャラリーには、ASP.NET MVC アプリケーションの外観をカスタマイズする最も簡単に提供します。 また、もちろん、ギャラリーには、完全に無料になるという大きな利点。

ただし、web サイトの完全に独自のデザインを作成する必要があります。 その場合は、理にかなって web サイトのデザイン会社を使用します。 Contact Manager アプリケーションを設計するためには、この手法を採用することにしました。

イテレーション 1 Contact Manager を zip 形式し、デザイン会社にプロジェクトを送信します。 Visual Studio (残念なことにです) はなかった t 問題が発生することが、所有せずします。 Microsoft Visual Web Developer を無料でダウンロードすることができました、 [ https://www.asp.net ](https://www.asp.net) web サイトを Visual Web Developer で Contact Manager アプリケーションを開きます。 いくつかの日には、図 7 の設計を生成されると必要があります。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**図 07**: ASP.NET MVC の Contact Manager デザイン ([フルサイズの画像を表示する をクリックします](iteration-2-make-the-application-look-nice-cs/_static/image14.png))。


2 つの主なファイルの新しいデザインを行いました。 新しいカスケード スタイル シート ファイルと新しいビュー マスター ページ ファイルです。 ビュー マスター ページには、レイアウトとビューの ASP.NET MVC アプリケーションで共有のコンテンツが含まれています。 たとえば、ビュー マスター ページにヘッダー、ナビゲーション タブ、およびフッターに表示される図 7 されます。 Views \shared フォルダー Site.Master 既存ビュー マスター ページが上書きされましたデザイン社では、新しい Site.Master ファイルには

デザイン会社では、新しいカスケード スタイル シートとイメージのセットも作成されます。 コンテンツのフォルダーにこれらの新しいファイルを配置し、既存の Site.css ファイルを上書きします。 コンテンツのフォルダーには、すべての静的コンテンツを配置する必要があります。

新しいデザイン用連絡先マネージャーに編集および連絡先を削除するためのイメージが含まれていることを確認します。 連絡先の HTML の表では、各連絡先の横にある、編集、削除のイメージが表示されます。

最初に、これらのリンク、HTML でレンダリングされました。このような ActionLink() ヘルパー:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

Html.ActionLink() メソッドは、イメージ (HTML メソッドには、セキュリティ上の理由のリンクのテキストがエンコードされます) をサポートしていません。 そのため、このような Url.Action() への呼び出しに Html.ActionLink() への呼び出しは置き換えられます。

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

Html.ActionLink() メソッドは、全体の HTML ハイパーリンクを表示します。 Url.Action() メソッドなしの URL だけをレンダリング一方で、 &lt;、&gt;タグ。

さらに、新しいデザインが選択および選択されていない [両方] タブが含まれることに注意してください。 たとえば、図 8 に、**新しい連絡先の作成**タブが選択されていると、**個人用の連絡先** タブが選択されていません。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**図 08**: 選択し、タブを選択解除 ([フルサイズの画像を表示する をクリックします](iteration-2-make-the-application-look-nice-cs/_static/image16.png))。


選択および選択されていない [両方] タブの表示をサポートするには、MenuItemHelper という名前のカスタム HTML ヘルパーを作成しました。 このヘルパー メソッドをレンダリングするか、 &lt;li&gt;タグまたは&lt;li クラス =「選択」&gt;タグ ヘルパーに渡されるコント ローラーとアクション名に、現在のコント ローラーとアクションが対応するかどうかによって異なります。 リスト 1 で、MenuItemHelper のコードが含まれています。

**1 - Helpers\MenuItemHelper.cs を一覧表示します。**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

MenuItemHelper TagBuilder クラスで、内部的にを使用してビルド、 &lt;li&gt; HTML タグ。 TagBuilder クラスは、新しい HTML タグを構築する必要がある場合に使用できる非常に便利なユーティリティ クラスです。 属性の追加、CSS クラスの追加、Id を生成するおよび s タグを変更するためのメソッドを含む内部 HTML。

## <a name="summary"></a>まとめ

このイテレーションでは、ASP.NET MVC アプリケーションのビジュアル デ ザインを改善しました。 最初に、ASP.NET MVC のデザインを紹介しました。 ASP.NET MVC アプリケーションで使用できる ASP.NET MVC デザイン ギャラリーから無料のデザイン テンプレートをダウンロードする方法を学習しました。

次に、既定のカスケード スタイル シート ファイルおよびマスター ビュー ページ ファイルを変更してカスタム デザインを作成する方法について説明します。 新しい設計をサポートするためには、Contact Manager アプリケーションをわずかな変更を有効にする必要があります。 たとえば、MenuItemHelper 選択および選択されていない タブを表示するという名前の新しい HTML ヘルパーを追加します。

次のイテレーションで検証の非常に重要なサブジェクトに取り組みます。 ユーザーがまず個人 s などの必要な値を指定せずに新しい連絡先を作成して、姓、名ことはできませんようににそのアプリケーションに検証コードを追加します。

> [!div class="step-by-step"]
> [前へ](iteration-1-create-the-application-cs.md)
> [次へ](iteration-3-add-form-validation-cs.md)
