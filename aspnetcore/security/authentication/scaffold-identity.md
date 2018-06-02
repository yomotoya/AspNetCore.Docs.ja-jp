---
title: ASP.NET Core プロジェクトに scaffold Id
author: rick-anderson
description: ASP.NET Core プロジェクト内の Id をスキャフォールディングする方法を説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: a43b7bbaf1f90d3373b3846bc3f4f32be6b80bd4
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729611"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>ASP.NET Core プロジェクトに scaffold Id

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 2.1 以降提供[ASP.NET Core Id](xref:security/authentication/identity)として、 [Razor クラス ライブラリ](xref:mvc/razor-pages/ui-class)です。 Id を含むアプリケーションでは、選択的に Identity Razor クラス ライブラリ (RCL) に含まれるソース コードを追加する scaffolder を適用できます。 コードを変更して、動作を変更できるように、ソース コードを生成する可能性があります。 たとえば、scaffolder 登録で使用するコードを生成するように指示できます。 生成されたコードは、Identity RCL で同じコードよりも優先されます。

実行するアプリケーション**いない**含める認証 RCL Identity パッケージを追加する scaffolder を適用できます。 生成される Id コードを選択した場合のオプションがあります。

Scaffolder に必要なコードのほとんどが生成されますが、プロセスを完了するようにプロジェクトを更新する必要があります。 このドキュメントでは、Identity スキャフォールディング更新を完了するために必要な手順について説明します。

Identity scaffolder を実行すると、 *ScaffoldingReadme.txt*プロジェクト ディレクトリにファイルを作成します。 *ScaffoldingReadme.txt*ファイルには、Id のスキャフォールディング更新を完了に必要な条件での一般的な手順が含まれています。 このドキュメントには、読み取りよりも詳細な手順が含まれています、 *ScaffoldingReadme.txt*ファイル。

ファイルの相違点を表示し、変更を取り消すには、ソース管理システムを使用することをお勧めします。 Identity scaffolder を実行した後、変更を検査します。

## <a name="scaffold-identity-into-an-empty-project"></a>空のプロジェクトに scaffold identity

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

次の強調表示されている呼び出しを追加、`Startup`クラス。

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>既存の承認はしない Razor プロジェクトに scaffold identity

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

ように構成された id *Areas/Identity/IdentityHostingStartup.cs*です。 詳細については、次を参照してください。 [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)です。

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

`Configure`のメソッド、`Startup`クラス、呼び出し[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)後`UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>レイアウトの変更

省略可能: 部分のログインを追加する (`_LoginPartial`) をレイアウト ファイル。

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>権限を持つ Razor プロジェクトに scaffold identity

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
いくつかの Id オプションを構成*Areas/Identity/IdentityHostingStartup.cs*です。 詳細については、次を参照してください。 [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)です。

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>既存の承認はしない MVC プロジェクトに scaffold identity

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

省略可能: 部分のログインを追加する (`_LoginPartial`) に、 *Views/Shared/_Layout.cshtml*ファイル。

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* 移動、 *Pages/Shared/_LoginPartial.cshtml*ファイルの名前を*Views/Shared/_LoginPartial.cshtml*

ように構成された id *Areas/Identity/IdentityHostingStartup.cs*です。 詳細については、IHostingStartup を参照してください。

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

呼び出す[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)後`UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>権限を持つ MVC プロジェクトに scaffold identity

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

削除、*ページ/共有*フォルダーとそのフォルダー内のファイルです。
