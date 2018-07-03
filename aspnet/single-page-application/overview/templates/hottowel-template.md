---
uid: single-page-application/overview/templates/hottowel-template
title: ホット Towel テンプレート |Microsoft Docs
author: madskristensen
description: HotTowel テンプレート
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: de81f12f57d7f2fb7c6478bfa1f3a278ae905a39
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388549"
---
<a name="hot-towel-template"></a>Hot Towel テンプレート
====================
によって[Mads Kristensen](https://github.com/madskristensen)

> ホットのタオル MVC テンプレートは、John Papa によって書き込まれます。
> 
> ダウンロードするバージョンを選択します。
> 
> [Visual Studio 2012 用のホット タオル MVC テンプレート](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Visual Studio 2013 のホット タオルの MVC テンプレート](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> ホットのタオル: ない SPA に移動するためです。


SPA を構築したいが、開始する場所を決定することはできませんか? ホット タオルを使用し、秒単位では、SPA とその上に構築する必要があるすべてのツールをしましたします。

ホットのタオルでは、ASP.NET を使用したシングル ページ アプリケーション (SPA) を構築するための出発点を作成します。 すぐするモジュール式の構造を提供、コード、ビューのナビゲーション、データ バインディング、豊富なデータ管理および単純ですが、洗練されたスタイルをします。 ホット タオルは、すべてのプラミングではないアプリに集中できるように、SPA を構築する必要があるものを提供します。

> SPA を構築について詳しく学習[John Papa のビデオ、チュートリアル、および Pluralsight コース](http://johnpapa.net/spa?vsix)します。


## <a name="application-structure"></a>アプリケーションの構造

ホットのタオル SPA には、アプリケーションを定義する JavaScript と HTML ファイルを含むアプリ フォルダーが提供します。

アプリ フォルダー。

- durandal
- サービス
- ビューモデル
- ビュー

アプリ フォルダーには、モジュールのコレクションが含まれています。 これらのモジュールは、機能をカプセル化し、他のモジュールの依存関係を宣言します。 Views フォルダーには、アプリケーションの HTML が含まれているし、ビューモデル フォルダーにはビュー (一般的な MVVM パターン) のプレゼンテーション ロジックが含まれています。 サービス フォルダーは、アプリケーションには、HTTP データを取得またはローカル ストレージとの対話など必要なすべての一般的なサービスの格納に最適です。 サービス モジュールからコードを再利用する複数のビューモデル一般的です。

## <a name="aspnet-mvc-server-side-application-structure"></a>ASP.NET MVC のサーバー側アプリケーションの構造

ホットのタオルは親しみやすく、強力な ASP.NET MVC の構造に基づいています。

- アプリ\_開始
- Content
- Controllers
- モデル
- スクリプト
- Views

## <a name="featured-libraries"></a>おすすめのライブラリ

- ASP.NET MVC
- ASP.NET Web API
- ASP.NET Web の最適化 - バンドルと縮小
- [Breeze.js](http://Breezejs.com) -豊富なデータ管理
- [Durandal.js](http://Durandaljs.com) -ナビゲーションとビューの構成
- [Knockout.js](http://Knockoutjs.com) -データのバインド
- [Require.js](http://requirejs.org) -AMD と最適化モジュール
- [Toastr.js](http://jpapa.me/c7toastr) -ポップアップ メッセージ
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) - 堅牢な CSS スタイル

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Visual Studio 2012 のホット タオル SPA テンプレートを使用してインストールします。

Visual Studio 2012 のテンプレートとしてホット タオルをインストールできます。 クリックするだけ`File`  |  `New Project`選択`ASP.NET MVC 4 Web Application`します。 選択し、' ホット タオル Single Page Application"テンプレートと実行。

## <a name="installing-via-the-nuget-package"></a>NuGet パッケージをインストールします。

ホットのタオルが空の既存の ASP.NET MVC プロジェクトを補足する NuGet パッケージもあります。 Nuget を使用してをインストールし、実行します。

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>ホットのタオルの構築方法

単に開始のコードを追加します。

1. 独自のサーバー側のコード (Breeze.js で真価) を Entity Framework では可能であれば、WebAPI の追加します。
2. ビューを追加、`App/views`フォルダー
3. Viewmodel の追加、`App/viewmodels`フォルダー
4. 新しいビューへの HTML と Knockout のデータ バインディングを追加します。
5. ナビゲーションのルートを更新します。 `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>HTML または JavaScript のチュートリアル

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index.cshtml は、開始のルートと MVC アプリケーション用のビューです。 すべての標準のメタ タグ、css のリンク、および期待する JavaScript の参照が含まれています。 本文には、1 つが含まれています。`<div>`これは、すべてのコンテンツ (ビュー) が配置される要求されたとき。 `@Scripts.Render` Require.js を使用して、含まれているアプリケーションのコードの開始ポイントを実行する、`main.js`ファイル。 スプラッシュ スクリーンは、アプリが読み込まれるときに、スプラッシュ スクリーンを作成する方法を示すために提供されます。

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/main.js

`main.js`ファイルには、アプリが読み込まれるとすぐに実行されるコードが含まれています。 これは、ナビゲーションのルートを定義、ビュー、スタートアップ設定、および任意のセットアップ/ブートス トラップなど、アプリケーションのデータを準備します。

`main.js`起動アプリケーションの開始を支援する durandal のモジュールのいくつかのファイルを定義します。 定義するステートメントは、関数の場合、利用できるように、モジュールの依存関係を解決に役立ちます。 まず、デバッグ メッセージが有効なコンソール ウィンドウに、アプリケーションのパフォーマンスがどのようなイベントに関するメッセージを送信します。 App.start のコードでは、durandal framework アプリケーションを起動するように指示します。 Durandal に知っているすべてのビューとビューモデルがそれぞれの名前付きの同じフォルダーに含まれるように、規則が設定されます。 最後に、`app.setRoot`読み込みが起動し、`shell`事前に定義されたを使用して`entrance`アニメーション。

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Views

内のビューにある、`App/views`フォルダー。

### <a name="shellhtml"></a>shell.html

`shell.html` HTML のマスター レイアウトが含まれています。 すべて、その他のビューの合成されるどこかの側で、`shell`ビュー。 ホットのタオルを提供します、`shell`でこのような 3 つのリージョン: ヘッダー、コンテンツ領域、およびフッターです。 これらの各リージョンが読み込まれる内容が要求されたときに、その他のビューを形成します。

`compose`ヘッダーとフッターのバインドが困難をポイントするホット タオルでコーディング、`nav`と`footer`ビューをそれぞれします。 セクションの compose バインド`#content`にバインドされて、`router`モジュールのアクティブな項目です。 つまり、クリックしたときに対応するビューのナビゲーション リンクがこの領域に読み込まれます。

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

`nav.html` SPA のナビゲーション リンクが含まれています。 これは、メニュー構造を配置できる場所、たとえばです。 多くの場合 (Knockout を使用) にバインドされたデータには、`router`モジュールで定義されているナビゲーションを表示する、`shell.js`します。 Knockout はデータ バインド属性し、バインドに、 `shell` viewmodel ナビゲーション ルートを表示して (Twitter Bootstrap を使用して) プログレス バーを表示する場合、`router`モジュールが (参照別に 1 つのビューから移動中です`router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>home.html と details.html

これらのビューには、カスタム ビューの HTML が含まれます。 ときに、`home`のリンクを`nav`ビューのメニューをクリックすると、`home`ビューのコンテンツ エリアに格納されます、`shell`ビュー。 これらのビューは拡張、または独自のカスタム ビューに置き換えられます。

### <a name="footerhtml"></a>footer.html

`footer.html`の下部に、フッターに表示される HTML が含まれています、`shell`ビュー。

## <a name="viewmodels"></a>ViewModels

Viewmodel がある、`App/viewmodels`フォルダー。

### <a name="shelljs"></a>shell.js

`shell` Viewmodel にはプロパティおよびにバインドされている関数が含まれています、`shell`ビュー。 メニュー ナビゲーションのバインドがある多くの場合は (を参照してください、`router.mapNav`ロジック)。

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>home.js と details.js

これらの viewmodel にバインドされている関数とプロパティを含む、`home`ビュー。 ビューのプレゼンテーション ロジックが含まれていて、データとビューの間の結び付きは、します。

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Services

サービスは、アプリ/サービス フォルダーで表示されます。 理想的には、取得、およびリモート データを送信するため、dataservice モジュールなど、将来のサービスを配置する可能性があります。

### <a name="loggerjs"></a>logger.js

ホットのタオルの提供、`logger`サービス フォルダー内のモジュール。 `logger`モジュールは、ログ メッセージをコンソールとトーストをポップアップで、ユーザーに最適です。
