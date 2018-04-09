---
uid: single-page-application/overview/templates/hottowel-template
title: ホット タオル テンプレート |Microsoft ドキュメント
author: madskristensen
description: HotTowel テンプレート
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: dbd037c2469d326a3d3248ca07492ed9eb93e225
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="hot-towel-template"></a>ホット タオル テンプレート
====================
によって[Mads Kristensen](https://github.com/madskristensen)

> ホット タオル MVC テンプレートは、John Papa によって書き込まれます。
> 
> ダウンロードするには、どのバージョンを選択します。
> 
> [Visual Studio 2012 用のホット タオル MVC テンプレート](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Visual Studio 2013 のホット タオル MVC テンプレート](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> ホット タオル: 1 つもない SPA に移動したくないためです。


SPA を構築したいが、開始する場所を決定することはできません。 ホット タオルを使用し、秒単位で、SPA および上でビルドする必要があります。 すべてのツールを必要があります。

ホット タオルでは、ASP.NET を使用した単一ページ アプリケーション (SPA) を構築するための優れた開始ポイントを作成します。 すぐするモジュール式の構造を提供コード、ビューのナビゲーション、データ バインディング、豊富なデータ管理および単純ですが、洗練されたスタイルです。 ホット タオルでは、組み込みではないアプリに集中できるように、SPA をビルドする必要がありますすべてを提供します。

> SPA を構築について学習[John Papa のビデオ、チュートリアル、Pluralsight コース](http://johnpapa.net/spa?vsix)です。


## <a name="application-structure"></a>アプリケーションの構造

アプリケーションを定義する JavaScript と HTML ファイルを含むアプリ フォルダーが提供されるホット タオル SPA です。

アプリのフォルダー。

- Durandal
- サービス
- viewmodels
- ビュー

App フォルダーには、モジュールのコレクションが含まれています。 これらのモジュールは、機能をカプセル化し、その他のモジュールの依存関係を宣言します。 Views フォルダーには、アプリケーションの HTML が含まれているし、viewmodels フォルダーには、ビュー (共通 MVVM パターン) のプレゼンテーション ロジックが含まれています。 サービスのフォルダーは、HTTP データの取得やローカル ストレージの相互作用など、アプリケーションを必要とする共通のサービスのハウジングに最適です。 サービス モジュールからコードを再利用する複数の viewmodels 一般的なです。

## <a name="aspnet-mvc-server-side-application-structure"></a>ASP.NET MVC のサーバー側アプリケーションの構造

ホット タオルは、使い慣れたと強力な ASP.NET MVC 構造に基づいています。

- アプリ\_開始
- Content
- Controllers
- モデル
- スクリプト
- ビュー

## <a name="featured-libraries"></a>機能を備えたライブラリ

- ASP.NET MVC
- ASP.NET Web API
- ASP.NET Web Optimization - バンドルと縮小
- [Breeze.js](http://Breezejs.com)の豊富なデータ管理
- [Durandal.js](http://Durandaljs.com) -ナビゲーションとビューの構成
- [Knockout.js](http://Knockoutjs.com)のデータ バインディング
- [Require.js](http://requirejs.org) -AMD と最適化モジュール
- [Toastr.js](http://jpapa.me/c7toastr) -ポップアップ メッセージ
- [Twitter のブートス トラップ](http://twitter.github.com/bootstrap/)- 堅牢な CSS スタイル

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Visual Studio 2012 ホット タオル SPA テンプレートを使用してインストールします。

ホット タオルは、Visual Studio 2012 のテンプレートとしてインストールできます。 クリックするだけ`File`  |  `New Project`選択`ASP.NET MVC 4 Web Application`です。 選択し、' ホット タオル単一ページ アプリケーション"テンプレートと実行。

## <a name="installing-via-the-nuget-package"></a>NuGet パッケージを使用してインストールします。

ホット タオルが空の既存の ASP.NET MVC プロジェクトを補足する NuGet パッケージもあります。 Nuget を使用してをインストールし、実行します。

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>ホット タオルで構築する方法は?

コードを追加するだけで開始します。

1. 独自のサーバー側のコード (を本当に Breeze.js で際立つ) 可能であれば Entity Framework と WebAPI を追加します。
2. ビューを追加、`App/views`フォルダー
3. Viewmodels を追加、`App/viewmodels`フォルダー
4. 新しいビューへの HTML および Knockout のデータ バインディングを追加します。
5. 内のナビゲーションのルートを更新します。 `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>HTML または JavaScript のチュートリアル

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index.cshtml は、開始のルートと MVC アプリケーション用のビューです。 すべての標準のメタ タグ、css リンク、および期待する JavaScript 参照が含まれています。 本文には、1 つが含まれています。`<div>`これは、すべてのコンテンツ (ビュー) を配置する場所が要求したときにします。 `@Scripts.Render` Require.js を使用して、含まれているアプリケーションのコードの開始ポイントを実行する、`main.js`ファイル。 スプラッシュ スクリーンは、アプリが読み込まれるときに、スプラッシュ スクリーンを作成する方法を説明するものです。

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/main.js

`main.js`ファイルには、アプリが読み込まれるとすぐに実行されるコードが含まれています。 これは、ナビゲーション ルートを定義、ビューを設定および任意セットアップ/ブートス トラップ アプリケーションのデータの準備などを実行する場所です。

`main.js`ファイルには、起動アプリケーションの開始を支援する durandal のモジュールのいくつかを定義します。 定義するステートメントは、関数の利用できるように、モジュールの依存関係の解決に役立ちます。 まず、デバッグ メッセージが有効なコンソール ウィンドウに、アプリケーションの実行、どのようなイベントに関するメッセージを送信します。 App.start のコードでは、durandal framework アプリケーションを起動するように指示します。 規則は、durandal がすべてのビューを認識し、viewmodels がそれぞれの名前付きの同じフォルダーに含まれるように設定されています。 最後に、`app.setRoot`読み込みが開始されると、`shell`事前に定義を使用して`entrance`アニメーション。

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>ビュー

内のビューにある、`App/views`フォルダーです。

### <a name="shellhtml"></a>shell.html

`shell.html` HTML のマスターのレイアウトが含まれています。 側で任意の場所すべて、他のビューの構成は、`shell`ビュー。 ホット タオルの提供、`shell`このような 3 つの領域を持つ: ヘッダー、コンテンツ領域およびフッターです。 これらの地域のそれぞれが読み込まれる内容が要求されたときに、他のビューを形成します。

`compose`ヘッダーとフッターのバインドがハードコードされたホット タオルを指すように、`nav`と`footer`ビューをそれぞれします。 作成するバインディング セクションの`#content`にバインドされて、`router`モジュールのアクティブな項目です。 つまりをクリックするは対応するビューのナビゲーション リンクは、この領域に読み込まれます。

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

`nav.html` SPA のナビゲーション リンクが含まれています。 これは、メニュー構造を配置できるなどです。 多くの場合、これは、データ (Knockout を使用して) バインドを`router`モジュールで定義されているナビゲーションを表示する、`shell.js`です。 Knockout 検索データ バインド属性およびバインドに、 `shell` viewmodel ナビゲーション ルートを表示して、(Twitter のブートス トラップを使用して) プログレス バーを表示する場合、`router`別 (を参照する 1 つのビューからの移動の途中では、モジュール`router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>home.html と details.html

これらのビューには、カスタム ビューの HTML が含まれます。 ときに、`home`内のリンク、`nav`ビューのメニューをクリックすると、`home`コンテンツの領域で、ビューが配置される、`shell`ビュー。 これらのビュー補強したり、独自のカスタム ビューに置き換えられます。

### <a name="footerhtml"></a>footer.html

`footer.html`の下部にある、フッターに表示される HTML が含まれています、`shell`ビュー。

## <a name="viewmodels"></a>ViewModels

ViewModels に含まれて、`App/viewmodels`フォルダーです。

### <a name="shelljs"></a>shell.js

`shell` Viewmodel プロパティおよびにバインドされている関数を含む、`shell`ビュー。 メニュー ナビゲーションのバインディングがある多くの場合は (を参照してください、`router.mapNav`ロジック)。

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>home.js と details.js

これら viewmodels プロパティおよびにバインドされている関数を含む、`home`ビュー。 また、ビューのプレゼンテーション ロジックを含んでいて、グルー データとビューの間は。

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>サービス

アプリケーション/サービス フォルダーには、サービスが見つかりませんでした。 理想的には、dataservice モジュールを取得して、リモート データの送信を担当するなど、将来のサービスを配置する可能性があります。

### <a name="loggerjs"></a>logger.js

ホット タオルの提供、 `logger` services フォルダー内のモジュール。 `logger`モジュールは、のログ メッセージをコンソール、およびトースト ポップアップでユーザーに最適です。
