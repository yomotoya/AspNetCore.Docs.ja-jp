---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Visual Studio を使用して ASP.NET Web 展開: データベース展開の準備 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 21b4a924115daff6ee79ce045330c748b58246ee
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388541"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Visual Studio を使用して ASP.NET Web 展開: データベース展開の準備
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。 系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、データベースの配置の準備完了、プロジェクトを取得する方法を示します。 データベースの構造と一部 (すべて)、アプリケーションの 2 つのデータのデータベースは、テスト、ステージング、および運用環境に配置する必要があります。

通常、アプリケーションを開発するときは、ライブ サイトに配置するデータベースにテスト データを入力します。 ただし、デプロイするいくつかの運用データもがあります。 このチュートリアルでは、Contoso University のプロジェクトを構成し、展開するときに、適切なデータが含まれるように、SQL スクリプトを準備します。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](troubleshooting.md)します。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

サンプル アプリケーションでは、SQL Server Express LocalDB を使用します。 SQL Server Express は、SQL Server の無償のエディションです。 全バージョンの SQL Server と同じデータベース エンジンに基づいているため開発時によく使用されます。 SQL Server Express を使ってテストでき、SQL Server のエディション間で異なる機能をいくつかの例外で、運用環境で同じアプリケーションで動作するように、確実に実行します。

LocalDB は SQL Server Express データベースとして使用することができますの特殊な実行モード *.mdf*ファイル。 LocalDB のデータベース ファイルを保持する、通常、*アプリ\_データ*web プロジェクトのフォルダー。 SQL Server express ユーザー インスタンス機能では使用することもできます *.mdf*ユーザー インスタンスの機能は、ファイルが非推奨とされます。 そのため、LocalDB を使用するため推奨 *.mdf*ファイル。

通常、実稼働 web アプリケーションの SQL Server Express は使用されません。 LocalDB 具体的には使用しないで web アプリケーションを運用環境で使用ため、IIS を使用するものではありません。

Visual Studio 2012 では、LocalDB は、Visual Studio では既定でインストールされます。 Visual Studio 2010 および以前のバージョンでは、Visual Studio; に既定で (LocalDB) なしの SQL Server Express はインストールします。つまり理由としてインストールしたことで前提条件のいずれかの[このシリーズの最初のチュートリアル](introduction.md)します。

SQL Server のエディションに関する詳細については、次のリソースを参照、LocalDB を含む[SQL Server データベースでの作業](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver)します。

## <a name="entity-framework-and-universal-providers"></a>Entity Framework、ユニバーサル プロバイダー

データベースへのアクセス、Contoso University アプリケーションは、.NET Framework で指定していないために、アプリケーションと共に配置する必要があります、次のソフトウェアが必要です。

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (により、ASP.NET メンバーシップ システムは Azure SQL Database を使用する)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

このソフトウェアは、NuGet パッケージに含まれています、ため、プロジェクトが既に設定、必要なアセンブリは、プロジェクトと共に配置されるようにします。 (リンクは、このチュートリアルでダウンロードしたスタート プロジェクトにインストールされている内容よりも新しい可能性のあるこれらのパッケージの現在のバージョン をポイント)。

Azure ではなく、サード パーティのホスティング プロバイダーに展開する場合は、Entity Framework 5.0 以降を使用することを確認します。 Code First Migrations の以前のバージョンが、完全な信頼を必要とし、ほとんどのホスティング プロバイダーは、中程度の信頼でアプリケーションを実行します。 中程度の信頼の詳細については、次を参照してください。、[テスト環境として IIS に配置する](deploying-to-iis.md)チュートリアル。

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Code First Migrations をアプリケーション データベースの配置を構成します。

Contoso University アプリケーションのデータベースは、Code First によって管理され、Code First Migrations を使用してデプロイします。 Code First Migrations を使用してデータベースの配置の概要については、次を参照してください。[このシリーズの最初のチュートリアル](introduction.md)します。

アプリケーション データベースを展開するときに通常しないデプロイするだけですべてのデータのデータベースを開発、運用環境にテスト目的でのみ内のデータの多くはおそらくありますので。 たとえば、テスト データベース内の学生の名前は、架空です。 その一方で、多くの場合、配置できませんデータことのないデータベース構造だけですべての。 テスト データベースでデータの一部は、実際のデータがあり、ユーザーがアプリケーションの使用を開始する場合がある必要があります。 たとえば、データベースには、グレードが有効な値または実際の部署名を含むテーブルがあります。

