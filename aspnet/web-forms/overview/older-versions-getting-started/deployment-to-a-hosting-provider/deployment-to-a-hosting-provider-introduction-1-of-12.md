---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'SQL Server compact の Visual Studio を使用して ASP.NET Web アプリケーションの配置: 導入 - 1/12 |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 3f1572bb890ee136cdd746040a5efae2ce537116
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>SQL Server compact の Visual Studio を使用して ASP.NET Web アプリケーションの配置: 12 の第 1 の概要
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。 Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。
> 
> チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。
> 
> これらのチュートリアルでは、テストは、ローカル開発用コンピューター上の IIS してから、サード パーティのホスティング プロバイダーに、最初に配置する方法を説明します。 展開するアプリケーションでは、アプリケーション データベースと、ASP.NET メンバーシップ データベースを使用します。 SQL Server Compact を使用して、SQL Server Compact への展開を開始して、後のチュートリアルを表示するデータベースの変更を展開する方法と SQL Server に移行する方法です。
> 
> チュートリアルでは、Visual Studio での ASP.NET の使用方法を知っていると仮定します。 開始するに適してがない場合は、[基本的な ASP.NET Web フォーム チュートリアル](../tailspin-spyworks/tailspin-spyworks-part-1.md)または[基本的な ASP.NET MVC のチュートリアル](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)です。
> 
> チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET 展開フォーラム](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)です。


## <a name="overview"></a>概要

これらのチュートリアルでは、テストは、ローカル開発用コンピューター上の IIS してから、サード パーティのホスティング プロバイダーに、最初に配置する方法を説明します。 展開するアプリケーションでは、アプリケーション データベースと、ASP.NET メンバーシップ データベースを使用します。 SQL Server Compact を使用して、SQL Server Compact への展開を開始して、後のチュートリアルを表示するデータベースの変更を展開する方法と SQL Server に移行する方法です。

数チュートリアル – のすべてのページで 11 とトラブルシューティングのページ – 可能性がありますように圧倒されて、展開プロセスです。 実際には、サイトを展開するための基本的な手順は、比較的小さいチュートリアルのセットの一部を構成します。 ただし、実際の状況では、多くの場合、情報が必要ないくつかの小さなが重要な余分な側面に関する展開の — たとえば、ターゲット サーバーでフォルダーのアクセス許可を設定します。 これらの他の手法の多くは、チュートリアルでは、チュートリアルの実際のアプリケーションを正常に展開を妨げる可能性のある情報を離れることを期待してに掲載されています。

チュートリアルが順番に実行するように設計し、各部分が、前のパートを上に構築します。 ただし、自分の状況に関連性のない部分をスキップすることができます。 (部分をスキップする必要があります後のチュートリアルの手順を調整します。)

## <a name="intended-audience"></a>想定読者

チュートリアルでは、小規模な組織やその他の環境で作業する ASP.NET 開発者が目的で。

- 継続的インテグレーション プロセス (自動ビルドと配置) は使用されません。
- 運用環境は、サード パーティのホスティング プロバイダーです。
- 1 人のユーザーは、通常、複数の役割 (同一人物を開発、テスト、および展開) を設定します。

エンタープライズ環境で継続的インテグレーション プロセスを実装する一般的なと運用環境は通常、会社のサーバーでホストします。 また通常、別のユーザーは、異なる役割を実行します。 エンタープライズ展開については、次を参照してください。[エンタープライズ シナリオで Web アプリケーションの配置](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)です。

