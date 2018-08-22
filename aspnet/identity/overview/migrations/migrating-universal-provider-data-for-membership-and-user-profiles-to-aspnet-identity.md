---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: メンバーシップと ASP.NET identity (c#) のユーザー プロファイル用のユニバーサル プロバイダー データの移行 |Microsoft Docs
author: rustd
description: このチュートリアルでは、ユーザーとロールのデータと既存のアプリのユニバーサル プロバイダーを使用して作成されたユーザー プロファイル データを移行するために必要な手順について説明しています.
ms.author: riande
ms.date: 12/13/2013
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 6115b2a6caca05659f1c35ce97954807a6fb01ae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829272"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>メンバーシップと ASP.NET identity (c#) のユーザー プロファイル用のユニバーサル プロバイダー データの移行
====================
によって[Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Robert McMurray](https://github.com/rmcmurray)、 [Suhas Joshi](https://github.com/suhasj)

> このチュートリアルでは、ユーザーとロールのデータと既存のアプリケーションを ASP.NET Identity のモデルのユニバーサル プロバイダーを使用して作成されたユーザー プロファイル データを移行するために必要な手順について説明します。 ここで説明したアプローチはユーザー プロファイル データを移行すると、SQL のメンバーシップも使用したアプリケーションで使用できます。


Visual Studio 2013 のリリースでは、ASP.NET チームは、新しい ASP.NET Id システムを導入して、詳細については、そのリリース[ここ](../../index.md)します。 Web アプリケーションを移行するアーティクルが[新しい Id システムを SQL メンバーシップ](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)、この記事では、次のユーザーとロール管理のためのプロバイダー モデルを既存のアプリケーションを移行する手順を示しています。新しい Id モデル。 このチュートリアルの焦点は、主にシームレスにそれを新しいシステムにフックするユーザー プロファイル データの移行になります。 移行すると、ユーザーとロールについては、SQL メンバーシップに似ています。 SQL メンバーシップもアプリケーションでは、プロファイル データを移行する方法を使用できます。

たとえば、プロバイダー モデルを使用して Visual Studio 2012 を使用して作成された web アプリで開始します。 しますプロファイル管理のためのコードを追加、ユーザーを登録、ユーザーのプロファイル データを追加、データベース スキーマを移行し、ユーザーおよびロール管理用の Id システムを使用するアプリケーションを変更します。 移行のテストとしてユニバーサル プロバイダーを使用して作成されたユーザーにログインできるようにする必要があり、新しいユーザーが登録できる必要があります。

> [!NOTE]
> 完全なサンプルを見つけることができます[ https://github.com/suhasj/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations)します。


## <a name="profile-data-migration-summary"></a>プロファイル データの移行の概要

移行を開始する前に、プロファイル データを格納するプロバイダー モデルでのエクスペリエンスについて見てみましょう。 ユーザーは、複数の方法で格納できるアプリケーションのプロファイル データ、それらの間で最も一般的なされるを使用してユニバーサル プロバイダーと共に出荷された組み込みのプロファイル プロバイダー。 手順が含まれます

1. プロファイル データを格納するためのプロパティを持つクラスを追加します。
2. 'ProfileBase' を拡張し、ユーザーに対して、上記のプロファイル データを取得するメソッドを実装するクラスを追加します。
3. 既定のプロファイル プロバイダーを使用して有効にする、 *web.config*ファイルし、プロファイル情報へのアクセスに使用する 2 の手順で宣言されたクラスを定義します。

プロファイル情報は、シリアル化された xml と、データベース内の"Profiles"テーブル内のバイナリ データとして格納されます。

新しい ASP.NET Identity システムを使用するアプリケーションを移行した後は、プロファイル情報が逆シリアル化し、ユーザー クラスのプロパティとして格納されます。 各プロパティは、ユーザー テーブル内の列にマップできます。 プロパティをデータの情報をシリアル化/逆シリアル化する必要があるだけでなく、ユーザー クラスを使用して直接に作業できることの利点は、それにアクセスするときの時間します。

## <a name="getting-started"></a>作業の開始

1. Visual Studio 2012 では、新しい ASP.NET 4.5 Web フォーム アプリケーションを作成します。 現在のサンプルは、Web フォーム テンプレートを使用しますが、MVC アプリケーションにも使用できます。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. 新しいフォルダーを作成するには、プロファイル情報を格納するには、' モデル'  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. たとえば、プロファイルに生年月日、市区町村、高さ、およびユーザーの重みの日付を格納お知らせします。 高さと重みは、'PersonalStats' と呼ばれるカスタム クラスとして格納されます。 格納およびプロファイルを取得、'ProfileBase' を拡張するクラスを必要があります。 新しいクラスを取得し、プロファイル情報を格納するには、' AppProfile' を作成しましょう。

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. プロファイルを有効にする、 *web.config*ファイル。 手順 3 で作成したユーザー情報の保存/取得するために使用するクラス名を入力します。

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. 'Account' フォルダー ユーザーからプロファイル データを取得し、格納するには、web フォーム ページを追加します。 プロジェクトを右クリックし、新しい項目の追加 を選択します。 マスター ページ 'AddProfileData.aspx' を含む新しい webforms ページを追加します。 'MainContent' セクションでは、次をコピーします。

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   分離コードで、次のコードを追加します。

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   コンパイル エラーを削除するどの AppProfile でクラスが定義されている名前空間を追加します。
6. アプリを実行し、ユーザー名を持つ新しいユーザーの作成 '**olduser'。** 'AddProfileData' のページに移動し、ユーザーのプロファイル情報を追加します。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

サーバー エクスプ ローラー ウィンドウを使用して"Profiles"テーブル内のシリアル化された xml としてデータが格納されていることを確認できます。 Visual Studio での表示 メニューからのサーバー エクスプ ローラーを選択します。 定義されているデータベースのデータ接続が必要があります、 *web.config*ファイル。 データ接続 をクリックすると、異なるサブ カテゴリが表示されます。 データベースで、別のテーブルを表示し、"Profiles"を右クリックし、プロファイル テーブルに格納されているプロファイル データを表示する ' テーブル データの表示 を選択するには、' テーブル' を展開します。

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>データベース スキーマを移行します。

既存のデータベースの Id システムを使用するために、元のデータベースに追加するフィールドをサポートするために Id のデータベースでスキーマを更新する必要があります。 これ行う SQL スクリプトを使用して、新しいテーブルを作成し、既存の情報をコピーします。 サーバー エクスプ ローラー ウィンドウでテーブルを表示する 'DefaultConnection' を展開します。 テーブルを右クリックし、[新規クエリ] を選択します。

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

SQL スクリプトを貼り付けて[ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt)して実行します。 'DefaultConnection' が更新された場合、新しいテーブルが追加されたことを確認できます。 情報が移行されたことを確認、テーブル内のデータを確認することができます。

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>ASP.NET Identity を使用するアプリケーションへの移行

1. ASP.NET Identity に必要な Nuget パッケージをインストールします。

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Nuget パッケージの管理の詳細についてはあります[ここ](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. テーブル内の既存のデータを操作するには、テーブルにマップし、Id システムでそれらをフックするモデル クラスを作成する必要があります。 Identity コントラクトの一部として、モデル クラスは Identity.Core dll で定義されたインターフェイスを実装する必要がありますか、または Microsoft.AspNet.Identity.EntityFramework で利用できるこれらのインターフェイスの既存の実装を拡張することができます。 使用する既存のクラスの役割、ユーザーのログインおよびユーザーの信頼性情報。 このサンプルのカスタム ユーザーを使用する必要があります。 プロジェクトを右クリックし、新しいフォルダー 'IdentityModels' を作成します。 次に示すように新しい 'User' クラスを追加します。

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   'ProfileInfo' が、ユーザー クラスのプロパティでになったことに注意してください。 そのため、ユーザー クラスを使用してプロファイル データを直接操作することができます。

内のファイルをコピー、 **IdentityModels**と**IdentityAccount**ダウンロード元のフォルダー ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) )。 これらは、残りのモデル クラスと必要なユーザーおよびロール管理を ASP.NET Identity Api を使用して新しいページがあります。 使用されているアプローチは SQL メンバーシップに似ており、詳細については、ある[ここ](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)します。

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>新しいテーブルにプロファイル データのコピー

前述のように、プロファイル テーブルに xml データを逆シリアル化し、AspNetUsers テーブルの列に格納する必要があります。 列が必要なデータを設定するため、残っているはすべて、前の手順で、users テーブルに新しい列が作成されました。 これには、ここには、users テーブル内の新しく作成された列の設定に 1 回実行され、コンソール アプリケーションを使用します。

1. 既存のソリューションでは、新しいコンソール アプリケーションを作成します。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Entity Framework パッケージの最新バージョンをインストールします。
3. コンソール アプリケーションへの参照として上記で作成した web アプリケーションを追加します。 このプロジェクトを右クリック、し、' 参照の追加 ' で、ソリューションは、プロジェクトをクリックし、[ok] をクリックします。
4. コピー、次のコードを Program.cs クラスにします。 このロジックは、'ProfileInfo' のオブジェクトとしてシリアル化して、および元のデータベースに格納する各ユーザーのプロファイル データを読み取る。

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   使用モデルの一部には、対応する名前空間を含める必要がありますので、web アプリケーション プロジェクトの 'IdentityModels' フォルダーで定義されます。
5. 上記のコードは、アプリ内のデータベース ファイルで動作\_前の手順で、web アプリケーション プロジェクトのデータ フォルダーを作成します。 それを参照するには、web アプリケーションの web.config 内の接続文字列で、コンソール アプリケーションの app.config ファイル内の接続文字列を更新します。 また、'AttachDbFilename' プロパティの完全な物理パスを提供します。
6. コマンド プロンプトを開き、上記のコンソール アプリケーションの bin フォルダーに移動します。 実行可能ファイルを実行し、次の図のように、ログ出力を確認します。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. サーバー エクスプ ローラーで 'AspNetUsers' テーブルを開き、プロパティを保持する新しい列のデータを確認します。 対応するプロパティの値で更新します。

## <a name="verify-functionality"></a>機能を確認します。

古いデータベースからユーザーをログインに、ASP.NET Identity を使用して実装されている、新しく追加されたメンバーシップ ページを使用します。 ユーザーは、同じ資格情報を使用してログインできる必要があります。 ロールなどにユーザーを追加する OAuth、パスワードの変更、新しいユーザーの作成、ロールの追加を追加するなど、他の機能をお試しください。

古いユーザーと、新しいユーザーのプロファイル データの取得され、users テーブルに格納されている必要があります。 古いテーブルを参照できなくする必要があります。

## <a name="conclusion"></a>まとめ

この記事では、ASP.NET Identity にメンバーシップ プロバイダー モデルを使用する web アプリケーションを移行するプロセスについて説明します。 さらに、情報の記事 Id システムにフックするユーザーのプロファイル データの移行の概要です。 質問や、アプリを移行するときに発生した問題のための下のコメントを残してください。

*Rick Anderson、Robert McMurray、記事のレビューに協力してくれた*