この一般的なシナリオをシミュレートするには、Code First Migrations を構成します`Seed`に実稼働環境であればするデータのみをデータベースに挿入するメソッド。 これは、 `Seed` Code First に実稼働環境でデータベースを作成した後、運用環境で実行されますので、メソッドがテスト データを挿入しないでください。

以前のバージョンの Code First Migrations がリリースされる前に、が一般的でした`Seed`メソッドを挿入するため、テスト データも、開発中にモデル変更のたびに、データベースは完全に削除され、最初から再作成する必要があります。 テスト データを保持するデータベースの変更後の Code First Migrations でにテスト データを含むため、`Seed`メソッドは必要ありません。 ダウンロードしたプロジェクトのすべてのデータを含むメソッドを使用して、`Seed`初期化子クラスのメソッド。 このチュートリアルではその初期化子クラスを無効になりますと`enable Migrations. Then you'll update the `シード ' メソッドの移行の構成でクラスの運用環境に挿入する唯一のデータを挿入できるようにします。

次の図は、アプリケーション データベースのスキーマを示しています。

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

これらのチュートリアルについてを想定します、`Student`と`Enrollment`サイトが最初に展開された場合に、テーブルが空にする必要があります。 その他のテーブルには、アプリケーションの運用時にプリロードする必要があるデータが含まれます。

### <a name="disable-the-initializer"></a>初期化子を無効にします。

Code First Migrations を使用する場合は、以降は使用する必要はありません、 `DropCreateDatabaseIfModelChanges` Code First 初期化子。 この初期化子のコードは、 *SchoolInitializer.cs* ContosoUniversity.DAL プロジェクト内のファイル。 設定、`appSettings`の要素、 *Web.config*ファイルにより、アプリケーションを初めてデータベースにアクセスしようとするたびに実行するこの初期化子。

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

アプリケーションを開く*Web.config*ファイルし削除、またはコメント アウト、 `add` Code First の初期化子クラスを指定する要素。 `appSettings`要素は次のようになります。

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> 初期化子クラスを指定することもできますが、それを呼び出すことによって行います`Database.SetInitializer`で、`Application_Start`メソッドで、 *Global.asax*ファイル。 そのメソッドを使用して、初期化子を指定するプロジェクトでの移行を有効にする場合は、そのコード行を削除します。


> [!NOTE]
> Visual Studio 2013 を使用している場合は、手順 2 および 3 の間で、次の手順を追加: (a) で PMC を入力してください"更新プログラム パッケージ entityframework-バージョン 6.1.1"EF の現在のバージョンを取得します。 (B) ビルド プロジェクトをビルドのエラーの一覧を取得し、修正します。 存在を右クリックしてステートメントを使用して、場所、必要な追加の解決 をクリックを不要になった名前空間の using ステートメントを削除し、System.Data.EntityState の出現箇所を System.Data.Entity.EntityState に変更します。


### <a name="enable-code-first-migrations"></a>Code First Migrations を有効にします。

