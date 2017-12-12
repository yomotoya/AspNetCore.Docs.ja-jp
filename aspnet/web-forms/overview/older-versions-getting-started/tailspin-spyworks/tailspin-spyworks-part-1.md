---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: "パート 1: ファイルに新しいプロジェクト-> |Microsoft ドキュメント"
author: JoeStagner
description: "このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 1 部では、概要とファイル]/[新しいプロジェクトについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: bd840f9f3f5d723e6bc1bb35955a7770634e9483
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-1-file--new-project"></a>パート 1: ファイルを新しいプロジェクト
====================
によって[行える](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。
> 
> このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 1 部では、概要とファイル]/[新しいプロジェクトについて説明します。


## <a id="_Toc260221666"></a>概要

このチュートリアルは、ASP.NET WebForms の概要です。 お緩やかに変化を開始することを初級レベルの web 開発が発生するように問題ありません。

構築するアプリケーションは、単純なオンライン ストアです。

![](tailspin-spyworks-part-1/_static/image1.jpg)


閲覧者は、カテゴリによって製品を参照できます。

![](tailspin-spyworks-part-1/_static/image2.jpg)

1 つの製品を表示し、カートに追加することができます。

![](tailspin-spyworks-part-1/_static/image3.jpg)

必要がなくなったすべての項目を削除する、カートを確認することができます。

![](tailspin-spyworks-part-1/_static/image4.jpg)

チェック アウトを続行するプロンプトされます。

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

、順序付けした後に、単純な確認画面が参照してください。

![](tailspin-spyworks-part-1/_static/image7.jpg)


まず、Visual Studio 2010 で ASP.NET WebForms の新しいプロジェクトを作成して、増分を完全に機能しているアプリケーションを作成する機能を追加します。 その過程では、ここデータベースへのアクセス、リストやグリッド ビュー、データ更新のページ、データの検証、一貫したページ レイアウト、AJAX、検証、ユーザーのメンバーシップ、および複数のマスター ページを使用します。

順を追っていくことができますをまたはから完成したアプリケーションをダウンロードする[http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Visual Studio 2010 または、空き Visual Web Developer 2010 からのいずれかを使用することができます[https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/)です。 アプリケーションをビルドするには、SQL Server または空き SQL Server Express データベースをホストするのいずれかを使用できます。

## <a id="_Toc260221667"></a>ファイル/新しいプロジェクト

まず、Visual Studio で、[ファイル] メニューから新しいプロジェクトを選択します。 新しいプロジェクト ダイアログが表示されます。

![](tailspin-spyworks-part-1/_static/image8.jpg)

ここを選択すると、Visual c#/Web テンプレートは、左側のグループ化され、中央の列で、「ASP.NET Web アプリケーション」テンプレートを選択します。 TailspinSpyworks、プロジェクトの名前を [ok] ボタンを押します。

![](tailspin-spyworks-part-1/_static/image9.jpg)

これにより、プロジェクトが作成されます。 ソリューション エクスプ ローラーの右側にあるアプリケーションに含まれているフォルダーを見てをみましょう。

![](tailspin-spyworks-part-1/_static/image10.jpg)

空のソリューションが完全に空でない – 基本的なフォルダー構造を追加します。

![](tailspin-spyworks-part-1/_static/image1.png)

ASP.NET 4 の既定のプロジェクト テンプレートによって実装される規則に注意してください。

- "Account"フォルダーは、ASP の基本的なユーザー インターフェイスを実装します。NET のメンバーシップのサブシステムです。
- "Scripts"フォルダーがクライアント側の JavaScript ファイルのリポジトリとして機能し、コア jQuery の .js ファイルは、既定で利用可能な。
- 「スタイル」フォルダーは、web サイトの視覚化 (CSS スタイル シート) を整理するために使用します。

F5 キーを押して、アプリケーションを実行し、default.aspx ページを表示するキーを押すとお次をご覧ください。

![](tailspin-spyworks-part-1/_static/image11.jpg)

WebForms の既定のテンプレートから Style.css ファイルを置き換える、CSS クラスと Tailspin Spyworks アプリケーション用の対象が visual asthetics を表示する関連付けられているイメージ ファイルに、最初のアプリケーションの機能強化がされます。

これを行った後、default.aspx ページは、次のように表示します。

![](tailspin-spyworks-part-1/_static/image12.jpg)

上部にあるイメージへのリンクに注意してください、ページおよびマスター ページに追加されているメニュー項目の右。 「サインイン」と「アカウント」のリンクは、(既定のテンプレートによって生成される) に存在するページとアプリケーションを構築するには実装のページの残りの部分をポイントします。

またしようとして、マスター ページをスタイル ディレクトリに移動します。 これは、優先順位ことをお点少し簡単に場合は、将来"skinable"にするには、アプリケーションにします。

マスター ページを変更する必要がありますこれを行った後は、すべての .aspx ファイル内の参照は、ASP.NET WebForms ページを既定で生成されます。

>[!div class="step-by-step"]
[次へ](tailspin-spyworks-part-2.md)
