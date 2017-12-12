---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: "パート 6: データ注釈を使用してモデルの検証の |Microsoft ドキュメント"
author: jongalloway
description: "このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 第 6 部では、モデル V のデータの注釈の使用方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b2083d5604741a0142f504f92779c9f931d0d6d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-6-using-data-annotations-for-model-validation"></a>モデル検証のためのパート 6: を使用してデータ注釈
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。  
>   
> MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。  
>   
> このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 第 6 部では、モデル検証のためのデータ注釈の使用方法について説明します。


作成および編集フォームの主要な問題があるお: 検証は一切行っていません。 価格 フィールドに必要なフィールドを空白または型の文字のままにするなどの処理を行って、後ほどお見せ最初エラーは、データベースからです。

モデル クラスをデータ注釈を追加することで、アプリケーションに検証簡単に追加できます。 データ注釈を使用するか、モデルのプロパティに適用される規則について説明することと、ASP.NET MVC の使用を強制して、ユーザーに適切なメッセージを表示するように注意はします。

## <a name="adding-validation-to-our-album-forms"></a>アルバム フォームへの検証の追加

次のデータの注釈属性は使用します。

- **必要な**– プロパティが必須フィールドであることを示します
- **DisplayName** – フォーム フィールドと検証メッセージで使用テキストを定義
- **StringLength** – 文字列フィールドの最大の長さを定義します。
- **範囲**– 数値フィールドの最大値と最小値を設定
- **バインド**– を除外したり、モデルのプロパティをパラメーターまたはフォームの値をバインドする場合は、フィールドの一覧
- **ScaffoldColumn** – エディターのフォームのフィールドを非表示が可能になります

*注: データの注釈属性を使用してモデルの検証の詳細については、マニュアルを参照して、MSDN*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

アルバム クラスを開き、次の追加*を使用して*ステートメントを先頭にします。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

次に、次に示すように、表示と検証の属性を追加するプロパティを更新します。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

存在している時にもに加えた変更ジャンル、アーティスト仮想プロパティです。 これにより、遅延読み込みに必要に応じて、Entity Framework。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

アルバム、モデルにこれらの属性を追加すること後の作成および編集画面がすぐにフィールドの検証を開始し、表示名を使用する場合は、この (例: アルバム アートの Url AlbumArtUrl ではなく) 選択したします。 アプリケーションを実行し、/StoreManager/Create を参照します。

![](mvc-music-store-part-6/_static/image1.png)

次に、いくつかの検証規則を破損します。 0 の価格を入力し、タイトルを空白のままにします。 [作成] ボタンをクリックしたときの検証エラー メッセージを表示するフィールドは、検証規則を満たしていませんでした定義したと表示されるフォームが表示されます。

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>クライアント側検証のテスト

サーバー側の検証は、ユーザーがクライアント側の検証を回避するためにアプリケーションの観点からが重要です。 ただし、のみサーバー側の検証を実装している web ページのフォームには、次の 3 つの重要な問題が発生します。

1. ユーザーは、フォーム ポストされた、サーバー上で検証して、ブラウザーに送信される応答を待機しています。
2. 今すぐ、検証規則を通過するようにフィールドを解決するときに、ユーザーは即時のフィードバックを取得しません。
3. ユーザーのブラウザーを利用する代わりに検証ロジックを実行するサーバーのリソースを無駄にしています。

幸いにも、ASP.NET MVC 3 scaffold テンプレートでは、一切の補足作業をする必要はありません組み込まれており、クライアント側検証があります。

検証メッセージがすぐに削除するため、検証の要件を満たす"タイトル"フィールドに 1 つの文字を入力します。

![](mvc-music-store-part-6/_static/image3.png)


>[!div class="step-by-step"]
[前へ](mvc-music-store-part-5.md)
[次へ](mvc-music-store-part-7.md)
