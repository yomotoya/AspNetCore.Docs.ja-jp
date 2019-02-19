---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'SQL Server compact の Visual Studio を使用して ASP.NET Web アプリケーションの配置: 12 の第 1 の概要 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9dacafaacdab12b8005cb6073647ae526cefcfb4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826544"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>SQL Server compact の Visual Studio を使用して ASP.NET Web アプリケーションの配置: 12 の第 1 の概要
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。 Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Azure App Service Web Apps にデプロイする方法を示していますチュートリアルでは、次を参照してください。 [ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。
> 
> これらのチュートリアルでは、テスト、ローカル開発用コンピューター上の iis とサード パーティのホスティング プロバイダーにし、最初に配置する方法を説明します。 デプロイするアプリケーションでは、アプリケーション データベースと、ASP.NET メンバーシップ データベースを使用します。 SQL Server Compact を使用して、SQL Server Compact への展開を開始して、以降のチュートリアルを表示するデータベースの変更をデプロイする方法と SQL Server に移行する方法。
> 
> チュートリアルでは、Visual Studio で ASP.NET を使用する方法を理解すると仮定します。 ない場合は、開始点としてを[基本的な ASP.NET Web フォームのチュートリアル](../tailspin-spyworks/tailspin-spyworks-part-1.md)または[基本的な ASP.NET MVC チュートリアル](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)します。
> 
> チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET 展開に関するフォーラム](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)します。


## <a name="overview"></a>概要

これらのチュートリアルでは、テスト、ローカル開発用コンピューター上の iis とサード パーティのホスティング プロバイダーにし、最初に配置する方法を説明します。 デプロイするアプリケーションでは、アプリケーション データベースと、ASP.NET メンバーシップ データベースを使用します。 SQL Server Compact を使用して、SQL Server Compact への展開を開始して、以降のチュートリアルを表示するデータベースの変更をデプロイする方法と SQL Server に移行する方法。

数のチュートリアル-すべて 11 とトラブルシューティングのページ – ことは一見大変な展開プロセス。 実際には、サイトの展開の基本的な手順は、比較的小さな、チュートリアル セットの一部を構成します。 ただし、実際の状況では、多くの場合、情報が必要ないくつかの小さいながらも重要な余分な側面に関する展開の — たとえば、対象サーバーでフォルダーのアクセス許可を設定します。 これらの他のテクニックの多くのチュートリアルは、実際のアプリケーションを正常に展開を妨げる可能性のある情報を忘れないでください期待して、チュートリアルで追加しました。

チュートリアルは順番に実行するように設計し、前のパートに各部分をビルドします。 ただし、状況に関連性のない部分をスキップできます。 (部分をスキップする必要があります以降のチュートリアルで手順を調整します。)

## <a name="intended-audience"></a>対象読者

小規模な組織やその他の環境で作業する ASP.NET 開発者が目的としたチュートリアルでは、場所。

- 継続的インテグレーション プロセス (自動ビルドと配置) は使用されません。
- 運用環境は、サード パーティのホスティング プロバイダーです。
- 通常、1 人のユーザーは、複数のロール (同一人物を開発、テスト、および展開) を設定します。

エンタープライズ環境で継続的インテグレーションのプロセスを実装する方が一般的ですし、運用環境が、通常、会社のサーバーによってホストされています。 通常、さまざまな人々 は、異なる役割を実行します。 エンタープライズ展開方法については、次を参照してください。[エンタープライズ シナリオで Web アプリケーションの配置](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)します。