あらゆる規模の組織では、azure の web アプリケーションを展開できますもと、このチュートリアルで示す手順のほとんどは、Azure の App Services の Web アプリにも適用されます。 Azure の概要については、次を参照してください。 [ https://azure.microsoft.com](https://azure.microsoft.com)です。

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>チュートリアルで示すように、ホスティング プロバイダー

チュートリアルでは、ホスティング プロバイダーを持つアカウントを設定して、そのホスティング プロバイダーへのアプリケーションの配置のプロセスを実行します。 チュートリアルでは、でしたライブ web サイトを展開する完全なエクスペリエンスを示すように、特定のホスティング企業が選択されました。 各ホスティング企業が、さまざまな機能を提供され、そのサーバーに展開する場合の動作が異なる多少です。ただし、このチュートリアルで説明するプロセスは通常、全体的なプロセスです。

Cytanium.com、このチュートリアルに使用されるホスティング プロバイダーは、使用可能な次のいずれかと、このチュートリアルで使用するにはなりません承認または推奨します。

## <a name="deploying-web-site-projects"></a>Web サイト プロジェクトの配置

Contoso 大学は、Visual Studio web アプリケーション プロジェクトです。 このチュートリアルで示されているツールと展開方法のほとんどには適用されません[Web サイト プロジェクト](https://msdn.microsoft.com/library/dd547590.aspx)です。 Web サイト プロジェクトを展開する方法については、次を参照してください。 [ASP.NET 展開のコンテンツ マップ](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects)です。

## <a name="deploying-aspnet-mvc-projects"></a>ASP.NET MVC プロジェクトの配置

このチュートリアルでは、ASP.NET Web フォーム プロジェクトを展開するを行う方法を説明するすべてのものはも ASP.NET MVC には適用します。 Visual Studio の MVC プロジェクトは、web アプリケーション プロジェクトの単なる別の形式です。 唯一の違いは、ことを ASP.NET MVC またはこれのターゲット バージョンをサポートしていないホスティング プロバイダーに配置する場合は、する必要があります、適切なインストールしてください ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0)または[MVC 4](http://nuget.org/packages/aspnetmvc))、プロジェクトの NuGet パッケージです。

## <a name="programming-language"></a>プログラミング言語

C# サンプル アプリケーションを使用が、チュートリアルには、C# の場合の知識は不要し、チュートリアルで示すように展開方法は、言語固有ではないです。

## <a name="troubleshooting-during-this-tutorial"></a>このチュートリアルでのトラブルシューティング

展開時に、エラーの発生時、または展開されたサイトが正しく実行されない場合は、エラー メッセージは常にソリューションを提供しません。 一般的な問題のシナリオで役立つ、[リファレンス ページのトラブルシューティング](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)は使用できます。 エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、トラブルシューティングのページを確認することを確認します。

## <a name="comments-welcome"></a>コメントへようこそ

チュートリアルでは、上のコメントはへようこそ とチュートリアルが更新されたときにすべての作業量になりますチュートリアルのコメントに用意された機能強化に関するアカウントの修正または提案を考慮します。

## <a name="prerequisites"></a>必須コンポーネント

開始する前に、Windows 7 またはそれ以降があり、そのコンピューターにインストールされている製品は次のいずれかを確認します。

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Visual Studio 2010 SP1 または Visual Web Developer Express 2010 SP1 があれば、また、次の製品をインストールします。

- [Azure SDK for .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (Web 公開の更新プログラムが含まれています)
- [Microsoft Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

他のソフトウェアが、チュートリアルを完了するために必要な必要はありませんがまだ読み込まれています。 このチュートリアルでは必要なときにインストールするための手順を説明します。

## <a name="downloading-the-sample-application"></a>サンプル アプリケーションのダウンロード

展開するアプリケーションは Contoso 大学の名前はおよびを既に作成されています。 疎で説明されている Contoso 大学アプリケーションに基づいて、大学の web サイトの簡素化されたバージョンは、 [Entity Framework、ASP.NET サイトのチュートリアル](https://asp.net/entity-framework/tutorials)です。

インストールの前提条件がある場合は、ダウンロード、 [Contoso 大学 web アプリケーション](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)です。 *.Zip*ファイルには、複数のバージョン プロジェクトと PDF ファイルを含むすべての 12 チュートリアルにはが含まれています。 チュートリアルの手順を使用するには、ContosoUniversity Begin で起動します。 チュートリアルの最後に、プロジェクトがどのようにについては、「ContosoUniversity 終了を開きます。 10 のチュートリアルでは、完全な SQL Server への移行前に、プロジェクトがどのようにを表示するには、ContosoUniversity AfterTutorial09 を開きます。

チュートリアルの手順を実行する準備をするには、Visual Studio プロジェクトを操作するために使用する任意のフォルダーに ContosoUniversity Begin を保存します。 既定では、次のフォルダーを示します。

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(このチュートリアルのスクリーン ショットのプロジェクト フォルダーがルート ディレクトリ内にある、 `C`: ドライブです)。

Visual Studio を起動、プロジェクトを開くおよび CTRL-f5 キーを押して実行します。

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Web サイトのページは、メニュー バーからアクセスできる、次の関数を実行できます。

- 学生の統計情報 (バージョン情報 ページ) を表示します。
- 表示、編集、削除、および受講者を追加します。
- 編集のコースを表示します。
- 編集講習においてインストラクターを表示します。
- 部門の編集を表示します。

いくつかの代表的なページのスクリーン ショットを次に示します。

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>展開に影響するアプリケーション機能の確認

アプリケーションの次の機能は、展開する方法、展開する必要があるかに影響します。 これらの各操作は、系列内の次のチュートリアルでさらに詳しく説明します。

- Contoso 大学では、SQL Server Compact データベースを使用して、受講者やインストラクター名などのアプリケーション データを格納します。 データベースには、テスト データと、実稼働データの両方が含まれているし、実稼働環境に展開するときにテスト データを除外する必要があります。 一連のチュートリアルで後で、SQL Server に SQL Server Compact から移行します。
- アプリケーションでは、SQL Server Compact データベースにユーザー アカウント情報を格納、ASP.NET メンバーシップ システムを使用します。 アプリケーションでは、制限された情報へのアクセスを持つ管理者ユーザーを定義します。 テスト アカウントを持たないが、1 人の管理者アカウントを使用して、メンバーシップ データベースを展開する必要があります。
- アプリケーション データベースと、メンバーシップ データベースを使用するため SQL Server Compact データベース エンジンとデータベース自体と同様に、ホスティング プロバイダーにデータベース エンジンを展開する必要があります。
- アプリケーションは、メンバーシップ システムは、SQL Server Compact データベースにデータを格納できるように、ASP.NET ユニバーサル メンバーシップ プロバイダーを使用します。 ユニバーサル メンバーシップ プロバイダーを格納するアセンブリは、アプリケーションと共に配置する必要があります。
- アプリケーションでは、Entity Framework 5.0 を使用して、アプリケーション データベースのデータにアクセスします。 Entity Framework 5.0 を含むアセンブリは、アプリケーションと共に配置する必要があります。
- アプリケーションでは、サード パーティ製のエラー ログ機能およびユーティリティのレポートを使用します。 このユーティリティは、アプリケーションと共に配置する必要がありますアセンブリで提供されます。
- エラーのログ記録ユーティリティは、ファイル フォルダーに、XML ファイルにエラー情報を書き込みます。 配置済みのサイトで ASP.NET を実行するアカウントが、このフォルダーの書き込みアクセス許可をこのフォルダーを展開から除外する必要があること確認する必要があります。 (それ以外の場合、テスト環境からのエラー ログのデータを実稼働環境に展開するとや、実稼働のエラー ログ ファイルを削除する可能性があります)。
- アプリケーションに変更する必要がある設定が含まれています、展開済みで*Web.config*先の環境 (テストまたは実稼働)、およびその他の設定によっては、ビルドの変更が必要に応じてファイル(デバッグまたはリリース) を構成します。
- Visual Studio ソリューションには、クラス ライブラリ プロジェクトが含まれています。 このプロジェクトで生成されるアセンブリのみを展開すると、プロジェクト自体ではありません。

系列の最初のチュートリアルでは、サンプルの Visual Studio プロジェクトをダウンロードし、アプリケーションを展開する方法に影響するサイトの機能を確認しました。 次のチュートリアルでは、展開準備で自動的に処理するのには次の作業の一部を設定します。 他のユーザーを処理する手動でします。

> [!div class="step-by-step"]
> [次へ](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
