---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'パート 1: ファイルに新しいプロジェクト-> |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 1 部では、概要とファイル]/[新しいプロジェクトについて説明します。
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 1c8ba561745c9c02b5513e326b4739ec97338393
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825688"
---
<a name="part-1-file--new-project"></a>パート 1: -> 新しいプロジェクトのファイル
====================
によって[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。
> 
> このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 1 部では、概要とファイル]/[新しいプロジェクトについて説明します。


## <a id="_Toc260221666"></a>  概要

このチュートリアルでは、ASP.NET WebForms の概要についてです。 私たちはゆっくりと開始されます、初心者レベルの web 開発のエクスペリエンスは問題ありません。

ビルドするアプリケーションは、シンプルなオンライン ストアです。

![](tailspin-spyworks-part-1/_static/image1.jpg)


訪問者には、カテゴリ別の製品を参照できます。

![](tailspin-spyworks-part-1/_static/image2.jpg)

1 つの製品を表示およびカートに追加できます。

![](tailspin-spyworks-part-1/_static/image3.jpg)

必要がなくなったすべての項目を削除する、カートを確認することができます。

![](tailspin-spyworks-part-1/_static/image4.jpg)

チェック アウトに進むには、するように求められます

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

、順序付けの後に、単純な確認画面が参照してください。

![](tailspin-spyworks-part-1/_static/image7.jpg)


まず、Visual Studio 2010 では、新しい ASP.NET WebForms プロジェクトを作成して、段階的に機能している完全なアプリケーションを作成する機能を追加します。 その過程について説明しますデータベースへのアクセス、リストやグリッド ビュー、データ更新のページ、データの検証、マスター ページを使用して、一貫性のあるページ レイアウト、AJAX、検証、ユーザーのメンバーシップ。

ステップ バイ ステップに従うことができます。 か、完成したアプリケーションをダウンロードすることができます。 [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Visual Studio 2010 または、無料 Visual Web Developer 2010 からのいずれかを使用する[ https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/)します。 アプリケーションをビルドするには、SQL Server または free SQL Server Express データベースをホストするのいずれかを使用できます。

## <a id="_Toc260221667"></a>  ファイル/新しいプロジェクト

まず、Visual Studio で [ファイル] メニューから新しいプロジェクトを選択します。 新しいプロジェクト ダイアログが表示されます。

![](tailspin-spyworks-part-1/_static/image8.jpg)

選択、Visual c#]、[Web テンプレートは、左側のグループし、中央の列で、「ASP.NET Web アプリケーション」テンプレートを選択します。 TailspinSpyworks をプロジェクトに名前を [ok] ボタンを押します。

![](tailspin-spyworks-part-1/_static/image9.jpg)

これにより、プロジェクトが作成されます。 右側にある ソリューション エクスプ ローラーで、アプリケーションに含まれているフォルダーを見ていきましょう。

![](tailspin-spyworks-part-1/_static/image10.jpg)

空のソリューションが完全に空でない – 基本的なフォルダー構造を追加します。

![](tailspin-spyworks-part-1/_static/image1.png)

ASP.NET 4 の既定のプロジェクト テンプレートによって実装される規則に注意してください。

- 「アカウント」フォルダーは、ASP の基本的なユーザー インターフェイスを実装します。NET のメンバーシップのサブシステムです。
- "Scripts"フォルダーがクライアント側の JavaScript ファイルのリポジトリとして機能し、コア jQuery の .js ファイルは、既定で利用可能な。
- [スタイル] フォルダーは、web サイトのビジュアル (CSS スタイル シート) を整理するために使用します。

F5 キーを押して、アプリケーションを実行し、default.aspx ページをレンダリングするキーを押すときに、次のわかります。

![](tailspin-spyworks-part-1/_static/image11.jpg)

この最初のアプリケーションの拡張機能は、CSS クラスと関連付けられているイメージ ファイル、Tailspin Spyworks アプリケーションに追加する visual asthetics をレンダリングする WebForms の既定のテンプレートから Style.css ファイルを置換するになります。

これを行った後、default.aspx ページは、次のように表示します。

![](tailspin-spyworks-part-1/_static/image12.jpg)

上部にあるイメージ リンクに注意してください、ページとマスター ページに追加されたメニュー項目の右。 「サインイン」と「アカウント」のリンクは、ページは、(既定のテンプレートによって生成された) に存在して、アプリケーションの構築を実装しますが、ページの残りの部分をポイントします。

マスター ページをスタイル ディレクトリに配置する場合にもなります。 これは、優先順位ことをおモ ノを少し簡単にアプリケーションを今後で"skinable"になった場合。

マスター ページを変更する必要がありますこれを行った後は、すべての .aspx ファイル内の参照は、ASP.NET WebForms ページを既定で生成されます。

> [!div class="step-by-step"]
> [次へ](tailspin-spyworks-part-2.md)
