---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET および Web Tools for Visual Studio 2013 リリース ノート 2013.2 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: fb36d9f469265be60a7e40ed7e317d8da6560bf9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824498"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET and Web Tools 2013.2 Visual Studio 2013 リリース ノート
====================
によって[Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>インストールに関する注記

ASP.NET および Web Tools for Visual Studio 2013.2、メインのインストーラーにまとめられの一部としてダウンロードできます[Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)します。

## <a name="documentation"></a>ドキュメント

チュートリアルおよび Visual Studio 2013.2 の ASP.NET と Web ツールの詳細については、その他の情報は、 [ASP.NET web サイト](https://www.asp.net/)します。

## <a name="software-requirements"></a>ソフトウェア要件

ASP.NET および Web Tools for Visual Studio 2013.2 には、Visual Studio 2013 が必要です。

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>ASP.NET および Web Tools for Visual Studio 2013.2 新機能

次のセクションでは、リリースで導入された機能について説明します。

- [1 つの ASP.NET プロジェクト テンプレート](#oneaspnet)
- [IIS Express で Web アプリケーションを起動するときに、SSL をサポートします。](#ssl)
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
- オンプレミス組織のアカウントを使用して認証をサポートする ASP.NET Web API テンプレートを更新します。
- ASP.NET SPA テンプレートには、MVC、およびサーバー側のビューに基づく認証が含まれています。 テンプレートは、認証されたユーザーのみアクセスできる web Api コント ローラーにします。

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>IIS Express で Web アプリケーションを起動するときに、SSL をサポートします。

参照と、localhost 上で HTTPS をデバッグするときに、セキュリティの警告を取り除くには、自己署名入りの IIS を信頼するには、Internet Explorer と Chrome express SSL 証明書を許可するためのダイアログを追加しました。

たとえば、SSL を使用する web プロジェクトのプロパティを設定できます。 プロパティ ダイアログ ボックスを表示、f4 キーをクリックします。 変更**SSL が有効になっている**を true にします。 SSL URL をコピーします。

![SSL には、プロパティが有効になっています。](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

HTTPS を使用する web プロジェクト プロパティ ページ web タブ セット ベースの URL (SSL url は、 `https://localhost:44300/` SSL Web サイトを以前に作成した場合を除き、します)。

![プロジェクトの URL (HTTPS) を設定します。](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 指示に従って、IIS Express が生成した自己署名証明書を信頼します。

![SSL の警告](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

読み取り、**セキュリティ警告**クリックしてダイアログ**はい**を表す localhost 証明書をインストールする場合。

![セキュリティの警告](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

IE または Chrome で、サイトが表示されます、ブラウザーで証明書の警告なし。

![警告なしに HTTPS ページ](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

警告に表示されるように、Firefox は独自の証明書ストアを使用します。

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web エディターの機能強化

- **新しいプロジェクト項目の JSON およびエディター**: Visual Studio にプロジェクト項目の JSON およびエディターを追加しました。 現在の JSON エディターの機能には、色づけ、構文検証が、中かっこの補完、アウトライン、ツールのオプションの設定などが含まれます。

    ![JSON エディター](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense になりました[JSON スキーマ](http://json-schema.org/)v3 と v4 です。 既存のスキーマを選択して、ローカル スキーマのパスを編集するスキーマのコンボ ボックスがあるまたはドラッグするだけですが、相対パスを取得するためにプロジェクトの JSON ファイルを削除します。

    ![JSON の Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![JSON スキーマ エディター](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **新しい Sass (SCSS) エディター**: VS2013 は、RTM では、以下を追加しましたと Sass プロジェクト項目およびエディターがあるようになりました。 Sass エディターの機能が LESS エディターに相当する、色付け、変数、Mixin IntelliSense、コメント/コメント解除、クイック ヒント、書式設定、構文検証、アウトライン、定義へ移動、カラー ピッカーなどのツール オプションの設定など。

    ![SCSS スタイル シートの 新しい項目を追加します。](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![スタイル シートの編集](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **新しい URL ピッカー HTML、Razor、CSS、LESS と Sass ドキュメント:** Web フォーム ページの外部でない URL ピッカーを使用して、VS 2013 が付属しています。 HTML、Razor、CSS、以下の新しい URL の選択と Sass エディターが認識ダイアログのない、fluent」と入力選択 '… ' img タグ、リンクのフィルター ファイルを適切に一覧表示します。

    ![イメージ タグの URL の選択](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![ビューの URL の選択](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Css URL ピッカー](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **LESS エディターでより多くの機能を追加する更新プログラム**
- **Knockout Intellisense アップグレード**: VS の intelliSense の標準 KnockOut 構文を追加しました"ko-vs エディター viewModel:"構文。 これは、フォームにコメントを使用してページ上の複数のビュー モデルにバインドを使用できます。

    ![Knockout の Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    サポートを追加しました ViewModel IntelliSense が入れ子になったため、ViewModel に深く入れ子になったオブジェクトにドリルダウンする可能性があります。

    `<div data-bind="text: foo.bar.baz.etc" />`

    表示される IntelilSense では、JavaScript オブジェクトの完全な IntelliSense です。

    ![Intellisense が表示された完全な JavaScript オブジェクト](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **新しい URL ピッカー HTML、Razor、CSS、LESS と Sass ドキュメント**: Web フォーム ページの外部でない URL ピッカーを使用して、VS 2013 が付属しています。 HTML、Razor、CSS、以下の新しい URL の選択と Sass エディターが認識ダイアログのない、fluent」と入力選択 '… ' img タグ、リンクのフィルター ファイルを適切に一覧表示します。

    ![イメージ タグの URL の選択](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![ビューの URL の選択](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Css URL ピッカー](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>ブラウザー リンク

- Browser Link は、HTTPS 接続をサポートするようになりましたとはボックスの一覧をダッシュ ボードでその他の接続をブラウザーで証明書が信頼されている限り、します。
- 静的な HTML ソースのマッピング
- SPA は、データのマッピング サポートします。
- マッピング データの自動更新

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Visual Studio での Azure App Service Web アプリのサポート

- **Azure のサポートにサインインします。**
- **リモート デバッグと web apps 用のリモート ビュー**: はサポートされる[Azure App Service で web アプリのリモート デバッグ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)と、サーバー エクスプ ローラーで web アプリのコンテンツ ファイルのリモート ビュー。

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>新しい Web プロジェクトを作成するときに、リモートの Azure リソースを作成します。

Azure を追加しました[「リモート リソースの作成」](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)新しい web アプリケーション ダイアログでチェック ボックスをオンします。 これを選択して、テスト、およびいくつかの簡単な手順で発行プロファイルを作成するための Azure の発行サイトの設定、新しい web アプリケーションの作成のエクスペリエンスを統合することになります。

![Azure リソースを含む新しいプロジェクト](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Azure への発行](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Web の発行の機能強化

- 発行のためのユーザー エクスペリエンスを向上します。

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET のスキャフォールディング

- **列挙型のサポート:** かどうかは、モデルは、列挙型を使用して、MVC スキャフォルダーは列挙型のドロップダウン リストを生成します。 これは、MVC で列挙ヘルパーを使用します。
- **サポートのブートス トラップ**: ブートス トラップ クラスを使用するように MVC スキャフォールディングで EditorFor テンプレートを更新します。
- **サポート パッケージ**: MVC と Web API のスキャフォールディングで MVC と Web API の 5.1 のパッケージを追加

次のスクリーン ショットは、モデルのスキャフォールディングを示しています。

- モデルのコード:

     ![モデルのコード](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- コンパイルし、モデル コードを右クリックし、選択**追加**、**スキャフォールディングされた新しい項目**します。

     ![新しくスキャフォールディングした項目を追加します。](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- 選択**Entity Framework を使用するビューと MVC5 コント ローラー**:

     ![新しい MVC5 コント ローラーとビューを追加します。](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **コント ローラーを追加**モデルを使用します。

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Views/WeekdayModels/Edit.cshtml を含む例については、生成されたコードを確認して`@Html.EnumDropDownListFor`: ![EnumDropDownListFor を格納しているかを表示](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- 生成された列挙型のコンボ ボックスを表示するにいる場合は、値を null にすることができます、空の文字列に選択できるコンボ ボックスに注意してください ページを実行します。 たとえば、**作成**ページには、次が表示されます。

    ![コンボ ボックスが空の文字列を許可します。](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 RTM は、2014 年 4 月にリリースされます。 リリース ノートから注目すべき点しますが、確認してください、[完全なリリース ノート](http://docs.nuget.org/docs/release-notes/nuget-2.8)これらの変更の詳細についてはします。

- **Windows Phone 8.1 アプリケーションのターゲット**: NuGet 2.8.1 は 'WindowsPhoneApp'、'WPA'、'WindowsPhoneApp81' および 'WPA81' のターゲット フレームワーク モニカーを使用して Windows Phone 8.1 アプリケーションを対象とするサポートしているようになりました。
- **依存関係の解決をパッチ**: NuGet がパッケージの依存関係を満たす最下位のメジャーおよびマイナーのパッケージ バージョンを選択した場合の戦略を実装してこれまでパッケージの依存関係を解決するときにします。 メジャーおよびマイナーのバージョンとは異なり、修正プログラムのバージョン常に解決された最上位のバージョンを。 動作は、理由の 1 つが、依存関係を持つパッケージをインストールするための決定性の欠如が作成されます。
- **DependencyVersion スイッチ**: が NuGet 2.8 の変更、*既定*依存関係を解決するための動作も追加の依存関係の解決プロセスで - DependencyVersion スイッチを使用してより細かく制御しますパッケージ マネージャー コンソール。 スイッチは、最下位の可能なバージョン (既定の動作)、最高の可能なバージョンや最上位のマイナーまたは修正プログラムのバージョンに依存関係を解決できます。 このスイッチは、powershell コマンドでインストール パッケージにのみ機能します。
- **DependencyVersion 属性**: 上記の詳細 - DependencyVersion スイッチだけでなく NuGet も - DependencyVersion スイッチする場合、既定値は定義する nuget.config ファイルに新しい属性を設定する機能を許可インストール パッケージの呼び出しで指定されていません。 この値は、任意の操作のインストール パッケージを NuGet パッケージ マネージャー ダイアログでも適用されます。 この値を設定するには、nuget.config ファイルに次の属性を追加します。

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **NuGet の操作を-whatif をプレビュー**: 一部の NuGet パッケージは、厳密な依存関係のグラフを持つことができ、そのため、そのことができます、インストール中に立つ、アンインストール、または更新操作がまずどうなるかを確認します。 NuGet 2.8 は、標準の PowerShell を追加します。 の場合、コマンドの適用先となるパッケージのクロージャ全体をビジュアル化を有効にするパッケージのインストール、アンインストール、パッケージ、および更新プログラム パッケージのコマンドに切り替えます。
- **パッケージをダウン グレード**: は新しい機能を調査するために、パッケージのプレリリース版をインストールし、最後の安定したバージョンにロールバックするには珍しくありません。 NuGet 2.8、前に、プレリリースのパッケージとその依存関係をアンインストールすると、以前のバージョンをインストールし、複数の手順をしました。 NuGet 2.8 でただし、更新プログラム パッケージはようになりました (例: パッケージの依存関係ツリー) 全体のパッケージ クロージャにロールバック以前のバージョン。
- **開発の依存関係**: さまざまな種類の機能を開発プロセスを最適化するために使用されるツールを含む - NuGet パッケージとして配信できます。 その後、新しいパッケージの依存関係が発行された、新しいパッケージの開発に役立つ可能性があるときに、これらのコンポーネントを考慮されませんする必要があります。 NuGet 2.8 では developmentDependency として .nuspec ファイル自体を識別するためにパッケージをできます。 インストールすると、このメタデータは、プロジェクト、パッケージのインストール先の packages.config ファイルにも追加されます。 Packages.config ファイルは後で NuGet の依存関係の nuget.exe パックの中に分析される場合は、開発の依存関係としてマークされているこれらの依存関係を除外します。
- **さまざまなプラットフォーム向けの個々 の packages.config ファイル**: 複数のターゲット プラットフォーム用のアプリケーションを開発する場合は、別のプロジェクト ファイルのそれぞれのビルド環境を持つ共通します。 パッケージがあるさまざまなレベルのさまざまなプラットフォームのサポートのためにも別のプロジェクト ファイルで別の NuGet パッケージを使用する一般的です。 NuGet 2.8 は、さまざまなプラットフォーム固有プロジェクト ファイルの別の packages.config ファイルを作成してこのシナリオのサポートの強化を提供します。
- **ローカル キャッシュにフォールバック**: が NuGet パッケージの通常使用される、リモート ギャラリーからなど、 [NuGet ギャラリー](http://www.nuget.org)ネットワーク接続を使用して、クライアントが接続されていないシナリオはよくあります。 ネットワーク接続を行わず、NuGet クライアントはこれらのパッケージは、ローカル NuGet キャッシュ内で、クライアント コンピューターに存在した場合でもパッケージを正常にインストールできませんでした。 NuGet 2.8 は、パッケージ マネージャー コンソールをキャッシュの自動フォールバックを追加します。

    キャッシュ フォールバック機能では、特定のコマンド引数は必要ありません。 さらに、フォールバックのキャッシュは、現在、パッケージ マネージャー コンソールでのみ機能 - パッケージ マネージャー ダイアログで、動作が現在機能しません。
- **バグの修正**: 更新プログラム パッケージのパフォーマンスの向上が行われた主要なバグ修正のいずれかのコマンドを再インストールします。

    NuGet のこのリリースには、これらの機能および上記のパフォーマンスの修正プログラム、に加えて、他の多くのバグ修正も含まれます。 リリースで対処された 181 合計問題が発生しました。 作業の完全な一覧の項目で修正された NuGet 2.8、くださいビュー、 [NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all)今回のリリース。

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET Web フォーム

- Web フォーム テンプレートには、ASP.NET Identity をアカウントの確認とパスワードのリセットを実行する方法が表示されます。
- エンティティのデータ ソース コントロール、Entity Framework 6 の動的なデータ プロバイダー。 詳細については、次の MSDN ブログをご覧ください:[動的なデータ プロバイダーと Entity Framework 6 の EntityDataSource コントロール](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx)します。

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [属性のルーティングが強化されました](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [エディター テンプレートのブートス トラップのサポート](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [ビューでの列挙型のサポート](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Minlength Unobstrusive サポート/MaxLength 属性](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [控えめな Ajax での 'this' コンテキストのサポート](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- さまざまな[バグの修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [グローバル エラー処理](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [属性のルーティングの機能強化](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [ヘルプ ページの改善](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [IgnoreRoute サポート](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [BSON メディアの種類のフォーマッタ](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [非同期フィルターのサポートの強化](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [クエリのクライアント ライブラリを書式設定の解析](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- さまざまな[バグの修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Web Pages 3.1.2

- さまざまな[バグの修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework をバージョン 6.1 ランタイムとツールの両方に更新されました。 Entity Framework (EF) 6.1 Entity Framework 6 のマイナー アップデートは、さまざまなバグ修正と新機能が含まれています。 新しい機能は、ドキュメントへのリンクを含む、EF6.1 の詳細については、次を参照してください。 [Entity Framework のバージョン履歴](https://msdn.microsoft.com/data/jj574253)します。 このリリースの新機能は次のとおりです。

- **ツールの統合**新しい EF モデルを作成する一貫した方法を提供します。 この機能は、既存のデータベースからリバース エンジニア リングを含む、Code First モデルの作成をサポートするために、ADO.NET Entity Data Model ウィザードを拡張します。 これらの機能では、以前の EF Power Tools Beta 品質で利用できます。
- **トランザクションのコミット エラーの処理**提供新しい[System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx)を使用する新しく導入された機能のトランザクションの操作を傍受します。 **CommitFailureHandler**トランザクションのコミット中に接続の障害からの自動復旧を許可します。
- **IndexAttribute**を Code First モデルのプロパティ (またはプロパティ) の属性を配置することで指定するインデックスを使用します。 コード最初はインデックスを作成して対応するデータベースにします。
- **パブリック マッピング API** EF がデータベースで列とテーブルにプロパティと型をマップする方法に情報へのアクセスを提供します。 以前のリリースこの API が内部です。
- **App/Web.config ファイルを使用してインターセプターを構成する機能**(インターセプターは、アプリケーションを再コンパイルしなくても追加することができます)。
- **DatabaseLogger**をファイルにすべてのデータベース操作を記録するが容易にする新しいインターセプターします。 従来の機能と組み合わせて、これにより簡単に再コンパイルすることがなく、デプロイされたアプリケーションのデータベース操作のログ記録を切り替えることです。
- **移行モデル変更の検出**スキャフォールディングの移行がより正確な; 変更の検出プロセスのパフォーマンスが大幅に強化されてもようにが改善されました。
- **パフォーマンスの向上**の初期化中に、制限のデータベース操作など、LINQ クエリで null の等値比較のための最適化高速化 (モデルの作成) の生成で表示するより多くのシナリオより効率的な複数のアソシエーションを持つ追跡対象エンティティの具体化します。

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **2 要素認証**: ASP.NET Identity では、2 要素認証をサポートします。 2 要素認証では、パスワードが侵害を取得する場合、ユーザー アカウントにセキュリティの追加のレイヤーを提供します。 2 要素コードに対するブルート フォース攻撃に対する保護もあります。
- **アカウント ロックアウト:** ユーザーに入力した場合、パスワードまたは 2 要素コード正しく場合に、ユーザーをロックする方法を提供します。 無効な試行回数と期間の数、ユーザーがロックアウトを構成できます。 開発者は必要に応じてオフにできますアカウント ロックアウトの特定のユーザー アカウントに必要な必要があります。
- **アカウントの確認:** ASP.NET Identity のシステムでは、アカウントの確認をサポートします。 これは、新しいアカウント、web サイトに登録するときに必要がある web サイトの操作を実行する前に、電子メールの確認に現在ほとんどの web サイトで非常に一般的なシナリオです。 確認の電子メールは作成されない偽アカウントを防ぐために役立ちます。 これは、フォーラムのサイトや銀行、e コマース、ソーシャル web サイトなど、web サイトのユーザーとの通信の方法として電子メールを使用している場合に非常に便利です。
- **パスワードのリセット:** パスワードのリセットは、機能を自分のパスワードを忘れてしまった場合に、ユーザーで、パスワードにリセットできます。
- **(すべての場所を記号) のセキュリティ スタンプ:** 関連 (など、Facebook、Google、関連付けられたログインを削除するなどの情報をユーザーが自分のパスワードまたはその他のセキュリティを変更した場合、ユーザーのセキュリティ トークンを再生成する方法をサポートしていますMicrosoft アカウントとなど)。 これは、古いパスワードで生成されたすべてのトークンが無効にすることを確認する必要です。 サンプル プロジェクトで、ユーザーのパスワードを変更する場合ユーザー用に新しいトークンを生成し、以前のトークンが無効になります。 Everywhere (その他のブラウザー) このアプリケーションにログインした場所からログアウトされますが、この機能は、以降にパスワードを変更すると、アプリケーションにセキュリティの追加のレイヤーを提供します。
- **主キーの型をユーザーおよびロールを拡張できるように**: ASP.NET Identity 1.0 では、テーブルのユーザーとロールの主キーの型が文字列。 この場合は、ASP.NET の Id システムは、SQL Server で、Entity Framework を使用して永続化されたときに、nvarchar を使用していますが。 Stack Overflow でこの既定の実装の多くの議論があったと受信のフィードバックに基づいて。 どのようなユーザーおよびロールのテーブルの主キーをする必要がありますを指定できます拡張性フックを提供しています。 この拡張フックがファイルを格納するユーザー Id は Guid または整数の場合は、アプリケーションを移行して、アプリケーションが特に便利です。
- **ユーザーおよびロールの IQueryable をサポートして**: UsersStore と RolesStore IQueryable のサポートを追加、ユーザーおよびロールの一覧に簡単に取得できます。
- **UserManager で Delete 操作をサポート**
- **ユーザー名のインデックス作成**: ASP.NET Identity Entity Framework 実装を追加しました一意のインデックスをユーザー名に新しい IndexAttribute EF 6.1.0 を使用しています。 これにより、ユーザー名が一意では常にし、競合の状態では重複するユーザー名で終わるする可能性がないことを確認します。
- **パスワード検証の強化:** ASP.NET Identity 1.0 に同梱されていたパスワードの検証コントロールが最小の長さの検証のみが非常に基本的なパスワードの検証コントロール。 パスワードの複雑さの詳細に制御を提供する新しいパスワードの検証コントロールがあります。 このパスワード内のすべての設定を有効にした場合でもをお勧めのユーザー アカウントの 2 要素認証を有効にすることに注意してください。
- **IdentityFactory ミドルウェア/CreatePerOwinContext**:

    - **ユーザー マネージャー**: OWIN コンテキストから UserManager のインスタンスを取得するファクトリの実装を使用することができます。 このパターンは、SignIn、SignOut の OWIN コンテキストから AuthenticationManager を取得するため使用と似ています。 これは、アプリケーションの要求ごとには、UserManager のインスタンスを取得することをお勧めします。
    - **DbContextFactory**: ASP.NET Identity が SQL Server の Id システムを保持するために Entity Framework を使用します。 Id システムがこれに、ApplicationDbContext への参照があります。 DbContextFactory ミドルウェアは、1 回の要求、アプリケーションで使用できる ApplicationDbContext のインスタンスを返します。
- **ASP.NET Identity のサンプルの NuGet パッケージ**: サンプル NuGet パッケージをやすく、インストール、ASP.NET Identity のサンプルを実行し、ベスト プラクティスに従ってください。 これは、サンプル ASP.NET MVC アプリケーションです。 これを運用環境で展開する前に、アプリケーションに合わせてコードを変更してください。 空の ASP.NET アプリケーションには、サンプルをインストールする必要があります。 パッケージの詳細については、次のブログ記事を参照してください:[発表 RTM の ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Microsoft OWIN コンポーネント

多数のこのリリースで修正されたバグが発生しました。 参照してください、 [、2.1.0 のリリース ノート リリース](https://katanaproject.codeplex.com/releases/view/113281)詳細な情報。

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

多数のこのリリースで修正されたバグが発生しました。 参照してください、 [、2.0.2 のリリース ノート リリース](https://github.com/SignalR/SignalR/releases/tag/2.0.2)詳細な情報。
