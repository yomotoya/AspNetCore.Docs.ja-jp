---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: "イテレーション 2 – は、検索 nice (c#) アプリケーションを作成する |。Microsoft ドキュメント"
author: microsoft
description: "このイテレーションで、アプリケーションの外観を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: 10379f5321773155aaff4c384d8e0716d7e0e874
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="iteration-2--make-the-application-look-nice-c"></a>イテレーション 2 – は、検索 nice (c#) アプリケーションを作成します。
====================
によって[Microsoft](https://github.com/microsoft)

[コードをダウンロードします。](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> このイテレーションで、アプリケーションの外観を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>連絡先管理 ASP.NET MVC アプリケーション (c#) の構築
  

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

このイテレーションの目標は、連絡先のマネージャー アプリケーションの外観を向上させるためにです。 現時点では、連絡先のマネージャーは、(図 1 を参照してください) の既定の ASP.NET MVC ビュー マスター ページおよびカスケード スタイル シートを使用します。 これらがない表示が不良ですが、他のすべての ASP.NET MVC web サイトと同じように検索する連絡先のマネージャーたくないです。 これらのファイルをカスタムのファイルに置き換える必要があります。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**図 01**: ASP.NET MVC アプリケーションの既定の外観 ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image2.png))


このイテレーションでは、アプリケーションのビジュアル デ ザインを向上させるために 2 つの方法を説明します。 最初を表示する空き ASP.NET MVC デザイン テンプレートをダウンロードする ASP.NET MVC デザイン ギャラリーを活用する方法です。 ASP.NET MVC デザイン ギャラリーでは、作業なしプロフェッショナル向けの web アプリケーションを作成することができます。

連絡先のマネージャー アプリケーションの ASP.NET MVC デザイン ギャラリーから、テンプレートを使用することにしました。 代わりに、プロフェッショナルなデザイン会社によって作成されたカスタム デザインがありました。 このチュートリアルの 2 番目の部分で ASP.NET MVC の最終的な設計を作成するプロフェッショナルなデザイン会社と提携方法を説明します。

## <a name="the-aspnet-mvc-design-gallery"></a>ASP.NET MVC デザイン ギャラリー

ASP.NET MVC デザイン ギャラリーは、Microsoft によって提供される無料リソースです。 ASP.NET MVC ギャラリーは、次のアドレスです。

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

ASP.NET MVC デザイン ギャラリーは、ASP.NET MVC プロジェクトで使用するためには、具体的に作成された無料の web サイトの設計のコレクションをホストします。 設計は、コミュニティのメンバーでアップロードされます。 ギャラリーへの訪問者が投票できる、お気に入りの設計 (図 2 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**図 02**: ASP.NET MVC デザイン ギャラリー ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image4.png))


このチュートリアルを書いてギャラリーで最も一般的なデザイン、年 10 月を付けた David Hauser 設計です。 ASP.NET MVC プロジェクトのこの設計を使用するには、次の手順を完了します。

1. クリックして、**ダウンロード**October.zip ファイルをコンピューターにダウンロードするボタンをクリックします。
2. ダウンロードした October.zip ファイルを右クリックし、をクリックして、**ブロックを解除する**(図 3 を参照してください) ボタンをクリックします。
3. 年 10 月をという名前のフォルダーにファイルを解凍します。
4. 年 10 月フォルダーに含まれているレイアウト フォルダーからすべてのファイルを選択し、ファイルを右クリックし、メニュー オプションを選択**コピー**です。
5. Visual Studio ソリューション エクスプ ローラー ウィンドウで ContactManager プロジェクト ノードを右クリックし、メニュー オプションを選択**貼り付け**(図 4 を参照してください)。
6. Visual Studio のメニュー オプションを選択**編集、検索し、置換 クイック置換**と置換*[MyProjectName]*で*ContactManager* (図 5 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**図 03**: web からダウンロードしたファイルのブロックを解除 ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image6.png))


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**図 04**: ソリューション エクスプ ローラーでファイルを上書きする ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image8.png))


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**図 05**: [プロジェクト名] に置き換えて ContactManager ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image10.png))


次の手順を完了すると、web アプリケーションは、新しいデザインを使用します。 図 6 内のページは、年 10 月の設計と連絡先のマネージャー アプリケーションの外観を示しています。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**図 06**: 年 10 月テンプレートを使用して ContactManager ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>カスタム ASP.NET MVC の設計の作成

ASP.NET MVC デザイン ギャラリーには、さまざまなデザイン スタイルの適切な選択があります。 ギャラリーでは、ASP.NET MVC アプリケーションの外観をカスタマイズする最も簡単に提供します。 また、もちろん、ギャラリーには、完全に解放されるの大きな利点。

