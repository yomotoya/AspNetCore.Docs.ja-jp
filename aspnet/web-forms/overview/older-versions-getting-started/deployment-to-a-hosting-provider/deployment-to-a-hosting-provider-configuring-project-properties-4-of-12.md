---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: プロジェクト プロパティの構成 - 12 の 4 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4aa1e55f21e74ae8da67d6d13c35a8de5693ec3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396270"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: プロジェクト プロパティの構成 - 4/12
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。 Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。 シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Azure App Service Web Apps にデプロイする方法を示していますチュートリアルでは、次を参照してください。 [ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。


## <a name="overview"></a>概要

一部の展開オプションは、プロジェクト ファイルに格納されているプロジェクトのプロパティで構成されます (、 *.csproj*または *.vbproj*ファイル)。 ほとんどの場合、これらの設定の既定値は目的の動作が使用することができます、**プロジェクト プロパティ**UI は、それらを変更する必要がある場合、これらの設定を使用する Visual Studio に組み込まれています。 このチュートリアルでの展開設定を確認する**プロジェクト プロパティ**します。 配置する空のフォルダーを原因となるプレース ホルダー ファイルを作成します。

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>プロジェクトのプロパティ ウィンドウで、展開設定を構成します。

以下のチュートリアルでわかる、発行プロファイルの展開時の処理に影響する設定ほとんどにはが含まれます。 意識する必要があるいくつかの設定にある、**パッケージ化/発行**のタブ、**プロジェクト プロパティ**ウィンドウ。 これらの設定は各ビルド構成に対して指定 — 数よりも、デバッグ ビルドにリリース ビルドごとに異なる設定があることができます、します。

**ソリューション エクスプ ローラー**を右クリックし、 **ContosoUniversity**プロジェクトで、**プロパティ**、クリックして、**パッケージ化/発行 Web**タブ。

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

ウィンドウが表示されたら、どのビルド構成が現在アクティブなソリューションの設定を示す既定になります。 場合、**構成**ボックスは示しません**アクティブ (リリース)** を選択します**リリース**リリース ビルド構成の設定を表示するためにします。 リリース ビルドをテストおよび運用の両方の環境にデプロイします。

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

**アクティブ (リリース)** または**リリース**リリースのビルド構成を使用して展開するときに有効な値を表示、選択しました。

- **配置する項目**ボックスで、**アプリケーションの実行に必要なファイルのみ**が選択されています。 その他のオプションは**このプロジェクト内のすべてのファイル**または**このプロジェクト フォルダー内のすべてのファイル**します。 既定の選択をそのままにすることではたとえば、ソース コード ファイルを展開しないでください。 この設定は、SQL Server Compact のバイナリ ファイルを含むフォルダーがプロジェクトに含まれるがなぜ理由です。 この設定の詳細については、次を参照してください。**理由はありませんすべてのプロジェクト フォルダーにファイルをデプロイしますか?** で[ASP.NET Web アプリケーション プロジェクトの展開に関する FAQ](https://msdn.microsoft.com/library/ee942158.aspx)します。
- **デバッグ シンボルの生成を除外する**が選択されています。 このビルド構成を使用する場合にデバッグはありません。
- **アプリからファイルを除外する\_データ フォルダー**が選択されていません。 メンバーシップ データベースの SQL Server Compact、ファイルはそのフォルダーであり、それをデプロイする必要があります。 データベースの変更が含まれていない更新プログラムを展開するときに、このチェック ボックスを選択します。
- **パブリッシュする前にこのアプリケーションをプリコンパイル**が選択されていません。 ほとんどのシナリオでは、web アプリケーション プロジェクトをプリコンパイルする必要はありません。 このオプションの詳細については、次を参照してください。[パッケージ化/発行 Web] タブの [プロジェクトのプロパティ](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx)と[プリコンパイルの設定 ダイアログの高度な](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx)します。
- **SQL のパッケージ化/発行 タブで構成されているすべてのデータベースを含める**が選択されている場合、このオプションも何も起こりません今すぐ構成していないためですが、**パッケージ化/発行 SQL**  タブ。そのタブは、SQL Server データベースを展開するための唯一のオプションを使用する従来のデータベース配置方法によってです。 使用して、**パッケージ化/発行 SQL**  タブで、 [SQL Server への移行](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)チュートリアル。
- **Web 展開パッケージの設定**セクションは 1 回のクリックを使用しているためには適用されませんこれらのチュートリアルで発行します。

変更、**構成** ボックスの一覧をデバッグすると、デバッグ ビルドの場合は、既定の設定を参照してください。 値が以外は同じですが、**生成されたデバッグ シンボルを除外する**がオフになって、デバッグ ビルドをデプロイするときにデバッグできるようにします。

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Elmah フォルダーがデプロイされることを確認をします。

前のチュートリアルで学習したように、 [Elmah NuGet パッケージ](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)エラー ログとレポート機能を提供します。 Contoso University アプリケーション エラーの詳細をという名前のフォルダーに格納する Elmah が構成されています*Elmah*:

![Elmah フォルダー](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

共通の要件は、展開から特定のファイルやフォルダーの除外別の例には、ユーザーがファイルをアップロードしたフォルダーがあります。 ログ ファイルをしたくないか、実稼働環境に展開する、開発環境で作成されたファイルをアップロードします。 運用環境に存在するファイルを削除する展開プロセスをしたくない運用環境に更新プログラムを展開する場合。 (設定に応じて、配置オプションを展開するときに、転送先サイトがソース サイトではなく、ファイルが存在する場合、Web 配置から削除しますが、変換先。)

このチュートリアルでは、前述のように、**配置する項目**オプション、**パッケージ化/発行 Web**に設定されているタブ**ファイルにのみこのアプリケーションを実行する必要**します。 その結果、開発で Elmah によって作成されるログ ファイルは配置されません、これは、発生します。 (プロジェクトに含まれる展開する必要がされ、その**ビルド アクション**プロパティに設定する必要があります**コンテンツ**します。 詳細については、次を参照してください。**理由はありませんすべてのプロジェクト フォルダーにファイルをデプロイしますか?** で[ASP.NET Web アプリケーション プロジェクトの展開に関する FAQ](https://msdn.microsoft.com/library/ee942158.aspx))。 ただし、Web Deploy いないフォルダーが作成されます、移行先サイトにある少なくとも 1 つのファイルをコピーする場合を除き、します。 このために追加、 *.txt*ファイルをフォルダーがコピーされるように、プレース ホルダーとして機能するフォルダー。

**ソリューション エクスプ ローラー**を右クリックし、 *Elmah*フォルダーで、**新しい項目の追加**、という名前のファイルを作成および*Placeholder.txt*します。 次のテキストを入力:「をフォルダーに配置を取得するのプレース ホルダー ファイルです」。 ファイルを保存してください。 すべてを行うために、Visual Studio でこのファイルとフォルダーを展開するかどうかを確認するためには、**ビルド アクション**プロパティの *.txt*に設定されているファイル**コンテンツ**既定。

これですべての展開のセットアップ タスクを完了しました。 次のチュートリアルでは、Contoso University サイトをテスト環境にデプロイし、し、そこでテストします。

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [次へ](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
