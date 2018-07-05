---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'パート 6: モデル検証のためのデータ注釈の使用 |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 6 では、V のモデルのデータ注釈の使用について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: ca0ac2ca909f838f1c91e6cc01b8aafa90c0b193
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397656"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>モデル検証のためのパート 6: を使用してデータ注釈
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。  
>   
> MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。  
>   
> このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 6 では、モデル検証のためのデータ注釈の使用について説明します。


Create と Edit のフォームの主要な問題がある: いずれかの検証を行わない。 価格フィールドに、型の数字または必要なフィールドを空白のままにするなどの処理を行ってし、データベースが最初のエラーを説明します。

データ注釈をモデル クラスに追加することでアプリケーションに検証を簡単に追加できます。 データ注釈を使用すると、必要、モデルのプロパティに適用される規則を詳しく説明して、ASP.NET MVC が使用を強制して、ユーザーに適切なメッセージを表示するが取得されます。

## <a name="adding-validation-to-our-album-forms"></a>アルバムのフォーム検証の追加

次のデータ注釈属性を使用します。

- **必要な**– プロパティが必須のフィールドであることを示します。
- **DisplayName** – フォーム フィールドと検証メッセージで使用される対象のテキストを定義します。
- **StringLength** – 文字列フィールドの最大長を定義します。
- **範囲**– 数値フィールドの最大値と最小値は、
- **バインド**– を除外または含めるパラメーターまたはフォームの値をモデルのプロパティにバインドするときにフィールドを一覧表示されます。
- **ScaffoldColumn** – エディターのフォームのフィールドを非表示を許可

*注: データ注釈属性を使用してモデルの検証の詳細については、MSDN ドキュメントを参照します。*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

アルバム クラスを開き、次の追加*を使用して*ステートメントを先頭にします。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

次に、次に示すように、表示と検証の属性を追加するプロパティを更新します。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

調査中にもに変更されましたジャンル、アーティスト仮想プロパティ。 これにより、遅延読み込みに必要に応じて、Entity Framework です。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

アルバム、モデルにこれらの属性を追加すること後、は、フィールドの検証、作成および編集画面がすぐに開始され、表示名を使用して (例: アルバム アートの Url AlbumArtUrl ではなく) を選択しました。 アプリケーションを実行し、/StoreManager/Create を参照します。

![](mvc-music-store-part-6/_static/image1.png)

次に、いくつかの検証ルールを破るします。 0 の価格を入力し、タイトルを空白のままにします。 [作成] ボタンをクリックしたときの検証エラーを示すメッセージ フィールド検証規則を満たしていない定義したと表示されるフォームが表示されます。

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>クライアント側検証のテスト

サーバー側検証は、ユーザーはクライアント側の検証を回避できるため、アプリケーションの観点から非常に重要です。 ただし、web ページ フォームのみサーバー側の検証を実装するには、次の 3 つの重要な問題が発生します。

1. ユーザーは、投稿、サーバーで、検証するフォームと、ブラウザーに送信される応答を待機します。
2. 今すぐ、検証規則を通過するようにフィールドを修正するときに、ユーザーは迅速なフィードバックを取得しません。
3. ユーザーのブラウザーを利用する代わりに検証ロジックを実行するサーバーのリソースを無駄にしています。

さいわい、ASP.NET MVC 3 のスキャフォールディング テンプレートは一切追加の作業を必要としない組み込まれており、クライアント側検証があります。

検証メッセージがすぐに削除するため、検証の要件を満たすタイトル フィールドに 1 つの文字を入力します。

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-5.md)
> [次へ](mvc-music-store-part-7.md)