1. ContosoUniversity プロジェクト (ContosoUniversity.DAL されません) をスタートアップ プロジェクトとして設定されていることを確認します。 **ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、選択、**スタートアップ プロジェクトとして設定**します。 Code First Migrations は、データベース接続文字列を検索するスタートアップ プロジェクトになります。
2. **ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー** (または**NuGet パッケージ マネージャー**) し、**パッケージ マネージャー コンソール**します。

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. 上部にある、**パッケージ マネージャー コンソール**ウィンドウとして、既定のプロジェクトとし、at ContosoUniversity.DAL を選択して、`PM>`プロンプトは「有効な移行」を入力します。

    ![enable-migrations コマンド](preparing-databases/_static/image4.png)

    (というエラーが発生した場合、*有効にする-移行*コマンドが認識されないコマンドを入力します*EntityFramework の更新プログラム パッケージの再インストール*もう一度やり直してください)。

    このコマンドは、作成、*移行*ContosoUniversity.DAL プロジェクト、およびそのフォルダーでは、2 つのファイルをそのフォルダー内に配置: *Configuration.cs*移行、および、構成に使用できるファイル*InitialCreate.cs*データベースを作成する最初の移行用のファイル。

    ![Migrations フォルダー](preparing-databases/_static/image5.png)

    DAL のプロジェクトを選択した、**既定のプロジェクト**のドロップダウン リスト、**パッケージ マネージャー コンソール**ため、 `enable-migrations` Code First を含むプロジェクトでコマンドを実行する必要がありますコンテキスト クラスです。 そのクラスは、クラス ライブラリ プロジェクトでは、Code First Migrations は、ソリューションのスタートアップ プロジェクト内のデータベース接続文字列が検索されます。 ContosoUniversity ソリューションで web プロジェクトがスタートアップ プロジェクトとして設定されています。 Visual Studio のスタートアップ プロジェクトとして、接続文字列を含むプロジェクトを指定しない場合は、PowerShell コマンドでスタートアップ プロジェクトを指定できます。 コマンドの構文を表示するには、コマンドを入力`get-help enable-migrations`します。

    `enable-migrations`コマンドは、最初の移行を自動的に作成、データベースが既に存在します。 別の方法は移行、データベースを作成します。 そのために使用**サーバー エクスプ ローラー**または**SQL Server オブジェクト エクスプ ローラー** Migrations を有効にする前に、ContosoUniversity データベースを削除します。 移行を有効にした後、"追加移行 InitialCreate"コマンドを入力して、最初の移行を手動で作成します。 コマンド「- データベースの更新」を入力して、データベースを作成できます。

### <a name="set-up-the-seed-method"></a>Seed メソッドを設定します。

このチュートリアルでは、コードを追加して固定のデータを追加します、`Seed`の Code First Migrations メソッド`Configuration`クラス。 Code First Migrations を呼び出し、`Seed`すべて移行後のメソッド。

