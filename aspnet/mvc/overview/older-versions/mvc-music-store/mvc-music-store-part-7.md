---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: '手順 7: メンバーシップと承認 |Microsoft ドキュメント'
author: jongalloway
description: このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 第 7 部では、メンバーシップと承認について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a0f599da4691c5bb7c8e6f01625fc0e94ce0eac8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="part-7-membership-and-authorization"></a>手順 7: メンバーシップと承認
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。  
>   
> MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。  
>   
> このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 第 7 部では、メンバーシップと承認について説明します。


このストア マネージャー コント ローラーは現在、サイトを訪問だれにもアクセス可能なです。 サイト管理者に権限を制限するこれを変更してみましょう。

## <a name="adding-the-accountcontroller-and-views"></a>AccountController とビューの追加

ASP.NET MVC 3 Web アプリケーション テンプレートのすべてと、ASP.NET MVC 3 空の Web アプリケーション テンプレートの 1 つの違いは、空のテンプレートが、アカウント コント ローラーが含まれていないことです。 すべての ASP.NET MVC 3 Web アプリケーション テンプレートから作成された新しい ASP.NET MVC アプリケーションからいくつかのファイルをコピーして、アカウント コント ローラーを追加します。

完全 ASP.NET MVC 3 Web アプリケーション テンプレートを使用して、新しい ASP.NET MVC アプリケーションを作成し、次のファイルをプロジェクトに同じディレクトリにコピーします。

1. コント ローラーのディレクトリにコピー AccountController.cs
2. モデルのディレクトリにコピー AccountModels
3. Views ディレクトリ内のアカウント ディレクトリを作成し、内のすべての 4 つのビューをコピー

コント ローラーとモデルのクラスの名前空間を変更して、名前の先頭 MvcMusicStore ようにします。 AccountController クラスが MvcMusicStore.Controllers 名前空間を使用する必要があり、AccountModels クラスが MvcMusicStore.Models 名前空間を使用する必要があります。

*注: これらのファイルも、チュートリアルの先頭に、サイト設計のファイルをコピーした MvcMusicStore Assets.zip ダウンロードで使用できます。メンバーシップのファイルはコード ディレクトリにあります。*

更新されたソリューションは、次のようになります。

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>ASP.NET 構成サイトを管理ユーザーを追加します。

当社の web サイトの承認を必要としてにアクセス権を持つユーザーを作成する必要になります。 ユーザーを作成する最も簡単な方法では、組み込みの ASP.NET の構成 web サイトを使用します。

次のソリューション エクスプ ローラーで、アイコンをクリックして、ASP.NET の構成 web サイトを起動します。

![](mvc-music-store-part-7/_static/image2.png)

これは、構成の web サイトを起動します。 ホーム画面で、セキュリティ タブをクリックし、画面の中央に「の役割を有効にする」リンクをクリックします。

![](mvc-music-store-part-7/_static/image3.png)

「ロールの作成または管理」のリンクをクリックします。

![](mvc-music-store-part-7/_static/image4.png)

ロール名として"Administrator"を入力し、[役割の追加] ボタンを押します。

![](mvc-music-store-part-7/_static/image5.png)

戻るボタンをクリックし、左側にあるユーザーの作成リンクをクリックします。

![](mvc-music-store-part-7/_static/image6.png)

左側の次の情報を使用してユーザー情報フィールドを入力します。

| **フィールド** | **[値]** |
| --- | --- |
| **ユーザー名** | 管理者 |
| **パスワード** | password123! |
| **パスワードの確認入力** | password123! |
| **電子メール** | (任意の電子メール アドレスは機能) |
| **セキュリティの質問** | (お好きなよう) |
| **セキュリティ返答** | (お好きなよう) |

*メモ: たい任意のパスワードを使用することができますもちろんです。既定のパスワード セキュリティ設定では、7 文字の長さは、1 つの英数字以外の文字を含むパスワードが必要です。*

このユーザーの管理者ロールを選択し、ユーザーの作成 ボタンをクリックします。

![](mvc-music-store-part-7/_static/image7.png)

この時点では、ユーザーが正常に作成されたことを示すメッセージが表示されます。

![](mvc-music-store-part-7/_static/image8.png)

これで、ブラウザー ウィンドウを閉じることができます。

## <a name="role-based-authorization"></a>ロール ベースの承認

これで、ユーザーがクラスのどのコント ローラー アクションへのアクセスを管理者ロールにする必要がありますを指定する [Authorize] 属性を使用して StoreManagerController へのアクセスを制限おことができます。

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*注: コント ローラーのクラス レベルだけでなく、特定のアクション メソッドでは、[Authorize] 属性を配置できます。*

ログオン ダイアログ/StoreManager を参照するようになりましたが表示されます。

![](mvc-music-store-part-7/_static/image9.png)

新しい管理者アカウントでログオンした後にする前として、アルバムの編集画面に移動することができました。

> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-6.md)
> [次へ](mvc-music-store-part-8.md)
