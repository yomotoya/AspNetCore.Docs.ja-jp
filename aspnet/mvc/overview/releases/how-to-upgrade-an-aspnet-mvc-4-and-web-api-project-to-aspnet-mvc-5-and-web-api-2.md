---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: ASP.NET MVC 4 にアップグレードしてから Web API プロジェクトを ASP.NET MVC 5 と Web API 2 の方法 |Microsoft Docs
author: Rick-Anderson
description: ASP.NET MVC 5 と Web API 2 は、属性ルーティング、認証フィルター、およびその他を含む新しい機能のホストをもたらします。
ms.author: riande
ms.date: 10/17/2013
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 2566e201e44ccd9642abda7c7996056c73178fd6
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912853"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 と Web API プロジェクトをアップグレードする方法
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

> ASP.NET MVC 5 と Web API 2 は、属性ルーティング、認証フィルター、およびその他を含む新しい機能のホストをもたらします。 参照してください[ https://www.asp.net/vnext ](https://www.asp.net/core)の詳細。
> 
> このチュートリアルでは、アプリケーションを最新バージョンのアップグレードに必要な手順を説明します。  
> 
> > [!NOTE]
> > 参照してください[ASP.NET および Web Tools for Visual Studio 2013 リリース ノート](../../../visual-studio/overview/2013/release-notes.md)についての最新の MVC 4 と Web API から次のバージョンに変更します。
> 
>   
> 
> この記事は Youngjune 香港特別行政区、Rick Anderson によって書き込まれました ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>アップグレード手順

1. プロジェクトをバックアップします。 このチュートリアルでは、プロジェクト ファイル、パッケージの構成、および web.config ファイルを変更することが必要です。
2. Web API 2 に、global.asax で Web API からのアップグレードの次のように変更します。

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   から

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. プロジェクトを使用するすべてのパッケージは、MVC 5 と Web API 2 と互換性があることを確認します。 次の表に示します、MVC 4 と Web API を変更する必要があるパッケージに関連します。 以下に示すパッケージのいずれかに依存するパッケージがあれば、MVC 5 と Web API 2 と互換性がある新しいバージョンを取得する発行元にお問い合わせください。 それらのパッケージのソース コードがあれば、MVC 5 と Web API 2 の新しいアセンブリを再コンパイルする必要があります。   

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
    | Microsoft.Net.Http | 2.0.x します。 | 2.2.x します。 |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | <o:p> </o:p> | 削除済み |
    | Microsoft.AspNet.WebPages.Administration | <o:p> </o:p> | 削除済み |
    | Microsoft の Web Helpers | <o:p> </o:p> | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Microsoft の Web Helpers が Microsoft.AspNet.WebHelpers に置き換えられました。 最初に、古いパッケージを削除して、新しいパッケージをインストールする必要があります。   
    >   
    > ASP.NET の主要なパッケージ間で相互のバージョンの互換性はありません。 たとえば、MVC 5 は、Razor の 3 としない Razor 2 のみと互換性のあります。
4. Visual Studio でプロジェクトを開きます。
5. インストールされている ASP.NET の NuGet パッケージの次のいずれかを削除します。 パッケージ マネージャー コンソール (PMC) を使用して、これらが削除されます。 PMC を開くには、次のように選択します。、**ツール**メニューから選択し、 **NuGet パッケージ マネージャー、** 選び**パッケージ マネージャー コンソール**します。 プロジェクトでは、これらすべてのなどがありますされません。

    1. `Microsoft.AspNet.WebPages.Administration`  
   このパッケージは通常、MVC 3 から MVC 4 にアップグレードするときに追加されます。 削除するには、PMC で、次のコマンドを実行します。  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   このパッケージとしてブランドされて`Microsoft.AspNet.WebHelpers`します。 削除するには、PMC で、次のコマンドを実行します。  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   このパッケージには、MVC 4 MVC 5 で修正されたバグの回避が含まれています。 削除するには、PMC で、次のコマンドを実行します。  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. PMC を使用してすべての ASP.NET の NuGet パッケージをアップグレードします。 PMC では、次のコマンドを実行します。  
    `Update-Package`  
   `Update-Package`コマンドを任意のパラメーターはすべてのパッケージを更新します。 ID の引数を使用してパッケージを個別に更新できます。 Update コマンドの詳細については、実行`get-help update-package`します。

## <a name="update-the-application-webconfig-file"></a>アプリケーションを更新して*web.config*ファイル

アプリでこれらの変更を必ず*web.config*ファイルではありません、 *web.config*ファイル、*ビュー*フォルダー。

検索、`<runtime>/<assemblyBinding>`セクションし、次の変更します。

1. Name 属性が"System.Web.Mvc"要素では、「5.0.0.0」を「4.0.0.0」からバージョン番号を変更します。 (その要素の 2 つの変更。 を)
2. 名前属性を持つ要素で&quot;System.Web.Helpers"と&quot;System.Web.WebPages&quot; 「2.0.0.0」から「3.0.0.0」のバージョン番号を変更します。 4 つの変更が発生、2 つの各要素にします。

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. 検索、`<appSettings>`セクションし、次に示すように 3.0.0.0 2.0.0.0.0 から webpages:version を更新します。

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. フル以外の任意の信頼レベルを削除します。 例えば:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>更新プログラム、 *web.config* Views フォルダーの下のファイル

アプリケーションの領域を使用している場合も必要がありますは各更新*web.config*ファイル、*ビュー*各区分フォルダーのサブフォルダーです。

1. バージョン「5.0.0.0」バージョン「4.0.0.0」から"System.Web.Mvc"が含まれているすべての要素を更新します。  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Version 3.0.0.0「から」バージョン「2.0.0.0」から"System.Web.WebPages.Razor"を格納するすべての要素を更新します。 このセクションに"System.Web.WebPages"が含まれている場合は、version 3.0.0.0「から」を「2.0.0.0」のバージョンからそれらの要素を更新します。  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. 削除した場合、`Microsoft-Web-Helpers`前の手順で NuGet パッケージをインストール`Microsoft.AspNet.WebHelpers`PMC で次のコマンドを使用します。  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. アプリで使用する場合、 [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx)メソッドに次のコードを追加、 *Web.config*ファイル。

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>最後の手順

ビルドして、アプリケーションをテストします。

プロジェクト ファイルから MVC 4 プロジェクト型 GUID を削除します。

1. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、**プロジェクトのアンロード**します。
2. プロジェクトを右クリックし、&gt;.csproj の編集します。
3. 検索、`ProjectTypeGuids`要素とし、削除、MVC 4 プロジェクト GUID、`{E3E379DF-F4C6-4180-9B81-6769533ABE47}`します。
4. 保存してプロジェクトを開くファイルを閉じます。
5. プロジェクトを右クリックして**プロジェクトの再読み込み**します。
