---
uid: mvc/overview/advanced/custom-mvc-templates
title: カスタム MVC テンプレート |Microsoft Docs
author: joeloff
description: VSIX 拡張機能としてテンプレートを作成します。
ms.author: aspnetcontent
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 67cb01e0fae036f01b1851695ae5a4358136d28e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809778"
---
<a name="custom-mvc-template"></a><span data-ttu-id="fbe60-103">カスタム MVC テンプレート</span><span class="sxs-lookup"><span data-stu-id="fbe60-103">Custom MVC Template</span></span>
====================
<span data-ttu-id="fbe60-104">によって[Jacques Eloff](https://github.com/joeloff)</span><span class="sxs-lookup"><span data-stu-id="fbe60-104">by [Jacques Eloff](https://github.com/joeloff)</span></span>

<span data-ttu-id="fbe60-105">MVC 3 Tools Update の Visual Studio 2010 のリリースでは、MVC プロジェクトの別のプロジェクト ウィザードが導入されました。</span><span class="sxs-lookup"><span data-stu-id="fbe60-105">The release of MVC 3 Tools Update for Visual Studio 2010 introduced a separate project wizard for MVC projects.</span></span> <span data-ttu-id="fbe60-106">変更が、2 つの要因によって決まります。</span><span class="sxs-lookup"><span data-stu-id="fbe60-106">The change was driven by two factors.</span></span> <span data-ttu-id="fbe60-107">最初に、MVC 3 と剃刀などの追加のビュー エンジンのサポートで新しいテンプレートの概要は、Visual Studio で新しいプロジェクト ダイアログを overcrowding 可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fbe60-107">First, the introduction of new templates in MVC 3 and support for additional view engines such as Razor lead to overcrowding the New Project dialog in Visual Studio.</span></span> <span data-ttu-id="fbe60-108">第 2 に、お客様が要望があった機能拡張ポイントと、新しい MVC プロジェクト ウィザードは余裕がある米国これらの要求に応答する機会をします。</span><span class="sxs-lookup"><span data-stu-id="fbe60-108">Second, customers had been asking for extensibility points and the new MVC project wizard would afford us the opportunity to respond to these requests.</span></span>

<span data-ttu-id="fbe60-109">カスタム テンプレートを追加すると、レジストリを使用して、新しいテンプレートを MVC プロジェクト ウィザードに表示されるようにすることに依存する困難な作業がでした。</span><span class="sxs-lookup"><span data-stu-id="fbe60-109">Adding custom templates was an arduous process that relied on using the registry to make new templates visible to the MVC project wizard.</span></span> <span data-ttu-id="fbe60-110">新しいテンプレートの作成者は、内部で、MSI インストール時に必要なレジストリ エントリが作成することを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fbe60-110">The author of a new template had to wrap it inside an MSI to ensure that the necessary registry entries would be created at install time.</span></span> <span data-ttu-id="fbe60-111">代わりを使用可能なテンプレートを含む ZIP ファイルを作成し、エンドユーザーが必要なレジストリ エントリを手動で作成されました。</span><span class="sxs-lookup"><span data-stu-id="fbe60-111">The alternative was to make a ZIP file containing the template available and have the end-user create the required registry entries by hand.</span></span>

<span data-ttu-id="fbe60-112">によって提供される既存のインフラストラクチャの一部を活用することにしましたので、前述の方法のどちらもは理想的な[VSIX](https://msdn.microsoft.com/library/ff363239.aspx)の作成者を簡単にする拡張機能の配布し、MVC 4 以降では、カスタムの MVC テンプレートをインストールします。Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="fbe60-112">Neither of the aforementioned approaches is ideal so we decided to leverage some of the existing infrastructure provided by [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) extensions to make it easier to author, distribute and install custom MVC templates starting with MVC 4 for Visual Studio 2012.</span></span> <span data-ttu-id="fbe60-113">このアプローチによって提供される利点を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-113">Some of the benefits provided by this approach are:</span></span>

- <span data-ttu-id="fbe60-114">VSIX 拡張機能は、さまざまな言語 (c# および Visual Basic) をサポートする複数のテンプレートと複数のビュー エンジン (ASPX と剃刀) に含めることができます。</span><span class="sxs-lookup"><span data-stu-id="fbe60-114">A VSIX extension can contain multiple templates that support different languages (C# and Visual Basic) and multiple view engines (ASPX and Razor).</span></span>
- <span data-ttu-id="fbe60-115">複数の Sku の Visual Studio Express Sku を含む VSIX 拡張機能を対象にできます。</span><span class="sxs-lookup"><span data-stu-id="fbe60-115">A VSIX extension can target multiple SKUs of Visual Studio including Express SKUs.</span></span>
- <span data-ttu-id="fbe60-116">[Visual Studio ギャラリー](https://visualstudiogallery.msdn.microsoft.com/)多様な対象ユーザーに拡張機能の配布を容易にします。</span><span class="sxs-lookup"><span data-stu-id="fbe60-116">The [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/) facilitates distributing the extension to a wide audience.</span></span>
- <span data-ttu-id="fbe60-117">VSIX 拡張機能をアップグレードするには、容易に修正し、カスタム テンプレートの更新を作成します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-117">VSIX extensions can be upgraded making it easier to author corrections and updates to your custom templates.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fbe60-118">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="fbe60-118">Prerequisites</span></span>

- <span data-ttu-id="fbe60-119">ユーザーは、vstemplate ファイルなどの必要なマークアップを含め、プロジェクト テンプレートの作成に慣れておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="fbe60-119">Users need to be familiar with authoring project templates, including the required markup for vstemplate files, etc.</span></span>
- <span data-ttu-id="fbe60-120">ユーザーは、Visual Studio Professional が必要し、以上をインストールします。</span><span class="sxs-lookup"><span data-stu-id="fbe60-120">Users will need to have Visual Studio Professional and higher installed.</span></span> <span data-ttu-id="fbe60-121">Express の Sku は、VSIX プロジェクトの作成をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="fbe60-121">Express SKUs do not support creating VSIX projects.</span></span>
- <span data-ttu-id="fbe60-122">[Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668)をインストールします。</span><span class="sxs-lookup"><span data-stu-id="fbe60-122">[Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) installed.</span></span>

## <a name="example"></a><span data-ttu-id="fbe60-123">例</span><span class="sxs-lookup"><span data-stu-id="fbe60-123">Example</span></span>

<span data-ttu-id="fbe60-124">最初の手順では、c# または Visual Basic を使用して、新しい VSIX プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-124">The first step is to create a new VSIX project using either C# or Visual Basic.</span></span> <span data-ttu-id="fbe60-125">選択**ファイル > 新しいプロジェクト**、順にクリックします**拡張**クリックし、左側のウィンドウで、 **VSIX プロジェクト**。</span><span class="sxs-lookup"><span data-stu-id="fbe60-125">Select **File > New Project**, then click **Extensibility** in the left pane and select the **VSIX Project**.</span></span>

![新しいプロジェクト](custom-mvc-templates/_static/image1.jpg)

<span data-ttu-id="fbe60-127">プロジェクトの作成後に、VSIX デザイナーが開きます。</span><span class="sxs-lookup"><span data-stu-id="fbe60-127">After the project is created, the VSIX designer will be opened.</span></span>

![プロジェクト デザイナーのメタデータ](custom-mvc-templates/_static/image2.jpg)

<span data-ttu-id="fbe60-129">デザイナーを使用して、拡張機能をインストールまたは Visual Studio でインストールされている拡張機能の参照したときに、ユーザーに表示する拡張機能の全般的なプロパティの一部を編集することができます (**ツール > 拡張機能と更新**)。</span><span class="sxs-lookup"><span data-stu-id="fbe60-129">The designer can be used to edit some of the general properties of the extension that will be shown to users when they install the extension or browse the installed extensions in Visual Studio (**Tools > Extensions and Updates**).</span></span> <span data-ttu-id="fbe60-130">一般的な情報 をクリックした後、**インストール ターゲット タブ**します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-130">Once you have completed the general information click on the **Install Targets tab**.</span></span>

![プロジェクト デザイナーのインストールのターゲット](custom-mvc-templates/_static/image3.jpg)

<span data-ttu-id="fbe60-132">このタブを使用して、Sku と、拡張機能によってサポートされている Visual Studio のバージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-132">This tab is used to specify the SKUs and versions of Visual Studio that are supported by your extension.</span></span> <span data-ttu-id="fbe60-133">チェック ボックスをオン**すべてのユーザーに対してこの VSIX がインストールされている**VSIX のマシンごとにインストールを有効にします。</span><span class="sxs-lookup"><span data-stu-id="fbe60-133">Select the checkbox for **This VSIX is installed for all users** to enable per-machine installs of the VSIX.</span></span> <span data-ttu-id="fbe60-134">をクリックして、**新規**Web Developer Express (VWD) などの他の Sku を追加するには、右にボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="fbe60-134">Click on the **New** button on the right to add additional SKUs such as Web Developer Express (VWD).</span></span>

![新しいインストール先を追加します。](custom-mvc-templates/_static/image4.jpg)

<span data-ttu-id="fbe60-136">のみファミリでは、最小の SKU を選択する必要がある場合は、すべての Professional 以上の Sku (Professional、Premium および Ultimate) をサポートするために**です**します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-136">If you intend to support all the Professional and higher SKUs (Professional, Premium and Ultimate) you only need to select the minimum SKU in the family, **Microsoft.VisualStudio.Pro**.</span></span> <span data-ttu-id="fbe60-137">インストールの対象を完了すると、すべての変更を保存してください。</span><span class="sxs-lookup"><span data-stu-id="fbe60-137">Remember to save all your changes once you have completed the Install Targets.</span></span>

![プロジェクト デザイナーのインストールのターゲット](custom-mvc-templates/_static/image5.jpg)

<span data-ttu-id="fbe60-139">**資産** タブは、すべてのコンテンツ ファイルを VSIX に追加するために使用します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-139">The **Assets** tab is used to add all your content files to the VSIX.</span></span> <span data-ttu-id="fbe60-140">使用する代わりに、VSIX のマニフェスト ファイルの生の XML を編集して MVC には、カスタムのメタデータが必要なため、**資産**コンテンツを追加するには、タブ。</span><span class="sxs-lookup"><span data-stu-id="fbe60-140">Since MVC requires custom metadata you will be editing the raw XML of the VSIX manifest file instead of using the **Assets** tab to add content.</span></span> <span data-ttu-id="fbe60-141">VSIX プロジェクト テンプレートの内容を追加することで開始します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-141">Start by adding the template contents to the VSIX project.</span></span> <span data-ttu-id="fbe60-142">フォルダーとその内容の構造が、プロジェクトのレイアウトをミラーリングすることが重要です。</span><span class="sxs-lookup"><span data-stu-id="fbe60-142">It is important that the structure of the folder and the contents mirror the layout of the project.</span></span> <span data-ttu-id="fbe60-143">次の例には、基本的な MVC プロジェクト テンプレートから取得した 4 つのプロジェクト テンプレートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fbe60-143">The example below contains four project templates that were derived from the Basic MVC project template.</span></span> <span data-ttu-id="fbe60-144">プロジェクト テンプレート (ProjectTemplates フォルダーの下にあるすべてのプロパティ) を構成するすべてのファイルが追加されたことを確認、**コンテンツ**itemgroup vsix プロジェクトのファイルおよび各項目が含まれている、 **CopyToOutputDirectory**と**IncludeInVsix**メタデータが次の例に示すように設定します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-144">Make sure that all the files that comprise your project template (everything underneath the ProjectTemplates folder) are added to the **Content** itemgroup in the VSIX project file and that each item contains the **CopyToOutputDirectory** and **IncludeInVsix** metadata set as shown in the example below.</span></span>

<span data-ttu-id="fbe60-145">&lt;コンテンツを含める =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;</span><span class="sxs-lookup"><span data-stu-id="fbe60-145">&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;</span></span>

<span data-ttu-id="fbe60-146">&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;</span><span class="sxs-lookup"><span data-stu-id="fbe60-146">&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;</span></span>

<span data-ttu-id="fbe60-147">&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;</span><span class="sxs-lookup"><span data-stu-id="fbe60-147">&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;</span></span>

<span data-ttu-id="fbe60-148">&lt;/Content&gt;</span><span class="sxs-lookup"><span data-stu-id="fbe60-148">&lt;/Content&gt;</span></span>

<span data-ttu-id="fbe60-149">有効でない場合は、IDE は、VSIX をビルドすると、エラー表示される可能性が、テンプレートの内容をコンパイルしようとしています。</span><span class="sxs-lookup"><span data-stu-id="fbe60-149">If not, the IDE will try to compile the contents of the template when you build the VSIX and you will likely see an error.</span></span> <span data-ttu-id="fbe60-150">テンプレート内のコード ファイルは多くの場合、特別な含む[テンプレート パラメーター](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx)プロジェクト テンプレートがインスタンス化され、IDE でコンパイルすることはできません、Visual Studio で使用します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-150">Code files in templates often contain special [template parameters](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) used by Visual Studio when the project template is instantiated and therefore cannot be compiled in the IDE.</span></span>

![ソリューション エクスプローラー](custom-mvc-templates/_static/image6.jpg)

<span data-ttu-id="fbe60-152">VSIX デザイナーを閉じてから、右クリック、 **source.extension.manifest**ファイル**ソリューション エクスプ ローラー**選択**プログラムから開く**を選択し、 **XML (テキスト) エディター**オプション。</span><span class="sxs-lookup"><span data-stu-id="fbe60-152">Close the VSIX designer, then right click on the **source.extension.manifest** file in **Solution Explorer** and select **Open With** and choose the **XML (Text) Editor** option.</span></span>

![ダイアログを開く](custom-mvc-templates/_static/image7.jpg)

<span data-ttu-id="fbe60-154">作成、 **&lt;資産&gt;** 要素を追加し、 **&lt;資産&gt;** VSIX に含める必要がある各ファイルの要素。</span><span class="sxs-lookup"><span data-stu-id="fbe60-154">Create an **&lt;Assets&gt;** element and add an **&lt;Asset&gt;** element for each file that must be included in the VSIX.</span></span> <span data-ttu-id="fbe60-155">**型**の各属性**&lt;資産&gt;** に要素を設定する必要があります**Microsoft.VisualStudio.Mvc.Template**します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-155">The **Type** attribute of each **&lt;Asset&gt;** element must be set to **Microsoft.VisualStudio.Mvc.Template**.</span></span> <span data-ttu-id="fbe60-156">これは、MVC プロジェクトのウィザードのみを認識するカスタムの名前空間です。</span><span class="sxs-lookup"><span data-stu-id="fbe60-156">This is a custom namespace that only the MVC project wizard understands.</span></span> <span data-ttu-id="fbe60-157">構造とマニフェスト ファイルのレイアウトの詳細については、VSIX 2.0 スキーマ ドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fbe60-157">Refer to the VSIX 2.0 Schema documentation for additional information on the structure and layout of the manifest file.</span></span>

<span data-ttu-id="fbe60-158">ファイルを VSIX に追加するだけは MVC のウィザードを使用して、テンプレートを登録するだけで十分ではありません。</span><span class="sxs-lookup"><span data-stu-id="fbe60-158">Just adding the files to the VSIX is not sufficient to register the templates with the MVC wizard.</span></span> <span data-ttu-id="fbe60-159">MVC のウィザードをテンプレートの名前、説明、サポートされているビュー エンジンとプログラミング言語などの情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fbe60-159">You need to provide information such as the template name, description, supported view engines and programming language to the MVC wizard.</span></span> <span data-ttu-id="fbe60-160">関連付けられているカスタム属性にこの情報が含まれる、 **&lt;資産&gt;** 要素ごと**vstemplate**ファイル。</span><span class="sxs-lookup"><span data-stu-id="fbe60-160">This information is carried in custom attributes associated with the **&lt;Asset&gt;** element for each **vstemplate** file.</span></span>

<span data-ttu-id="fbe60-161">&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;</span><span class="sxs-lookup"><span data-stu-id="fbe60-161">&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;</span></span>

<span data-ttu-id="fbe60-162">Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;</span><span class="sxs-lookup"><span data-stu-id="fbe60-162">Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;</span></span>

<span data-ttu-id="fbe60-163">d:Source=&quot;File&quot;</span><span class="sxs-lookup"><span data-stu-id="fbe60-163">d:Source=&quot;File&quot;</span></span>

<span data-ttu-id="fbe60-164">Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;</span><span class="sxs-lookup"><span data-stu-id="fbe60-164">Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;</span></span>

<span data-ttu-id="fbe60-165">ProjectType=&quot;MVC&quot;</span><span class="sxs-lookup"><span data-stu-id="fbe60-165">ProjectType=&quot;MVC&quot;</span></span>

<span data-ttu-id="fbe60-166">言語 =&quot;(C#)&quot;</span><span class="sxs-lookup"><span data-stu-id="fbe60-166">Language=&quot;C#&quot;</span></span>

<span data-ttu-id="fbe60-167">ViewEngine=&quot;Aspx&quot;</span><span class="sxs-lookup"><span data-stu-id="fbe60-167">ViewEngine=&quot;Aspx&quot;</span></span>

<span data-ttu-id="fbe60-168">TemplateId=&quot;MyMvcApplication&quot;</span><span class="sxs-lookup"><span data-stu-id="fbe60-168">TemplateId=&quot;MyMvcApplication&quot;</span></span>

<span data-ttu-id="fbe60-169">タイトル =&quot;カスタムの基本的な Web アプリケーション&quot;</span><span class="sxs-lookup"><span data-stu-id="fbe60-169">Title=&quot;Custom Basic Web Application&quot;</span></span>

<span data-ttu-id="fbe60-170">説明 =&quot;基本的な MVC web アプリケーション (Razor) から派生したカスタム テンプレート&quot;</span><span class="sxs-lookup"><span data-stu-id="fbe60-170">Description=&quot;A custom template derived from a Basic MVC web application (Razor)&quot;</span></span>

<span data-ttu-id="fbe60-171">バージョン =&quot;4.0&quot;/&gt;</span><span class="sxs-lookup"><span data-stu-id="fbe60-171">Version=&quot;4.0&quot;/&gt;</span></span>

<span data-ttu-id="fbe60-172">次に必要なカスタム属性の説明に示します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-172">Below is an explanation of the custom attributes that must be present:</span></span>

- <span data-ttu-id="fbe60-173">**ProjectType** MVC に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fbe60-173">**ProjectType** must be set to MVC.</span></span>
- <span data-ttu-id="fbe60-174">**言語**テンプレートでサポートされている開発言語を指定します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-174">**Language** designates the development language supported by the template.</span></span> <span data-ttu-id="fbe60-175">有効な値は、c# または VB. です。</span><span class="sxs-lookup"><span data-stu-id="fbe60-175">Valid values are either C# or VB.</span></span>
- <span data-ttu-id="fbe60-176">**ViewEngine** Aspx など Razor テンプレートでサポートされているビュー エンジンを指定します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-176">**ViewEngine** designates the view engine supported by the template such as Aspx or Razor.</span></span> <span data-ttu-id="fbe60-177">このフィールドのカスタム値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="fbe60-177">You can specify a custom value for this field.</span></span>
- <span data-ttu-id="fbe60-178">**TemplateId**テンプレートをグループ化するために使用します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-178">**TemplateId** is used for grouping the templates.</span></span> <span data-ttu-id="fbe60-179">値は、既存のテンプレート ID が一致する場合は、MVC のウィザードを使用して以前に登録されているテンプレートをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="fbe60-179">If the value matches an existing template ID it will be override templates previously registered with the MVC wizard.</span></span>
- <span data-ttu-id="fbe60-180">**タイトル**各プロジェクト テンプレートの下に MVC ウィザードに表示される簡単な説明を指定します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-180">**Title** designates the short description displayed in the MVC wizard beneath each project template.</span></span>
- <span data-ttu-id="fbe60-181">**説明**テンプレートのより詳細な説明を指定します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-181">**Description** designates a more verbose description of the template.</span></span>

<span data-ttu-id="fbe60-182">マニフェストにすべてのファイルを追加すると、保存、表示になります、**資産**すべてのファイルをデザイナーのタブが表示されますが、カスタムではない属性に追加した、 **&lt;資産&gt;** の要素、 **vstemplate**ファイル。</span><span class="sxs-lookup"><span data-stu-id="fbe60-182">After you have added all the files to the manifest and saved it, you will notice that the **Assets** tab in the designer will display all the files, but not the custom attributes you added to the **&lt;Asset&gt;** elements for the **vstemplate** files.</span></span>

![プロジェクト デザイナーの資産](custom-mvc-templates/_static/image8.jpg)

<span data-ttu-id="fbe60-184">ここでは、VSIX プロジェクトをコンパイルしてインストールします。</span><span class="sxs-lookup"><span data-stu-id="fbe60-184">All that remains now is to compile the VSIX project and install it.</span></span>

<span data-ttu-id="fbe60-185">コンピューターの Visual Studio のすべてのインスタンスが閉じられていることを確認して VSIX 拡張機能をテストするを作成します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-185">Make sure that all instances of Visual Studio are closed on the machine where you intend to test the VSIX extension.</span></span> <span data-ttu-id="fbe60-186">Visual Studio は起動時に、新しい拡張機能のスキャンため、VSIX のインストール中に IDE が開いている場合は、Visual Studio を再起動する必要があります。 します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-186">Visual Studio scans for new extensions during startup, so if the IDE is open while installing a VSIX you will need to restart Visual Studio.</span></span> <span data-ttu-id="fbe60-187">エクスプ ローラーで、起動する VSIX ファイルをダブルクリックします。、 **VSIX インストーラー**、 をクリック**インストール**し、Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-187">In Explorer, double click on the VSIX file to launch the **VSIX Installer**, click **Install** and then launch Visual Studio.</span></span>

![VSIX インストーラー](custom-mvc-templates/_static/image9.jpg)

<span data-ttu-id="fbe60-189">メニューから、次のように選択します。**ツール > 拡張機能と更新**を、拡張機能がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-189">From the menu, select **Tools > Extensions and Updates** to confirm that your extension was installed.</span></span> <span data-ttu-id="fbe60-190">VSIX インストーラーには、拡張機能のインストール中にエラーが報告された場合は、VSIX インストーラーのログで詳細を表示できます。</span><span class="sxs-lookup"><span data-stu-id="fbe60-190">If the VSIX Installer reported any errors during the installation of the extension you can view the VSIX Installer log for more information.</span></span> <span data-ttu-id="fbe60-191">ログを作成して、通常、 **%temp%** など、拡張機能をインストールしたユーザーのフォルダー **C:\Users\Bob\AppData\Local\Temp**します。</span><span class="sxs-lookup"><span data-stu-id="fbe60-191">The log is usually created in the **%temp%** folder of the user that installed the extension, for example **C:\Users\Bob\AppData\Local\Temp**.</span></span>

![拡張機能と更新プログラム](custom-mvc-templates/_static/image10.jpg)

<span data-ttu-id="fbe60-193">ウィンドウを閉じた後に、MVC ウィザードで、新しいテンプレートが表示されているかどうかを MVC 4 プロジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="fbe60-193">After closing the window you can create an MVC 4 project to see whether your new templates are shown in the MVC wizard.</span></span>

![新しい ASP.NET MVC 4 プロジェクト](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a><span data-ttu-id="fbe60-195">制限事項</span><span class="sxs-lookup"><span data-stu-id="fbe60-195">Limitations</span></span>

1. <span data-ttu-id="fbe60-196">MVC のウィザードは、ローカライズされたカスタム テンプレートをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="fbe60-196">The MVC wizard does not support localized custom templates.</span></span>
2. <span data-ttu-id="fbe60-197">カスタム テンプレートを見つけることが失敗した場合、ウィザードはすべてのエラーを報告しません。</span><span class="sxs-lookup"><span data-stu-id="fbe60-197">The wizard will not report any errors if it fails to locate custom templates.</span></span> <span data-ttu-id="fbe60-198">存在しない場合、必要なカスタム属性のいずれかの場合は、テンプレート、単に、ウィザードから除外されます。</span><span class="sxs-lookup"><span data-stu-id="fbe60-198">If any of the required custom attributes are absent, the template would simply be excluded from the Wizard.</span></span>