あらゆる規模の組織を Azure に web アプリケーションをデプロイもでき、このチュートリアルで示す手順のほとんどは、Azure App Services の Web Apps にも適用します。 Azure の概要については、次を参照してください。 [ https://azure.microsoft.com](https://azure.microsoft.com)します。

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>チュートリアルで示すように、ホスティング プロバイダー

チュートリアルでは、ホスティング プロバイダーを持つアカウント設定とそのホスティング プロバイダーにアプリケーションを展開するプロセスを実行します。 特定のホスティング企業は、チュートリアルでは、ライブ web サイトへのデプロイの包括的なエクスペリエンスを示す可能性がありますように選択されました。 各ホスティング プロバイダーのさまざまな機能を提供して、サーバーへのデプロイの経験が少し; を異なるただし、このチュートリアルで説明されているプロセスは、全体的なプロセスの一般的なものです。

このチュートリアルでは、Cytanium.com で使用するホスティング プロバイダーは、使用可能な次のいずれかとを承認または推奨、このチュートリアルでは、その使用は構成されません。

## <a name="deploying-web-site-projects"></a>Web サイト プロジェクトの配置

Contoso 大学は、Visual Studio web アプリケーション プロジェクトです。 ほとんどの展開方法とツールがこのチュートリアルで示すには適用されません[Web サイト プロジェクト](https://msdn.microsoft.com/library/dd547590.aspx)します。 Web サイト プロジェクトをデプロイする方法については、次を参照してください。 [ASP.NET 配置コンテンツ マップ](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects)します。

## <a name="deploying-aspnet-mvc-projects"></a>ASP.NET MVC プロジェクトの配置

このチュートリアルでは、ASP.NET Web フォーム プロジェクトを展開するも ASP.NET MVC に適用可能なすべてを行う方法について説明します。 Visual Studio MVC プロジェクトでは、web アプリケーション プロジェクトのもう 1 つの形式です。 唯一の違いは、ASP.NET MVC またはこれのターゲット バージョンをサポートしていないホスティング プロバイダーにデプロイする場合おく必要があります、適切なインストールしておきます ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0)または[MVC 4](http://nuget.org/packages/aspnetmvc)) プロジェクトに NuGet パッケージ。

## <a name="programming-language"></a>プログラミング言語

C# サンプル アプリケーションを使用が、チュートリアルには、C# の知識が必要としないと、チュートリアルで示すように、展開方法は、言語固有ではないです。

## <a name="troubleshooting-during-this-tutorial"></a>このチュートリアルの中のトラブルシューティング

展開中にエラーが発生したとき、またはデプロイされたサイトが正しく実行されない場合は、エラー メッセージは、ソリューションを常に用意されていません。 問題の一般的なシナリオで役立つ、[トラブルシューティングのリファレンス ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)は使用できます。 エラー メッセージを取得する、または、チュートリアルを進めるときに機能しない、必ず、トラブルシューティングのページを確認してください。

## <a name="comments-welcome"></a>コメントの開始

チュートリアルでは、コメントは、[ようこそ]、およびあらゆる努力をアカウントの修正または提案のコメントのチュートリアルに用意されている改善点を考慮できるように、チュートリアルが更新されたときにします。

## <a name="prerequisites"></a>必須コンポーネント

開始する前に、Windows 7 以降があり、そのコンピューターにインストールされている製品は次のいずれかを確認します。

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC または Visual Studio Express の 2012 RC for Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Visual Studio 2010 SP1 または Visual Web Developer Express 2010 SP1 がある場合も、次の製品をインストールします。

- [Azure SDK for .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (Web 発行の更新を含む)
- [SQL Server 用 Microsoft Visual Studio 2010 SP1 Tools Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

その他のソフトウェアが、チュートリアルを完了するために必要ですがまだ読み込まれている必要はありません。 このチュートリアルでは必要なときにインストールするための手順説明をします。

## <a name="downloading-the-sample-application"></a>サンプル アプリケーションのダウンロード

展開するアプリケーションでは、Contoso University の名前しが既に作成済み。 これは疎で説明されている Contoso University アプリケーションに基づいて、大学の web サイトの簡略化されたバージョン、 [ASP.NET サイト上の Entity Framework チュートリアル](https://asp.net/entity-framework/tutorials)。

前提条件のインストールがある場合は、ダウンロード、 [Contoso University web アプリケーション](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)します。 *.Zip*ファイルには、複数のバージョン プロジェクトと 12 のすべてのチュートリアルを含む PDF ファイルにはが含まれています。 チュートリアルの手順を使用するには、ContosoUniversity Begin を起動します。 チュートリアルの最後に、プロジェクトがどのようにして ContosoUniversity エンドを開きます。 10 のチュートリアルでは、完全な SQL Server への移行の前に、プロジェクトがどのようにを表示するには、ContosoUniversity AfterTutorial09 を開きます。

チュートリアルの手順を実行する準備をするには、ContosoUniversity Begin を Visual Studio プロジェクトを操作するために使用する任意のフォルダーに保存します。 既定の設定では、次のフォルダーです。

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(このチュートリアルのスクリーン ショットでは、プロジェクト フォルダーに配置された、ルート ディレクトリで、 `C`: ドライブ)。

Visual Studio 起動プロジェクトを開きそれを実行する場合は ctrl キーを押し f5 キーを押します。

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Web サイトのページでは、メニュー バーからアクセスできるし、次の機能を実行することができます。

- 学生の統計情報 (バージョン情報 ページ) を表示します。
- 表示、編集、削除、および学生を追加します。
- 表示し、コースを編集します。
- 表示し、インストラクターを編集します。
- 表示および部門を編集します。

いくつかの代表的なページのスクリーン ショットを次に示します。

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>展開に影響するアプリケーション機能の確認

アプリケーションの次の機能は、展開する方法や、それをデプロイするのでは行う必要があるに影響します。 これらの各操作は、以下のチュートリアル シリーズでさらに詳しく説明します。

- Contoso University では、SQL Server Compact データベースを使用して、student および instructor 名などのアプリケーション データを格納します。 さまざまなテスト データ、および運用環境のデータを格納しているし、実稼働環境に展開するときにテスト データを除外する必要があります。 チュートリアル シリーズの後半で SQL Server への SQL Server Compact から移行します。
- アプリケーションでは、SQL Server Compact データベース内のユーザー アカウント情報を格納する、ASP.NET メンバーシップ システムを使用します。 アプリケーションでは、制限された情報へのアクセスを持つ管理者ユーザーを定義します。 テスト アカウントを持たないが、1 人の管理者アカウントを使用して、メンバーシップ データベースをデプロイする必要があります。
- アプリケーション データベースと、メンバーシップ データベースを使用して、SQL Server Compact データベース エンジンとして、ためには、データベース自体に加えて、ホスティング プロバイダーにデータベース エンジンを展開する必要があります。
- アプリケーションは、メンバーシップ システムは、SQL Server Compact データベースにデータを格納できるように、ASP.NET ユニバーサル メンバーシップ プロバイダーを使用します。 ユニバーサル メンバーシップ プロバイダーを格納するアセンブリは、アプリケーションと共に配置する必要があります。
- アプリケーションでは、Entity Framework 5.0 を使用して、アプリケーション データベースのデータにアクセスします。 Entity Framework 5.0 を含むアセンブリは、アプリケーションと共に配置する必要があります。
- アプリケーションでは、サード パーティ製のエラー ログとレポート ユーティリティを使用します。 このユーティリティは、アプリケーションと共に配置する必要がありますアセンブリで提供されます。
- エラーのログ記録ユーティリティでは、ファイル フォルダーに XML ファイルにエラー情報を書き込みます。 ある、配置サイトで ASP.NET を実行するアカウントは、このフォルダーに書き込みアクセス許可を持つこと、およびデプロイからこのフォルダーを除外する必要があることを確認します。 (それ以外の場合、テスト環境からのエラー ログのデータを運用環境にデプロイする場合がありますや、運用環境のエラー ログ ファイルを削除する可能性があります)。
- アプリケーションには、いくつかの設定の変更が必要で、デプロイされている*Web.config*先の環境 (テストまたは運用)、およびその他の設定によっては、ビルドの変更が必要に応じてファイル(デバッグまたはリリース) を構成します。
- Visual Studio ソリューションには、クラス ライブラリ プロジェクトが含まれています。 このプロジェクトで生成されるアセンブリのみを配置する必要があります、プロジェクト自体ではありません。

シリーズの最初のチュートリアルでは、サンプルの Visual Studio プロジェクトをダウンロードし、アプリケーションをデプロイする方法に影響するサイトの機能を確認しました。 以下のチュートリアルでの展開を準備を自動的に処理するような設定です。 その他の手動で注意します。

> [!div class="step-by-step"]
> [次へ](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