ただし、web サイトの完全に独自のデザインを作成する必要があります。 その場合は、意味が web サイト デザイン会社を使用します。 連絡先のマネージャー アプリケーションの設計のためには、この方法を採用することにしました。

私は、イテレーション 1 から連絡先マネージャーを圧縮し、デザイン会社にプロジェクトを送信します。 でした t 問題が発生することが (残念なことにです)、Visual Studio を所有するしなかったされません。 Microsoft Visual Web Developer を無料でダウンロードすることができました、 [https://www.asp.net](https://www.asp.net) web サイトと Visual Web Developer で、連絡先のマネージャー アプリケーションを開きます。 日数のいくつかで図 7 にデザインが生成される必要があります。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**図 07**: ASP.NET MVC の連絡先のマネージャー デザイン ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image14.png))


この新しい設計は、2 つのメイン ファイルで構成されている: 新しいカスケード スタイル シート ファイルと新しいビュー マスター ページ ファイルです。 ビュー マスター ページには、レイアウトと ASP.NET MVC アプリケーションでのビューの共有のコンテンツが含まれています。 たとえば、ビュー マスター ページが含まれています、ヘッダー、ナビゲーション タブおよび表示されるフッター図 7 にします。 \Shared フォルダーに、Site.Master 既存ビュー マスター ページを上書きしたデザイン会社から新しい Site.Master ファイルには

デザイン会社では、新しいカスケード スタイル シートとイメージのセットも作成されます。 これらの新しいファイルをコンテンツ フォルダーに配置して、既存の Site.css ファイルが上書きされました。 すべての静的なコンテンツは、コンテンツのフォルダーに配置する必要があります。

連絡先マネージャー用の新しいデザインには編集、および連絡先を削除するイメージが含まれていることを確認します。 連絡先の HTML テーブルでは、各連絡先の横にある編集および削除イメージが表示されます。

最初に、これらのリンク、HTML でレンダリングされました。次のように ActionLink() ヘルパー。

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

Html.ActionLink() メソッドは、イメージ (HTML、メソッドは、セキュリティ上の理由には、このリンク テキストをエンコード) をサポートしていません。 そのため、次のように Url.Action() への呼び出しに Html.ActionLink() への呼び出しは置き換えられます。

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

Html.ActionLink() メソッドでは、HTML ハイパーリンク全体を表示します。 Url.Action() メソッドがせず URL だけをその一方で、表示、 &lt;、&gt;タグ。

さらに、新しいデザインが選択および選択されていない [両方] タブが含まれることに注意してください。 たとえば、図 8 に、**の連絡先の新規作成**タブが選択されていると、**連絡先** タブが選択されていません。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**図 08**: 選択されているし、タブの選択を解除 ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image16.png))


選択したおよび選択されていない [両方] タブの表示をサポートするには、MenuItemHelper をという名前のカスタム HTML ヘルパーを作成しました。 このヘルパー メソッドを表示するか、 &lt;li&gt;タグまたは&lt;li クラス =「選択されている」&gt;タグは、現在のコント ローラーとアクションがヘルパーに渡されるコント ローラーとアクション名に対応しているかどうかによって異なります。 MenuItemHelper のコードは、1 のリストに含まれます。

**1 - Helpers\MenuItemHelper.cs を一覧表示します。**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

MenuItemHelper クラスを使用して、TagBuilder 内部的に作成する、 &lt;li&gt; HTML タグ。 TagBuilder クラスは、新しい HTML タグを構築する必要がある場合に使用できる非常に便利なユーティリティ クラスです。 属性を追加する、CSS クラスを追加する、Id を生成および s タグを変更するメソッドが含まれていますの内部 HTML。

## <a name="summary"></a>概要

このイテレーションは、ASP.NET MVC アプリケーションのビジュアル デ ザインを向上します。 最初に、ASP.NET MVC デザイン ギャラリーに導入されています。 ASP.NET MVC アプリケーションで使用できる ASP.NET MVC デザイン ギャラリーから無料のデザインのテンプレートをダウンロードする方法を学習しました。

次に、既定のカスケード スタイル シート ファイルおよびマスター ビュー ページのファイルを変更してカスタム デザインを作成する方法について説明します。 新しい設計をサポートするために、連絡先のマネージャー アプリケーションに若干の変更を行いました。 たとえば、および選択されていない [選択] タブを表示する MenuItemHelper をという名前の新しい HTML ヘルパーを追加します。

次のイテレーションで検証の非常に重要なサブジェクトに取り組みます。 アプリケーションに検証コードを追加おユーザーできません最初 s の利用者などの必要な値を指定せず新しい連絡先を作成し、姓、名ようにします。

>[!div class="step-by-step"]
[前へ](iteration-1-create-the-application-cs.md)
[次へ](iteration-3-add-form-validation-cs.md)
