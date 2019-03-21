---
ms.openlocfilehash: 698a127120f7f52672ceb927ff24eb1b02f91521
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320281"
---
<span data-ttu-id="a8795-101">生成された Id のデータベース コードが必要です[Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/)します。</span><span class="sxs-lookup"><span data-stu-id="a8795-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="a8795-102">移行を作成し、データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="a8795-102">Create a migration and update the database.</span></span> <span data-ttu-id="a8795-103">たとえば、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a8795-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8795-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8795-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a8795-105">Visual Studio で**パッケージ マネージャー コンソール**:</span><span class="sxs-lookup"><span data-stu-id="a8795-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a8795-106">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a8795-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="a8795-107">"CreateIdentitySchema"名前のパラメーター、`Add-Migration`コマンドは任意です。</span><span class="sxs-lookup"><span data-stu-id="a8795-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="a8795-108">`"CreateIdentitySchema"` 移行をについて説明します。</span><span class="sxs-lookup"><span data-stu-id="a8795-108">`"CreateIdentitySchema"` describes the migration.</span></span>