以降、`Seed`メソッドの実行ごとの移行後に、データが既にテーブルの最初の移行後にします。 使用してこのような状況を処理するために、`AddOrUpdate`既に挿入されている、またはまだ存在していない場合は、それらを挿入する行を更新するメソッド。 `AddOrUpdate`メソッドには、シナリオに最適な選択肢をできない可能性があります。 詳細については、次を参照してください。 [EF 4.3 AddOrUpdate メソッドを使用して対処](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman のブログ。

1. 開く、 *Configuration.cs*ファイルを開き、コメント、`Seed`メソッドを次のコード。

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. 参照`List`必要ないため、その下に赤い波線がある、`using`まだその名前空間のステートメント。 インスタンスの 1 つを右クリックして`List` をクリック**解決**、順にクリックします**System.Collections.Generic を使用して**します。

    ![ステートメントを使用すると解決します。](preparing-databases/_static/image6.png)

    このメニュー項目が次のコードを追加します、`using`ファイルの上部にステートメント。

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. CTRL + SHIFT+B プロジェクトをビルドするキーを押します。

プロジェクトは、デプロイする準備ができました、 *ContosoUniversity*データベース。 最初に、データベースにアクセスするページに移動して、実行、アプリケーションの配置後 Code First はデータベースを作成し、実行`Seed`メソッド。

> [!NOTE]
> コードを追加、`Seed`メソッドは、固定データがデータベースに挿入できるさまざまな方法の 1 つです。 代わりに、コードを追加し、`Up`と`Down`各移行クラスのメソッド。 `Up`と`Down`メソッドには、データベースの変更を実装するコードが含まれています。 これらの例が表示されます、[データベース更新の展開](deploying-a-database-update.md)チュートリアル。
> 
> 使用して SQL ステートメントを実行するコードを記述することも、`Sql`メソッド。 たとえば、Department テーブルを予算の列を追加し、移行の一環として、すべての部門予算を $1,000.00 を初期化するために必要な場合は、コードの次の行を追加でした、`Up`その移行のためのメソッド。
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>メンバーシップ データベースの配置のスクリプトを作成します。

Contoso University アプリケーションでは、ASP.NET メンバーシップ システムとフォーム認証を使用して、ユーザー認証および承認します。 **更新クレジット**ページには、管理者ロールのユーザーにのみにアクセスします。

アプリケーションを実行し、をクリックして**コース**、 をクリックし、**更新クレジット**します。

![更新プログラムのクレジットをクリックします。](preparing-databases/_static/image7.png)

**ログイン**ために、ページが表示されます、**更新クレジット**ページには、管理者特権が必要です。

入力*管理者*としてユーザー名と*devpwd*パスワードをクリックします**ログイン**。

![ログイン ページ](preparing-databases/_static/image8.png)

**更新クレジット**ページが表示されます。

![クレジットのページを更新します。](preparing-databases/_static/image9.png)

ユーザーおよびロールの情報は、 *aspnet ContosoUniversity*によって指定されているデータベース、 **DefaultConnection**内の接続文字列、 *Web.config*ファイル。

移行を使用して、それをデプロイすることはできませんので、このデータベースは Entity Framework Code First によって、管理されていません。 DbDacFx プロバイダー、データベース スキーマのデプロイに使用して、データベース テーブルに初期データを挿入するスクリプトを実行する発行プロファイルを構成します。

> [!NOTE]
> (ASP.NET Identity という名前になりました)、新しい ASP.NET メンバーシップ システムは、Visual Studio 2013 で導入されました。 新しいシステムでは、アプリケーションとメンバーシップ テーブルの両方を同じデータベースに保持することができ、両方を展開する Code First Migrations を使用することができます。 サンプル アプリケーションでは、Code First Migrations を使用して展開することはできません前 ASP.NET メンバーシップ システムを使用します。 このメンバーシップ データベースを配置する手順は、Entity Framework Code First によって作成されていない SQL Server データベースを配置する、アプリケーションが必要なその他のシナリオにも適用されます。


ここで、通常は必要ありません開発で必要のある運用環境で同じデータ。 最初に、サイトを展開するときに、ほとんどまたはすべてのテストを作成するユーザー アカウントを除外する一般的なは。 ダウンロードしたプロジェクトの 2 つのメンバーシップ データベースがそのため、: *aspnet ContosoUniversity.mdf*開発ユーザーと*aspnet-ContosoUniversity-Prod.mdf*運用環境のユーザーとします。 このチュートリアルでは、ユーザー名が両方のデータベースで同じ:*管理者*と*nonadmin*します。 両方のユーザー パスワードを使用して*devpwd* 、開発データベースと*prodpwd*実稼働データベースでします。

開発ユーザーは、テスト環境とステージング環境と運用環境を運用環境のユーザーにデプロイします。 そのためにはこのチュートリアルで開発用に 1 つと、運用環境用に 2 つの SQL スクリプトを作成し、以降のチュートリアルでは、それらを実行する、発行プロセスを構成します。

> [!NOTE]
> メンバーシップ データベースには、アカウントのパスワードのハッシュが格納されます。 1 台のコンピューターから別のアカウントをデプロイするには、ハッシュのルーチンも、これは、移行元コンピューターには、移行先サーバーで異なるハッシュを生成しないことを確認してください。 既定のアルゴリズムを変更しない限り、ASP.NET ユニバーサル プロバイダーを使用する場合に、同じハッシュを生成、されます。 既定のアルゴリズムはで指定された、HMACSHA256、**検証**の属性、 **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** Web.config ファイル内の要素。


SQL Server Management Studio (SSMS) を使用して、またはサード パーティ製ツールを使用して、データ デプロイ スクリプトを手動で作成することができます。 このチュートリアルの残りの部分が SSMS では、その方法を示しますをインストールして SSMS を使用しない場合は、プロジェクトの完成版からスクリプトを入手して、ソリューション フォルダーに格納する場所のセクションにスキップします。

SSMS をインストールするインストールから[ダウンロード センター: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062)をクリックして[enu \x64\sqlmanagementstudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe)または[Enu \x86\sqlmanagementstudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe)します。 問題の 1 つを選択した場合、システムのインストールに失敗し、もう 1 つを試みることができます。

(600 メガバイトのダウンロードは、このことに注意してください。 長い時間がかかる場合がありますをインストールして、コンピューターの再起動が必要になります)。

SQL Server インストール センターの最初のページで次のようにクリックします。 **SQL Server の新規スタンドアロン インストールまたは既存のインストールに機能の追加**、既定の選択を受け入れ、指示に従います。

### <a name="create-the-development-database-script"></a>開発データベース スクリプトを作成します。

1. SSMS を実行します。
2. **サーバーへの接続** ダイアログ ボックスに、入力 *(localdb) \v11.0*として、**サーバー名**、まま**認証**に設定**Windows 認証**、 をクリックし、 **Connect**します。

    ![SSMS がサーバーに接続します。](preparing-databases/_static/image10.png)
3. **オブジェクト エクスプ ローラー**ウィンドウで、展開**データベース**、右クリックして**aspnet ContosoUniversity**、 をクリックして**タスク**、順にクリックします**スクリプトを生成する**します。

    ![SSMS は、スクリプトを生成します。](preparing-databases/_static/image11.png)
4. **発行スクリプトの生成と**ダイアログ ボックスで、をクリックして**スクリプト作成オプションの設定**します。

    スキップすることができます、**オブジェクトの選択**既定値はステップ**データベース全体とすべてのデータベース オブジェクトをスクリプト**対象です。
5. **[詳細設定]** をクリックします。

    ![SSMS のスクリプト作成オプション](preparing-databases/_static/image12.png)
6. **スクリプト作成オプションの高度な** ダイアログ ボックスで、下にスクロール**スクリプトへのデータ型**、 をクリックし、**データのみ**ドロップダウン リストのオプション。
7. 変更**スクリプトを使用してデータベース**に**False**します。 ステートメントを使用では、Azure SQL Database の有効なありませんし、テスト環境で SQL Server Express への展開は必要ありません。

    ![SSMS スクリプト データのみ、USE ステートメントはありません。](preparing-databases/_static/image13.png)
8. **[OK]** をクリックします。
9. **発行スクリプトの生成と** ダイアログ ボックスで、**ファイル名**ボックスは、スクリプトを作成する場所を指定します。 ソリューション フォルダー (ContosoUniversity.sln ファイルを含むフォルダー) とファイル名をパスに変更*aspnet-データ-dev.sql*します。
10. をクリックして**次へ**に進むには、**概要** タブをクリックして**次へ**スクリプトを作成するには、もう一度です。

    ![SSMS スクリプトの作成](preparing-databases/_static/image14.png)
11. **[完了]** をクリックします。

### <a name="create-the-production-database-script"></a>実稼働データベースのスクリプトを作成します。

運用データベースでプロジェクトを実行していないため、LocalDB インスタンスにまだその添付されません。 したがって、最初にデータベースをアタッチする必要があります。

1. SSMS で**オブジェクト エクスプ ローラー**、右クリック**データベース** をクリック**アタッチ**します。

    ![SSMS をアタッチします。](preparing-databases/_static/image15.png)
2. **データベースのアタッチ**ダイアログ ボックスで、をクリックして**追加**順に移動します、 *aspnet-ContosoUniversity-Prod.mdf*ファイル、*アプリ\_データ*フォルダー。

     ![.Mdf ファイルをアタッチする SSMS の追加](preparing-databases/_static/image16.png)
3. **[OK]** をクリックします。
4. 運用環境のファイルのスクリプトを作成する前に使用した同じ手順に従います。 スクリプト ファイルに名前を*aspnet-データ-prod.sql*します。

## <a name="summary"></a>まとめ

両方のデータベースをデプロイする準備が整いましたし、ソリューション フォルダーに 2 つのデータ配置スクリプトがあります。

![データ デプロイ スクリプト](preparing-databases/_static/image17.png)

次のチュートリアルでは、展開に影響するプロジェクトの設定を構成して、自動に設定する*Web.config*デプロイされたアプリケーションで別にする必要がある設定のファイルの変換。

## <a name="more-information"></a>説明

NuGet の詳細については、次を参照してください。 [nuget プロジェクトのライブラリを管理](https://msdn.microsoft.com/magazine/hh547106.aspx)と[NuGet のドキュメント](http://docs.nuget.org/docs/start-here/overview)します。 NuGet を使用しない場合がインストールされているときの動作を決定する NuGet パッケージを分析する方法を説明する必要があります。 (たとえば、構成が*Web.config*変換、ビルド時などに実行する PowerShell スクリプトを構成します)。NuGet の動作方法について詳しくは、次を参照してください。[を作成すると、パッケージを公開する](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package)と[構成ファイルとソース コード変換](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations)します。

> [!div class="step-by-step"]
> [前へ](introduction.md)
> [次へ](web-config-transformations.md)
