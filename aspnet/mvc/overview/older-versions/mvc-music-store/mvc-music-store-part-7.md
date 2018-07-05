---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'パート 7: メンバーシップと承認 |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 第 7 部では、メンバーシップと承認について説明します。
ms.author: aspnetcontent
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 1f2ad9de3a21366931efe6ca2466f4efc23a6192
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802540"
---
<a name="part-7-membership-and-authorization"></a>パート 7: メンバーシップと承認
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。  
>   
> MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。  
>   
> このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 第 7 部では、メンバーシップと承認について説明します。


ストア マネージャー コント ローラーは、サイトにアクセスしてすべてのユーザーに現在アクセスできません。 サイト管理者に権限を制限するこれを変更してみましょう。

## <a name="adding-the-accountcontroller-and-views"></a>AccountController とビューの追加

完全な ASP.NET MVC 3 Web アプリケーション テンプレートと、ASP.NET MVC 3 空 Web アプリケーション テンプレートの 1 つの違いは、空のテンプレートには、アカウントのコント ローラーは含まれません。 完全な ASP.NET MVC 3 Web アプリケーション テンプレートから作成された新しい ASP.NET MVC アプリケーションからいくつかのファイルをコピーして、アカウント コント ローラーを追加します。

完全な ASP.NET MVC 3 Web アプリケーション テンプレートを使用して、新しい ASP.NET MVC アプリケーションを作成し、次のファイルをプロジェクト内の同じディレクトリにコピーします。

1. コント ローラーのディレクトリにコピー AccountController.cs
2. モデルのディレクトリにコピー AccountModels
3. Views ディレクトリ内のアカウント ディレクトリを作成し、4 つすべてのビューをコピー

MvcMusicStore で開始するために、コント ローラーとモデルのクラスの名前空間を変更します。 AccountController クラスは MvcMusicStore.Controllers 名前空間を使用する必要があり、AccountModels クラス MvcMusicStore.Models 名前空間を使用する必要があります。

*注: これらのファイルは、チュートリアルの先頭に、サイト設計のファイルをコピーした MvcMusicStore Assets.zip ダウンロードで入手できます。メンバーシップのファイルは、Code ディレクトリにあります。*

更新されたソリューションは、次のようになります。

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>ASP.NET の構成サイトを持つ管理ユーザーを追加します。

Web サイトで承認を要求する、前にアクセス権を持つユーザーを作成する必要になります。 ユーザーを作成する最も簡単な方法では、組み込みの ASP.NET の構成 web サイトを使用します。

次のソリューション エクスプ ローラーで、アイコンをクリックして、ASP.NET の構成 web サイトを起動します。

![](mvc-music-store-part-7/_static/image2.png)

これは、構成の web サイトを起動します。 [ホーム画面で、[セキュリティ] タブをクリックし、画面の中央に"ロールを有効にする] リンクをクリックします。

![](mvc-music-store-part-7/_static/image3.png)

「ロールの作成または管理」リンクをクリックします。

![](mvc-music-store-part-7/_static/image4.png)

ロール名として"Administrator"を入力し、[役割の追加] ボタンを押します。

![](mvc-music-store-part-7/_static/image5.png)

戻る ボタンをクリックし、左側にあるユーザーの作成リンクをクリックします。

![](mvc-music-store-part-7/_static/image6.png)

左側の次の情報を使用してユーザー情報フィールドに入力します。

| **フィールド** | **[値]** |
| --- | --- |
| **ユーザー名** | 管理者 |
| **パスワード** | password123! |
| **パスワードの確認入力** | password123! |
| **電子メール** | (任意の電子メール アドレスは機能) |
| **セキュリティの質問** | (好き) |
| **セキュリティ返答** | (好き) |

*注: たい任意のパスワードもちろん使用できます。既定のパスワード セキュリティ設定が必要なパスワードは 7 文字の長さを 1 つの英数字以外の文字が含まれています。*

このユーザーの管理者ロールを選択し、ユーザーの作成 ボタンをクリックします。

![](mvc-music-store-part-7/_static/image7.png)

この時点では、ユーザーが正常に作成されたことを示すメッセージが表示されます。

![](mvc-music-store-part-7/_static/image8.png)

ブラウザー ウィンドウを閉じることができますようになりました。

## <a name="role-based-authorization"></a>ロール ベースの承認

ユーザーが、クラス内のコント ローラー アクションにアクセスする管理者ロールである必要がありますを指定する [Authorize] 属性を使用して StoreManagerController にアクセスを制限できることです。

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*注: 特定のアクション メソッドをおよびコント ローラーのクラス レベルでは、[Authorize] 属性を配置できます。*

ログオン ダイアログ/StoreManager を今すぐ参照が表示されます。

![](mvc-music-store-part-7/_static/image9.png)

後、新しい管理者アカウントでログオンする前に、とアルバムの編集画面に移動することができました。

> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-6.md)
> [次へ](mvc-music-store-part-8.md)
