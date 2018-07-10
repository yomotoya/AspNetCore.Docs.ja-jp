<a name="codegenerator"></a> <span data-ttu-id="89736-101">次の表で、ASP.NET Core コード ジェネレーターのパラメーターについて詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="89736-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="89736-102">パラメーター</span><span class="sxs-lookup"><span data-stu-id="89736-102">Parameter</span></span>               | <span data-ttu-id="89736-103">説明</span><span class="sxs-lookup"><span data-stu-id="89736-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="89736-104">-m</span><span class="sxs-lookup"><span data-stu-id="89736-104">-m</span></span>  | <span data-ttu-id="89736-105">モデルの名前。</span><span class="sxs-lookup"><span data-stu-id="89736-105">The name of the model.</span></span> |
| <span data-ttu-id="89736-106">-dc</span><span class="sxs-lookup"><span data-stu-id="89736-106">-dc</span></span>  | <span data-ttu-id="89736-107">データ コンテキスト。</span><span class="sxs-lookup"><span data-stu-id="89736-107">The data context.</span></span> |
| <span data-ttu-id="89736-108">-udl</span><span class="sxs-lookup"><span data-stu-id="89736-108">-udl</span></span> | <span data-ttu-id="89736-109">既定のレイアウトを使用します。</span><span class="sxs-lookup"><span data-stu-id="89736-109">Use the default layout.</span></span> |
| <span data-ttu-id="89736-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="89736-110">-outDir</span></span> | <span data-ttu-id="89736-111">ビューを作成するための相対出力フォルダー パス。</span><span class="sxs-lookup"><span data-stu-id="89736-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="89736-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="89736-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="89736-113">[編集] および [作成] ページに `_ValidationScriptsPartial` を追加します。</span><span class="sxs-lookup"><span data-stu-id="89736-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="89736-114">次のように、`h` スイッチを使用して、`aspnet-codegenerator razorpage` コマンドに関するヘルプを取得します。</span><span class="sxs-lookup"><span data-stu-id="89736-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
