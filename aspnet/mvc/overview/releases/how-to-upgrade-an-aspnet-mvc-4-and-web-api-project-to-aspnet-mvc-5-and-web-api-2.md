---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: ASP.NET MVC 4 にアップグレードし、Web API プロジェクトは、ASP.NET MVC 5 と Web API 2 にする方法 |Microsoft ドキュメント
author: Rick-Anderson
description: ASP.NET MVC 5 と Web API 2 は、属性のルーティング、認証フィルター、いっそうなどの新機能のホストを表示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f61502933a5ba92896ee97cef9cff915fe23831d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874731"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 および Web API プロジェクトをアップグレードする方法
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 と Web API 2 は、属性のルーティング、認証フィルター、いっそうなどの新機能のホストを表示します。 参照してください[ https://www.asp.net/vnext ](https://www.asp.net/core)詳細についてはします。
> 
> このチュートリアルでは、最新バージョンに、アプリケーションをアップグレードするための手順を説明します。  
> 
> > [!NOTE]
> > 参照してください[ASP.NET および Web Tools for Visual Studio 2013 のリリース ノート](../../../visual-studio/overview/2013/release-notes.md)についての最新の MVC 4 および Web API から次のバージョンに変更します。
> 
>   
> 
> この記事は Youngjune 香港特別行政区、Rick Anderson によって書き込まれました ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>アップグレードの手順

1. プロジェクトをバックアップします。 このチュートリアルは、プロジェクト ファイル、パッケージの構成および web.config ファイルを変更することが必要です。
2. Global.asax で Web API から Web API 2 へのアップグレードの次のように変更します。

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   から

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. MVC 5 と Web API 2 と互換性のある、プロジェクトを使用するすべてのパッケージを確認します。 以下の表に示します MVC 4 Web API は、パッケージを関連するよりも、変更する必要があります。 以下に示すパッケージのいずれかに依存しているパッケージがある場合は、MVC 5 と Web API 2 と互換性がある新しいバージョンを取得する発行者に問い合わせてください。 場合は、それらのパッケージのソース コードがある場合は、新しい MVC 5 と Web API 2 のアセンブリに再コンパイルする必要があります。   

    | **パッケージ Id** | **以前のバージョン** | **新しいバージョン** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | 4.0.x.x | 5.0.0 |
    | Microsoft.Net.Http | 2.0.x. | 2.2.x. |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | <o:p> </o:p> | 削除済み |
    | Microsoft.AspNet.WebPages.Administration | <o:p> </o:p> | 削除済み |
    | Microsoft Web Helpers | <o:p> </o:p> | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Microsoft Web Helpers は Microsoft.AspNet.WebHelpers に置き換えられました。 最初に、古いパッケージを削除し、新しいパッケージをインストールする必要があります。   
    >   
    > ASP.NET の主要なパッケージ間で相互のバージョンの互換性はありません。 たとえば、MVC 5 は互換性がのみ Razor 3、および Razor 2 ではありません。
4. Visual Studio 2013 では、プロジェクトを開きます。
5. インストールされている ASP.NET の NuGet パッケージの次のいずれかを削除します。 パッケージ マネージャー コンソール (PMC) を使用して、これらが削除されます。 PMC を開くには、**ツール**クリックしてメニュー**ライブラリ パッケージ マネージャー**を選択し、 **Package Manager Console**です。 プロジェクトでは、これらすべての含まれません可能性があります。

    1. `Microsoft.AspNet.WebPages.Administration`  
   このパッケージは通常、MVC 3 から MVC 4 にアップグレードするときに追加されます。 削除するには、PMC で、次のコマンドを実行します。  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   このパッケージとしてブランドされて`Microsoft.AspNet.WebHelpers`です。 削除するには、PMC で、次のコマンドを実行します。  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   このパッケージには、MVC 4 MVC 5 で修正されたバグの回避が含まれています。 削除するには、PMC で、次のコマンドを実行します。  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. PMC を使用してすべての ASP.NET NuGet パッケージをアップグレードします。 PMC では、次のコマンドを実行します。  
    `Update-Package`  
   `Update-Package`コマンドのすべてのパッケージを更新して、すべてのパラメーターです。 ID の引数を使用してパッケージを個別に更新できます。 更新コマンドの詳細については、実行`get-help update-package`です。

## <a name="update-the-application-webconfig-file"></a>アプリケーションを更新*web.config*ファイル

アプリでこれらの変更を必ず*web.config* 、ファイル、 *web.config*ファイルで、*ビュー*フォルダーです。

検索、`<runtime>/<assemblyBinding>`セクションし、次の変更します。

1. Name 属性が"System.Web.Mvc"要素では、「5.0.0.0」を「4.0.0.0」からバージョン番号を変更します。 (その要素に 2 つの変更。 を)
2. Name 属性を持つ要素で&quot;System.Web.Helpers"と&quot;System.Web.WebPages&quot; 「2.0.0.0」から「3.0.0.0」のバージョン番号を変更します。 4 つの変更が発生、2 つの各要素にします。

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. 検索、`<appSettings>`セクションし、次に示すように 3.0.0.0 2.0.0.0.0 から webpages:version を更新します。

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. フル以外の任意の信頼レベルを削除します。 例えば:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>更新プログラム、 *web.config* Views フォルダーの下のファイル

場合は、アプリケーションは、領域を使用して、それぞれを更新する必要がありますする*web.config*ファイルで、*ビュー*領域の各フォルダーのサブフォルダーです。

1. バージョン「4.0.0.0」からバージョン「5.0.0.0」に"System.Web.Mvc"が含まれているすべての要素を更新します。  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. バージョン「2.0.0.0」からバージョン「3.0.0.0」に"System.Web.WebPages.Razor"を含むすべての要素を更新します。 ここ"System.Web.WebPages"が含まれている場合は、バージョン「2.0.0.0」バージョン「3.0.0.0」へのそれらの要素を更新します。  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. 削除した場合、`Microsoft-Web-Helpers`前の手順で NuGet パッケージのインストール`Microsoft.AspNet.WebHelpers`PMC で次のコマンドで。  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. アプリで使用する場合、 [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx)メソッドに以下を追加、 *Web.config*ファイル。

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>最後の手順

ビルドし、アプリケーションをテストします。

プロジェクト ファイルから MVC 4 プロジェクトの種類の GUID を削除します。

1. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、**プロジェクトのアンロード**です。
2. プロジェクトを右クリックし、ProjectName.csproj の編集を選択します。
3. 検索、`ProjectTypeGuids`要素し、[削除]、MVC 4 プロジェクト GUID、`{E3E379DF-F4C6-4180-9B81-6769533ABE47}`です。
4. 保存して、開いているプロジェクト ファイルを閉じます。
5. プロジェクトを右クリックし **プロジェクトの再読み込み**です。
