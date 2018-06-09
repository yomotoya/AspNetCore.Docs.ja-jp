---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET および Web Tools for Visual Studio 2013 のリリース ノート 2013.2 |Microsoft ドキュメント
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2014
ms.topic: article
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 0e7ad52662f7ceaa1f087d007d0b14b610f90bee
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036025"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET および Web ツール 2013.2 for Visual Studio 2013 のリリース ノート
====================
によって[Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>インストールに関する注意事項

ASP.NET および Web Tools for Visual Studio 2013.2 がメインのインストーラーにバンドルしの一部としてダウンロードできます[Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)です。

## <a name="documentation"></a>ドキュメント

チュートリアルおよび Visual Studio 2013.2 の ASP.NET および Web ツールに関するその他の情報は、 [ASP.NET web サイト](https://www.asp.net/)です。

## <a name="software-requirements"></a>ソフトウェア要件

ASP.NET および Web Tools for Visual Studio 2013.2 には、Visual Studio 2013 が必要です。

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>ASP.NET および Web Tools for Visual Studio 2013.2 の新機能

次のセクションでは、リリースで導入された機能について説明します。

- [1 つの ASP.NET プロジェクト テンプレート](#oneaspnet)
- [IIS Express での Web アプリケーションを起動するときに、SSL をサポートします。](#ssl)
- [Visual Studio Web エディターの機能強化](#vswebeditor)
- [ブラウザー リンク](#browserlink)
- [Visual Studio での Azure App Service Web アプリのサポート](#waws)
- [新しい Web プロジェクトを作成するときに、リモートの Azure リソースを作成します。](#AzureResources)
- [Web の発行の機能強化](#webpublish)
- [ASP.NET のスキャフォールディング](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.NET Web フォーム](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [ASP.NET Web Pages 3.1.2](#webpages)
- [Entity Framework 6.1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Microsoft OWIN コンポーネント](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>1 つの ASP.NET プロジェクト テンプレート

- アカウントの確認とパスワードのリセットをサポートする ASP.NET プロジェクト テンプレートを更新します。
- 内部設置型の組織アカウントを使用して認証をサポートする ASP.NET Web API テンプレートを更新します。
- これで、>asp.net SPA テンプレートには、MVC およびサーバー側のビューに基づく認証が含まれます。 テンプレートは、認証済みユーザーによってのみアクセスできる WebAPI コント ローラーがします。

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>IIS Express での Web アプリケーションを起動するときに、SSL をサポートします。

参照と、localhost で HTTPS のデバッグ時にセキュリティの警告を回避するのには、自己署名入りの IIS を信頼するには、Internet Explorer と Chrome express の SSL 証明書を許可するためのダイアログを追加しました。

たとえば、SSL を使用する web プロジェクトのプロパティを設定できます。 プロパティ ダイアログ ボックスを表示 f4 キーをクリックします。 変更**SSL が有効になっている**true に設定します。 SSL URL をコピーします。

![SSL には、プロパティが有効になっています。](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Https プロトコルを使用する web プロジェクト プロパティ ページ web タブ セット ベースの URL (SSL URL がある`https://localhost:44300/`SSL Web サイトを以前に作成した場合を除き、します)。

![プロジェクトの URL (HTTPS) を設定します。](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 IIS Express が生成した自己署名証明書を信頼する手順に従います。

![SSL の警告](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

読み取り、**セキュリティ警告**クリックしてダイアログ**はい**を表す localhost 証明書をインストールする場合。

![セキュリティの警告](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

IE または Chrome に表示される、サイト、ブラウザーで証明書の警告なし。

![HTTPS ページ警告なし](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

警告に表示されるように、Firefox は独自の証明書ストアを使用します。

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web エディターの機能強化

- **新しい JSON プロジェクト項目およびエディター**: Visual Studio に JSON プロジェクト項目およびエディターを追加しました。 現在の JSON エディター機能には、色づけ、構文の検証、中かっこの補完、アウトライン、ツールのオプションの設定などが含まれます。

    ![JSON エディター](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense サポート[JSON スキーマ](http://json-schema.org/)v3 と v4 です。 既存のスキーマを選択して、ローカルのスキーマのパスを編集、スキーマのコンボ ボックスはまたはドラッグするだけが相対パスを取得するためにプロジェクトの JSON ファイルを削除します。

    ![JSON の Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![JSON スキーマ エディター](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **新しい Sass (SCSS) エディター**: RTM では VS2013、以下を追加しましたと Sass プロジェクト項目およびエディターがあるようになりました。 Sass エディター、LESS エディターに相当する機能と機能は、色付け、変数および Mixins IntelliSense、コメント/コメントを解除、クイック ヒント、書式設定、構文の検証、アウトライン表示、定義へ移動、カラー ピッカー、ツールのオプションの設定などです。

    ![SCSS スタイル シートの 新しい項目を追加します。](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![スタイル シート エディター](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **HTML、Razor、CSS で、以下の新しい URL ピッカーと Sass ドキュメント:** Web フォーム ページの外部でない URL ピッカーを使用して VS 2013 が付属しています。 HTML、Razor、CSS、以下の新しい URL ピッカーと Sass エディターが認識するためのダイアログを必要としない、fluent 入力ピッカー '.. ' およびの img タグとリンクのフィルター ファイルが適切に一覧表示します。

    ![イメージ タグの URL ピッカー](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![ビューの URL ピッカー](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![CSS の URL ピッカー](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **多くの機能を追加することによって、LESS エディターへの更新**
- **Knockout Intellisense アップグレード**: VS intelliSense などの非標準の KnockOut 構文が追加されました"ko vs エディター viewModel:"構文です。 これは、フォームでのコメントを使用し、ページ上の複数のビュー モデルにバインドを使用できます。

    ![Knockout Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    深く入れ子にされた、ViewModel オブジェクトにドリル ダウンする可能性がありますので、入れ子になった ViewModel IntelliSense のサポートも追加されました。

    `<div data-bind="text: foo.bar.baz.etc" />`

    表示される IntelilSense は、JavaScript オブジェクトの完全 IntelliSense です。

    ![Intellisense を示す完全な JavaScript オブジェクト](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **HTML、Razor、CSS で、以下の新しい URL ピッカーと Sass ドキュメント**: Web フォーム ページの外部でない URL ピッカーを使用して VS 2013 が付属しています。 HTML、Razor、CSS、以下の新しい URL ピッカーと Sass エディターが認識するためのダイアログを必要としない、fluent 入力ピッカー '.. ' およびの img タグとリンクのフィルター ファイルが適切に一覧表示します。

    ![イメージ タグの URL ピッカー](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![ビューの URL ピッカー](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![CSS の URL ピッカー](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>ブラウザー リンク

- Browser Link ようになりました HTTPS 接続をサポートしてはリストをダッシュ ボードで他の接続に証明書がブラウザーによって信頼されている限り、します。
- 静的な HTML ソースのマッピング
- SPA をサポートしてデータのマッピング
- 自動更新のマッピング データ

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Visual Studio での Azure App Service Web アプリのサポート

- **Azure のサポートでサインインします。**
- **リモート デバッグと web アプリ用のリモート ビュー**: サポート[Azure App service web アプリのリモート デバッグ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)と web アプリのコンテンツ ファイル サーバー エクスプ ローラーでのリモートのビューです。

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>新しい Web プロジェクトを作成するときに、リモートの Azure リソースを作成します。

Azure が追加されました[「リモート リソースの作成」](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)新しい web アプリケーションのダイアログでチェック ボックスをオンします。 これを選択して、テスト、およびいくつかの簡単な手順で発行プロファイルを作成するための Azure 発行サイトを設定する、新しい web アプリケーションを作成する場合の動作を統合することができます。

![Azure リソースを新しいプロジェクト](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Azure への発行](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Web の発行の機能強化

- 発行用のユーザー エクスペリエンスを向上します。

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET のスキャフォールディング

- **列挙型のサポート:** かどうかは、モデルは、列挙型を使用して、MVC Scaffolder は列挙型のドロップダウン リストを生成します。 これは、MVC で列挙ヘルパーを使用します。
- **サポートのブートス トラップ**: ブートス トラップのクラスを使用するように、MVC のスキャフォールディングで EditorFor テンプレートを更新します。
- **サポート パッケージ**: MVC および Web API スキャフォールダーは MVC と Web API 5.1 パッケージを追加

次のスクリーン ショットでは、スキャフォールディング モデルを示しています。

- モデルのコード:

     ![モデルのコード](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- コンパイル、モデルのコードを右クリックし、**追加**、**スキャフォールディングされた新しい項目**です。

     ![新しいスキャフォールディング項目を追加します。](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- 選択**Entity Framework を使用して、ビューがある MVC5 コント ローラー**:

     ![ビューで新しい MVC5 のコント ローラーを追加します。](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **コント ローラーを追加**モデルを使用します。

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Views/WeekdayModels/Edit.cshtml を含む例については、生成されたコードを確認して`@Html.EnumDropDownListFor`: ![EnumDropDownListFor を含むを表示](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- 生成された enum combobox を参照してください、確認する場合は、値は null には、空の文字列を選択できますコンボ ボックスのページを実行します。 たとえば、**作成**ページ、次に示します。

    ![コンボ ボックスが空の文字列を許可します。](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 RTM は、2014 年 4 月にリリースします。 ここでは、リリース ノートから注目すべき点が、確認してください、[完全リリース ノート](http://docs.nuget.org/docs/release-notes/nuget-2.8)これらの変更についての詳細。

- **Windows Phone 8.1 アプリケーションのターゲット**: NuGet 2.8.1 を 'WindowsPhoneApp'、'WPA'、'WindowsPhoneApp81' および 'WPA81' のターゲット フレームワーク モニカーを使用して Windows Phone 8.1 アプリケーションを対象とするサポートしているようになりました。
- **依存関係の解決のパッチ**: NuGet はパッケージへの依存関係を満たすメジャーおよびマイナーのパッケージの最も低いバージョンを選択した場合の戦略を実装してきましたパッケージの依存関係を解決するときにします。 メジャーおよびマイナーのバージョンとは異なりただし、修正プログラムのバージョンが常に解決最高のバージョン。 動作が善意、依存関係を持つパッケージをインストールするための決定性の欠如が作成されます。
- **DependencyVersion スイッチ**: も NuGet 2.8 の変更、*既定*の依存関係を解決するための動作も追加で - DependencyVersion スイッチ経由で依存関係の解決プロセスを細かく制御しますパッケージ マネージャー コンソールです。 スイッチは、最下位の可能なバージョン (既定の動作)、最大の可能なバージョンまたは最高のマイナーまたは修正プログラムのバージョンに依存関係を解決できます。 このスイッチは、powershell コマンドでのインストール パッケージに対してのみ機能します。
- **DependencyVersion 属性**: 上述 - DependencyVersion スイッチ、に加えて NuGet も - DependencyVersion 切り替えた場合、既定値は定義する nuget.config ファイルに新しい属性を設定する機能を許可インストール パッケージの呼び出しで指定されていません。 この値が、任意の操作のインストール パッケージの NuGet Package Manager ダイアログ ボックスでも適用されます。 この値を設定するには、nuget.config ファイルに以下の属性を追加します。

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **NuGet の操作と-whatif をプレビュー**: 一部の NuGet パッケージの詳細な依存関係グラフを持つことができ、そのため、そのことができますのインストール中に役に立ちます、アンインストール、または更新操作が最初に何が起こるかを確認します。 NuGet 2.8 を追加、標準の PowerShell の場合、コマンドの適用先となるパッケージの全体のクロージャをビジュアル化を有効にする、パッケージのインストール、アンインストール、パッケージ、および更新プログラム パッケージのコマンドに切り替えます。
- **パッケージをダウン グレード**: はの新機能を調査するために、パッケージのプレリリース版をインストールし、最後の安定したバージョンにロールバックするには少なくありません。 NuGet 2.8 の前に、これはプレリリースのパッケージとその依存関係をアンインストールし、以前のバージョンをインストールのプロセスを複数のステップをでした。 NuGet 2.8 を使用ただし、更新プログラム パッケージは今すぐパッケージ全体のクロージャ (例: パッケージの依存関係ツリー) にロールバック前のバージョン。
- **開発の依存関係**: さまざまな種類の機能は、NuGet パッケージの開発プロセスを最適化するために使用されるツールを含むとして配信することができます。 新しいパッケージの開発に役立つ可能性があるときに、これらのコンポーネントは見なされません以降である場合に、新しいパッケージの依存関係が公開。 NuGet 2.8 は、自身を識別、developmentDependency .nuspec ファイルにパッケージを使用できます。 インストールすると、このメタデータは、プロジェクト、パッケージのインストール先の packages.config ファイルには追加も。 Packages.config ファイルは後で分析される NuGet の依存関係の中 nuget.exe パックには、開発の依存関係としてマークされているこれらの依存関係を除外します。
- **さまざまなプラットフォーム用の個別の packages.config ファイル**: 複数のターゲット プラットフォーム用のアプリケーションを開発する場合は、ごとに、別のプロジェクト ファイルのそれぞれのビルド環境の共通します。 パッケージがさまざまなプラットフォームのサポートのさまざまなレベルにも、別のプロジェクト ファイル内の別の NuGet パッケージを使用する一般的なです。 NuGet 2.8 は、さまざまなプラットフォーム固有のプロジェクト ファイルを別の packages.config ファイルを作成することで、このシナリオのサポートの向上を提供します。
- **ローカル キャッシュにフォールバック**: が NuGet パッケージの通常使用されるリモート ギャラリーからなど、 [NuGet ギャラリー](http://www.nuget.org)ネットワーク接続を使用するシナリオは多くのクライアントが接続されていません。 ネットワーク接続を行わず NuGet クライアントは、それらのパッケージは NuGet のローカル キャッシュ内のクライアント コンピューターに存在した場合でも、パッケージ - を正常にインストールできませんでした。 NuGet 2.8 は、キャッシュの自動フォールバックをパッケージ マネージャー コンソールに追加します。

    キャッシュのフォールバック機能には、特定のコマンド引数は不要です。 さらに、フォールバック キャッシュが現在パッケージ マネージャー コンソールでのみ機能 - パッケージ マネージャー ダイアログで、動作が現在機能しません。
- **バグの修正**: 更新プログラム パッケージのパフォーマンスの向上が加えられた主なバグの修正のいずれかのコマンドを再インストールします。

    NuGet の今回のリリースには、これらの機能と、上記のパフォーマンス修正に加えて、他の多くのバグ修正も含まれます。 リリースで対処 181 の合計の問題が発生しました。 作業の完全な一覧の項目で修正 NuGet 2.8 くださいビュー、 [NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all)今回のリリースです。

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET Web フォーム

- Web フォーム テンプレートは、ASP.NET Identity のアカウントの確認とパスワードのリセットを行う方法を示します。
- エンティティ データ ソース コントロールは、Entity Framework 6 の動的なデータ プロバイダー。 詳細については、次の MSDN ブログをご覧ください:[動的なデータ プロバイダーと Entity Framework 6 の EntityDataSource コントロール](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx)です。

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [属性のルーティングの機能強化](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [エディター テンプレートのブートス トラップのサポート](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [ビューで列挙型のサポート](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [MinLength の Unobstrusive サポート/MaxLength 属性](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [控えめな Ajax での 'this' コンテキストのサポート](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- さまざまな[バグの修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [グローバルのエラー処理](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [属性のルーティングの機能強化](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [ヘルプ ページ機能強化](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [IgnoreRoute サポート](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [BSON メディア タイプ フォーマッタ](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [非同期のフィルターのサポートの向上](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [クエリのライブラリを書式設定、クライアントの解析](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- さまざまな[バグの修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Web Pages 3.1.2

- さまざまな[バグの修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework ランタイムとツールの両方のバージョン 6.1 に更新されました。 Entity Framework (EF) 6.1 Entity Framework 6 のマイナー アップデートは、多数バグの修正や新機能にはが含まれています。 EF6.1、新規の機能に関するドキュメントへのリンクなどの詳細については、次を参照してください。 [Entity Framework のバージョン履歴](https://msdn.microsoft.com/data/jj574253)です。 このリリースの新機能は次のとおりです。

- **ツールの統合**新しい EF モデルを作成する一貫した方法を提供します。 この機能は、既存のデータベースをリバース エンジニア リングを含む、Code First モデルの作成をサポートするために、ADO.NET Entity Data Model ウィザードを拡張します。 これらの機能が以前 EF パワー ツールの品質をベータ版で使用できます。
- **トランザクション コミットの失敗処理**提供新しい[System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx)を使用する新しく導入された機能のトランザクションの処理を中断します。 **CommitFailureHandler**トランザクションのコミット中に、接続の障害からの自動復旧を許可します。
- **IndexAttribute** Code First モデルのプロパティ (またはプロパティ) の属性を配置することで指定するインデックスを使用します。 コード最初はインデックスを作成して、対応するデータベースにします。
- **パブリック マッピング API** EF はプロパティと型を列と、データベース内のテーブルにマップする方法には、情報へのアクセスを提供します。 以前のリリースこの API は内部です。
- **App/Web.config ファイルを使用してインターセプターを構成する機能**(アプリケーションを再コンパイルせずに追加するインターセプターを許可) します。
- **DatabaseLogger**は新しいインターセプターをファイルにすべてのデータベース操作を記録するが容易にします。 従来の機能と組み合わせて、これによりに再コンパイルする必要はありません、配置されたアプリケーションのデータベース操作のログ記録を簡単に切り替えることです。
- **移行のモデル変更の検出**スキャフォールディング マイグレーションより正確な; 変更の検出プロセスのパフォーマンスが大幅に強化されてもできるように強化されました。
- **パフォーマンスの向上**の初期化中に、制限のデータベース操作など、LINQ クエリで null の等値比較のための最適化も高速生成 (モデルの作成) より多くのシナリオでと表示より効率的な複数のアソシエーションで追跡対象のエンティティを具体化します。

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **2 要素認証**: ASP.NET Identity は、今すぐ 2 要素認証をサポートしています。 2 要素認証は、パスワードが侵害された場合、ユーザー アカウントにセキュリティの強化を提供します。 2 要素コードに対するブルート フォース攻撃に対する保護もあります。
- **アカウントのロックアウト:** 場合は、ユーザーが入力のパスワードまたは 2 要素コード正しく、ユーザーをロックアウトする方法を提供します。 無効な試行回数と期間の数、ユーザーがロックアウトを構成できます。 開発者必要に応じてオフにできますアカウント ロックアウトの特定のユーザー アカウントに必要な必要があります。
- **アカウントの確認:** ASP.NET Identity システムは今すぐアカウントの確認をサポートします。 これは、非常に一般的なシナリオ現在ほとんどの web サイト、web サイトで新しいアカウントに登録するときに必要がある web サイトの操作を実行する前に、電子メールを確認します。 確認の電子メールは、その bogus により、アカウントが作成されるので便利です。 これは、フォーラム サイト、銀行、e コマース、ソーシャル web サイトなど、web サイトのユーザーとの通信方法として電子メールを使用している場合に非常に便利です。
- **パスワードのリセット:** パスワードのリセットは、ユーザー パスワードをリセットするパスワードを忘れた場合、機能します。
- **セキュリティ スタンプ (サインアウト everywhere):** 関連 (など、Facebook、Google、関連付けられたログインを削除するなどの情報をパスワードやその他のセキュリティを変更したときにセキュリティ トークンの場合、ユーザーの再生成する方法をサポートしていますMicrosoft アカウントなど)。 古いパスワードを使用して生成されたすべてのトークンが無効にしたことを確認する必要です。 サンプル プロジェクトで、ユーザーのパスワードを変更する場合ユーザー用に新しいトークンを生成し、前のすべてのトークンが無効になります。 Everywhere (その他のブラウザー) このアプリケーションにログインした場所からログアウトするが、この機能は、以降、パスワードを変更すると、アプリケーションにセキュリティの強化を提供します。
- **主キーの型のユーザーおよびロール拡張できるように**: ASP.NET Identity 1.0 では、テーブルのユーザーおよびロールの主キーの型が文字列。 この場合、ASP.NET Identity システムが Entity Framework を使用して、SQL Server で永続化される場合を使用していた nvarchar です。 この既定の実装でのスタック オーバーフローに関する多くのディスカッションがあったと受信のフィードバックに基づいて。 指定すること、テーブルの主キー、ユーザーおよびロール拡張フックが用意されています。 この拡張フックは、ファイルを格納するユーザー Id は Guid または整数場合は、アプリケーションを移行して、アプリケーションが特に便利です。
- **ユーザーおよびロールの IQueryable をサポートして**: UsersStore と RolesStore IQueryable のサポートを追加、ユーザーおよびロールの一覧に簡単に取得できます。
- **UserManager を介して削除操作をサポートします。**
- **ユーザー名のインデックス作成**: ASP.NET Identity Entity Framework 実装を追加しました、一意のインデックス、ユーザー名に EF 6.1.0 では IndexAttribute に新しいを使用しています。 こうことを確認してユーザー名が一意では常に順番を終了して、重複するユーザー名と競合状態はありませんでした。
- **強化されたパスワードの検証:** ASP.NET Identity 1.0 に同梱されていたパスワードの検証コントロールが最小の長さの検証のみが非常に基本的なパスワードの検証コントロール。 パスワードの複雑さより詳細に制御を提供する新しいパスワード検証コントロールがあります。 で、このパスワードのすべての設定を有効にする場合でもはいただくためユーザー アカウントの 2 要素認証を有効に注意してください。
- **IdentityFactory ミドルウェア/CreatePerOwinContext**:

    - **ユーザー マネージャー**: OWIN コンテキストから UserManager のインスタンスを取得するファクトリの実装を使用することができます。 このパターンは、SignIn、SignOut の OWIN コンテキストからの AuthenticationManager を取得するため使用と似ています。 これは、アプリケーションの要求ごと UserManager のインスタンスを取得することをお勧めします。
    - **DbContextFactory**: ASP.NET Identity Entity Framework を使用して SQL Server で Id システムを保持します。 これを行う Id システム、ApplicationDbContext を参照しています。 DbContextFactory ミドルウェアは、アプリケーションで使用できる要求ごとの ApplicationDbContext のインスタンスを返します。
- **ASP.NET Identity のサンプルの NuGet パッケージ**: サンプル NuGet パッケージがしやすくインストール、ASP.NET Identity のサンプルを実行し、ベスト プラクティスに従ってください。 これは、ASP.NET MVC アプリケーションのサンプルです。 アプリケーションに合わせて、実稼働環境でこれを展開する前にコードを変更してください。 空の ASP.NET アプリケーションでは、サンプルをインストールしてください。 パッケージの詳細については、次のブログの投稿を参照してください: [Announcing RTM の ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Microsoft OWIN コンポーネント

多数のこのリリースで修正されたバグが発生しました。 参照してください、[のリリース ノート、2.1.0 リリース](https://katanaproject.codeplex.com/releases/view/113281)詳細についてはします。

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

多数のこのリリースで修正されたバグが発生しました。 参照してください、[のリリース ノート、2.0.2 リリース](https://github.com/SignalR/SignalR/releases/tag/2.0.2)詳細についてはします。
