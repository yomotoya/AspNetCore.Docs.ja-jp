---
uid: config-builder
title: ASP.NET の構成ビルダー
author: rick-anderson
description: 外部ソースから値を web.config 以外のソースから構成データを取得する方法について説明します。
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148838"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="b984c-103">ASP.NET の構成ビルダー</span><span class="sxs-lookup"><span data-stu-id="b984c-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="b984c-104">によって[Stephen Molloy](https://github.com/StephenMolloy)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b984c-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b984c-105">構成ビルダーは、外部ソースから構成値を取得する ASP.NET アプリに対して最新でアジャイルなメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="b984c-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="b984c-106">構成ビルダー:</span><span class="sxs-lookup"><span data-stu-id="b984c-106">Configuration builders:</span></span>

* <span data-ttu-id="b984c-107">.NET Framework 4.7.1 で使用可能な以降。</span><span class="sxs-lookup"><span data-stu-id="b984c-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="b984c-108">構成値を読み取るための柔軟なメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="b984c-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="b984c-109">コンテナーに移動し、フォーカスがある環境をクラウド アプリの基本的な要件の一部に対処します。</span><span class="sxs-lookup"><span data-stu-id="b984c-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="b984c-110">これまで使用できなかったソースから描画することで構成データ (たとえば、Azure Key Vault と環境変数) の保護を向上させるために、.NET 構成システムで使用できます。</span><span class="sxs-lookup"><span data-stu-id="b984c-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="b984c-111">キー/値の構成ビルダー</span><span class="sxs-lookup"><span data-stu-id="b984c-111">Key/value configuration builders</span></span>

<span data-ttu-id="b984c-112">構成ビルダーによって処理できる一般的なシナリオは、キー/値のパターンに準拠する構成セクションのキー/値の基本的な交換メカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="b984c-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="b984c-113">ConfigurationBuilders の .NET Framework の概念は、特定の構成のセクションでは、またはパターンに制限はありません。</span><span class="sxs-lookup"><span data-stu-id="b984c-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="b984c-114">ただし、多くの構成ビルダーの`Microsoft.Configuration.ConfigurationBuilders`([github](https://github.com/aspnet/MicrosoftConfigurationBuilders)、 [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) キー/値のパターン内で機能します。</span><span class="sxs-lookup"><span data-stu-id="b984c-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="b984c-115">キー/値の構成ビルダーの設定</span><span class="sxs-lookup"><span data-stu-id="b984c-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="b984c-116">次の設定ですべてのキー/値構成ビルダーを適用する`Microsoft.Configuration.ConfigurationBuilders`します。</span><span class="sxs-lookup"><span data-stu-id="b984c-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="b984c-117">モード</span><span class="sxs-lookup"><span data-stu-id="b984c-117">Mode</span></span>

<span data-ttu-id="b984c-118">構成ビルダーでは、外部ソースのキー/値の情報を使用して、構成システムの要素を選択したキー/値の設定。</span><span class="sxs-lookup"><span data-stu-id="b984c-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="b984c-119">具体的には、`<appSettings/>`と`<connectionStrings/>`のセクションでは、構成ビルダーから特別な処理を受信します。</span><span class="sxs-lookup"><span data-stu-id="b984c-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="b984c-120">作成者は、3 つのモードで動作します。</span><span class="sxs-lookup"><span data-stu-id="b984c-120">The builders work in three modes:</span></span>

* <span data-ttu-id="b984c-121">`Strict` 既定モードです。</span><span class="sxs-lookup"><span data-stu-id="b984c-121">`Strict` - The default mode.</span></span> <span data-ttu-id="b984c-122">このモードで構成ビルダーはよく知られているキー/値を中心とした構成セクションに対してのみ動作します。</span><span class="sxs-lookup"><span data-stu-id="b984c-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="b984c-123">`Strict` モードでは、セクションの各キーを列挙します。</span><span class="sxs-lookup"><span data-stu-id="b984c-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="b984c-124">一致する場合、外部ソース キーはあります。</span><span class="sxs-lookup"><span data-stu-id="b984c-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="b984c-125">構成ビルダーは、外部ソースからの値で結果として得られる構成セクションの値を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b984c-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="b984c-126">`Greedy` -このモードは密接に関連する`Strict`モード。</span><span class="sxs-lookup"><span data-stu-id="b984c-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="b984c-127">既にキーにとらわれることがなく、元の構成が存在します。</span><span class="sxs-lookup"><span data-stu-id="b984c-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="b984c-128">構成ビルダーは、結果として得られる構成セクションに、外部ソースからのすべてのキー/値ペアを追加します。</span><span class="sxs-lookup"><span data-stu-id="b984c-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="b984c-129">`Expand` -構成セクション オブジェクトに解析前に、生の XML では動作します。</span><span class="sxs-lookup"><span data-stu-id="b984c-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="b984c-130">文字列内のトークンの拡張と考えることができます。</span><span class="sxs-lookup"><span data-stu-id="b984c-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="b984c-131">パターンに一致する生の XML 文字列の一部`${token}`はトークンの拡張の候補です。</span><span class="sxs-lookup"><span data-stu-id="b984c-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="b984c-132">外部ソースに対応する値が見つからない場合、トークンは変更されません。</span><span class="sxs-lookup"><span data-stu-id="b984c-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="b984c-133">このモードでのビルダーがこれらに限定されません、`<appSettings/>`と`<connectionStrings/>`セクション。</span><span class="sxs-lookup"><span data-stu-id="b984c-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="b984c-134">次のマークアップから*web.config*により、 [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/)で`Strict`モード。</span><span class="sxs-lookup"><span data-stu-id="b984c-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="b984c-135">次のコードの読み取り、`<appSettings/>`と`<connectionStrings/>`前述*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b984c-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="b984c-136">上記のコードでは、プロパティ値に設定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="b984c-137">内の値、 *web.config*ファイル環境変数では、キーが設定されていない場合。</span><span class="sxs-lookup"><span data-stu-id="b984c-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="b984c-138">環境の値、変数の場合に設定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="b984c-139">たとえば、`ServiceID`が含まれます。</span><span class="sxs-lookup"><span data-stu-id="b984c-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="b984c-140">「Web.config ファイルからの ServiceID 値」場合は、環境変数`ServiceID`が設定されていません。</span><span class="sxs-lookup"><span data-stu-id="b984c-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="b984c-141">値、`ServiceID`環境変数の場合に設定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="b984c-142">次の図は、 `<appSettings/>` 、前のキー/値の*web.config*環境エディターでのファイル セット。</span><span class="sxs-lookup"><span data-stu-id="b984c-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![環境のエディター](config-builder/static/env.png)

<span data-ttu-id="b984c-144">注: 終了し、環境変数に変更を表示する Visual Studio を再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b984c-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="b984c-145">プレフィックスの処理</span><span class="sxs-lookup"><span data-stu-id="b984c-145">Prefix handling</span></span>

<span data-ttu-id="b984c-146">キーのプレフィックスは、ために、設定キーを簡略化できます。</span><span class="sxs-lookup"><span data-stu-id="b984c-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="b984c-147">.NET Framework 構成は、複雑で入れ子になったです。</span><span class="sxs-lookup"><span data-stu-id="b984c-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="b984c-148">外部キー/値のソースは、基本的なと本質的にフラットでは一般的です。</span><span class="sxs-lookup"><span data-stu-id="b984c-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="b984c-149">たとえば、環境変数は入れ子にできません。</span><span class="sxs-lookup"><span data-stu-id="b984c-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="b984c-150">次の方法のいずれかを使用して、両方を挿入する`<appSettings/>`と`<connectionStrings/>`環境変数を使用して、構成に。</span><span class="sxs-lookup"><span data-stu-id="b984c-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="b984c-151">`EnvironmentConfigBuilder`既定`Strict`モードと、構成ファイル内の適切なキー名。</span><span class="sxs-lookup"><span data-stu-id="b984c-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="b984c-152">上記のコードとマークアップは、このアプローチを採用します。</span><span class="sxs-lookup"><span data-stu-id="b984c-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="b984c-153">できます、このアプローチを使用して**いない**両方のキーが同じ名前の付いた`<appSettings/>`と`<connectionStrings/>`します。</span><span class="sxs-lookup"><span data-stu-id="b984c-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="b984c-154">2 つを使用して、`EnvironmentConfigBuilder`内`Greedy`異なるプレフィックスを持つモードと`stripPrefix`します。</span><span class="sxs-lookup"><span data-stu-id="b984c-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="b984c-155">この方法で、アプリが読み取ることができます`<appSettings/>`と`<connectionStrings/>`構成ファイルを更新する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b984c-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="b984c-156">次のセクションでは、 [stripPrefix](#stripprefix)、これを行う方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="b984c-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="b984c-157">2 つを使用して、`EnvironmentConfigBuilder`内`Greedy`異なるプレフィックスを持つモード。</span><span class="sxs-lookup"><span data-stu-id="b984c-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="b984c-158">この方法でキー名を変更する必要がありますプレフィックスとキー名が重複することはできません。</span><span class="sxs-lookup"><span data-stu-id="b984c-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="b984c-159">例えば:</span><span class="sxs-lookup"><span data-stu-id="b984c-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="b984c-160">上記のマークアップでは、2 つの異なるセクションの構成を設定する同じフラットなキー/値のソースを使用できます。</span><span class="sxs-lookup"><span data-stu-id="b984c-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="b984c-161">次の図は、`<appSettings/>`と`<connectionStrings/>`、前のキー/値の*web.config*環境エディターでのファイル セット。</span><span class="sxs-lookup"><span data-stu-id="b984c-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![環境のエディター](config-builder/static/prefix.png)

<span data-ttu-id="b984c-163">次のコードの読み取り、`<appSettings/>`と`<connectionStrings/>`キー/値の前に含まれている*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b984c-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="b984c-164">上記のコードでは、プロパティ値に設定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="b984c-165">内の値、 *web.config*ファイル環境変数では、キーが設定されていない場合。</span><span class="sxs-lookup"><span data-stu-id="b984c-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="b984c-166">環境の値、変数の場合に設定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="b984c-167">たとえば、前を使用して*web.config*ファイル、前の環境のエディターの画像および前のコードは、次の値のキー/値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="b984c-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="b984c-168">キー</span><span class="sxs-lookup"><span data-stu-id="b984c-168">Key</span></span>              | <span data-ttu-id="b984c-169">[値]</span><span class="sxs-lookup"><span data-stu-id="b984c-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="b984c-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="b984c-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="b984c-171">環境変数から AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="b984c-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="b984c-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="b984c-172">AppSetting_default</span></span>            | <span data-ttu-id="b984c-173">Env から AppSetting_default 値</span><span class="sxs-lookup"><span data-stu-id="b984c-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="b984c-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="b984c-174">ConnStr_default</span></span>         | <span data-ttu-id="b984c-175">Env から ConnStr_default val</span><span class="sxs-lookup"><span data-stu-id="b984c-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="b984c-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="b984c-176">stripPrefix</span></span>

<span data-ttu-id="b984c-177">`stripPrefix`: ブール値、の既定値は`false`します。</span><span class="sxs-lookup"><span data-stu-id="b984c-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="b984c-178">上記の XML マークアップは、アプリ設定接続文字列からを区切る、内のすべてのキーが必要です、 *web.config*指定したプレフィックスを使用するファイル。</span><span class="sxs-lookup"><span data-stu-id="b984c-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="b984c-179">たとえば、プレフィックス`AppSetting`に追加する必要があります、`ServiceID`キー ("AppSetting_ServiceID")。</span><span class="sxs-lookup"><span data-stu-id="b984c-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="b984c-180">`stripPrefix`で、プレフィックスが使用されていない、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b984c-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="b984c-181">(たとえば、環境です。) で構成ビルダーのソースでプレフィックスが必要です。ほとんどの開発者が使用予定`stripPrefix`します。</span><span class="sxs-lookup"><span data-stu-id="b984c-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="b984c-182">アプリケーションは、通常、プレフィックスを削除します。</span><span class="sxs-lookup"><span data-stu-id="b984c-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="b984c-183">次*web.config*プレフィックスを削除します。</span><span class="sxs-lookup"><span data-stu-id="b984c-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="b984c-184">上記の「 *web.config*ファイル、`default`キー両方では、`<appSettings/>`と`<connectionStrings/>`。</span><span class="sxs-lookup"><span data-stu-id="b984c-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="b984c-185">次の図は、`<appSettings/>`と`<connectionStrings/>`、前のキー/値の*web.config*環境エディターでのファイル セット。</span><span class="sxs-lookup"><span data-stu-id="b984c-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![環境のエディター](config-builder/static/prefix.png)

<span data-ttu-id="b984c-187">次のコードの読み取り、`<appSettings/>`と`<connectionStrings/>`キー/値の前に含まれている*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b984c-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="b984c-188">上記のコードでは、プロパティ値に設定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="b984c-189">内の値、 *web.config*ファイル環境変数では、キーが設定されていない場合。</span><span class="sxs-lookup"><span data-stu-id="b984c-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="b984c-190">環境の値、変数の場合に設定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="b984c-191">たとえば、前を使用して*web.config*ファイル、前の環境のエディターの画像および前のコードは、次の値のキー/値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="b984c-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="b984c-192">キー</span><span class="sxs-lookup"><span data-stu-id="b984c-192">Key</span></span>              | <span data-ttu-id="b984c-193">[値]</span><span class="sxs-lookup"><span data-stu-id="b984c-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="b984c-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="b984c-194">ServiceID</span></span>           | <span data-ttu-id="b984c-195">環境変数から AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="b984c-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="b984c-196">default</span><span class="sxs-lookup"><span data-stu-id="b984c-196">default</span></span>            | <span data-ttu-id="b984c-197">Env から AppSetting_default 値</span><span class="sxs-lookup"><span data-stu-id="b984c-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="b984c-198">default</span><span class="sxs-lookup"><span data-stu-id="b984c-198">default</span></span>         | <span data-ttu-id="b984c-199">Env から ConnStr_default val</span><span class="sxs-lookup"><span data-stu-id="b984c-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="b984c-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="b984c-200">tokenPattern</span></span>

<span data-ttu-id="b984c-201">`tokenPattern`: 文字列、既定値は `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="b984c-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="b984c-202">`Expand` 、ビルダーの動作は、生の XML のようなトークンを検索`${token}`します。</span><span class="sxs-lookup"><span data-stu-id="b984c-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="b984c-203">既定の正規表現では検索`@"\$\{(\w+)\}"`します。</span><span class="sxs-lookup"><span data-stu-id="b984c-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="b984c-204">一致する文字セット`\w`が XML と多くの構成ソースを使用するよりも厳格です。</span><span class="sxs-lookup"><span data-stu-id="b984c-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="b984c-205">使用`tokenPattern`よりも文字以上と`@"\$\{(\w+)\}"`トークン名が必要です。</span><span class="sxs-lookup"><span data-stu-id="b984c-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="b984c-206">`tokenPattern`: 文字列:</span><span class="sxs-lookup"><span data-stu-id="b984c-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="b984c-207">開発者は、トークンの照合に使用される正規表現を変更できます。</span><span class="sxs-lookup"><span data-stu-id="b984c-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="b984c-208">適切な形式で、危険で非正規表現がかどうかを確認する検証は行われません。</span><span class="sxs-lookup"><span data-stu-id="b984c-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="b984c-209">キャプチャ グループを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="b984c-209">It must contain a capture group.</span></span> <span data-ttu-id="b984c-210">全体の正規表現は、全体のトークンと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b984c-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="b984c-211">最初のキャプチャは、構成ソースを検索するため、トークンの名前である必要があります。</span><span class="sxs-lookup"><span data-stu-id="b984c-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="b984c-212">構成ビルダー Microsoft.Configuration.ConfigurationBuilders で</span><span class="sxs-lookup"><span data-stu-id="b984c-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="b984c-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b984c-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="b984c-214">[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="b984c-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="b984c-215">構成ビルダーの最も簡単です。</span><span class="sxs-lookup"><span data-stu-id="b984c-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="b984c-216">環境から値を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="b984c-216">Reads values from the environment.</span></span>
* <span data-ttu-id="b984c-217">追加の構成オプションはありません。</span><span class="sxs-lookup"><span data-stu-id="b984c-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="b984c-218">`name`属性の値は任意です。</span><span class="sxs-lookup"><span data-stu-id="b984c-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="b984c-219">**注:** Windows コンテナー環境で、実行時に設定された変数は、エントリ ポイント プロセスの環境に挿入のみされます。</span><span class="sxs-lookup"><span data-stu-id="b984c-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="b984c-220">サービスとして実行するアプリまたは非エントリ ポイント プロセスでは、それ以外の場合、コンテナー内のメカニズムを通じてを注入がない限り、これらの変数を選択しないでください。</span><span class="sxs-lookup"><span data-stu-id="b984c-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="b984c-221">[IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-コンテナーの現在のバージョンに基づく[ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41)でこれを処理、 *DefaultAppPool*のみです。</span><span class="sxs-lookup"><span data-stu-id="b984c-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="b984c-222">その他の Windows ベースのコンテナーのバリアントは、EntryPoint 以外のプロセスの独自性の注入メカニズムを開発する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b984c-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="b984c-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b984c-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="b984c-224">決してのパスワードを保存、機密性の高い接続文字列、またはソース コード内の他の機密データにします。</span><span class="sxs-lookup"><span data-stu-id="b984c-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="b984c-225">運用シークレットを使用する必要があります、開発やテストします。</span><span class="sxs-lookup"><span data-stu-id="b984c-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="b984c-226">この構成ビルダーのような機能を提供する[ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets)します。</span><span class="sxs-lookup"><span data-stu-id="b984c-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="b984c-227">[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) .NET Framework プロジェクトで使用できますが、シークレット ファイルを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b984c-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="b984c-228">代わりに、定義、`UserSecretsId`プロジェクト内のプロパティ ファイルを開き、正しい位置を読み取り用に、生のシークレット ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="b984c-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="b984c-229">プロジェクトから外部の依存関係を保持するには、シークレットのファイルは XML 形式です。</span><span class="sxs-lookup"><span data-stu-id="b984c-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="b984c-230">実装の詳細は、XML の書式設定して、形式に依存する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b984c-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="b984c-231">共有する必要がある場合、 *secrets.json* .NET Core プロジェクトでファイルを使用を検討して、 [SimpleJsonConfigBuilder](#simplejsonconfig)します。</span><span class="sxs-lookup"><span data-stu-id="b984c-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfig).</span></span> <span data-ttu-id="b984c-232">`SimpleJsonConfigBuilder` .NET Core 見なす必要がありますも変更される可能性の実装の詳細の書式を設定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="b984c-233">構成属性を`UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="b984c-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="b984c-234">`userSecretsId` -これは、XML のシークレット ファイルを識別するための推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="b984c-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="b984c-235">使用して .NET Core のような動作、`UserSecretsId`プロジェクトのプロパティをこの識別子を格納します。</span><span class="sxs-lookup"><span data-stu-id="b984c-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="b984c-236">文字列は一意である必要があります、GUID を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b984c-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="b984c-237">この属性で、`UserSecretsConfigBuilder`によく知られているローカルの場所を検索 (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) にこの識別子に属しているシークレット ファイル。</span><span class="sxs-lookup"><span data-stu-id="b984c-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="b984c-238">`userSecretsFile` -省略可能な属性は、シークレットを格納するファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="b984c-239">`~`アプリケーション ルートへの参照を先頭に文字を使用できます。</span><span class="sxs-lookup"><span data-stu-id="b984c-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="b984c-240">このいずれかの属性または`userSecretsId`属性が必要です。</span><span class="sxs-lookup"><span data-stu-id="b984c-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="b984c-241">両方が指定されて場合`userSecretsFile`が優先されます。</span><span class="sxs-lookup"><span data-stu-id="b984c-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="b984c-242">`optional`: ブール値、既定値`true`-シークレット ファイルが見つからない場合は、例外を防止します。</span><span class="sxs-lookup"><span data-stu-id="b984c-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="b984c-243">`name`属性の値は任意です。</span><span class="sxs-lookup"><span data-stu-id="b984c-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="b984c-244">シークレット ファイルには、次の形式があります。</span><span class="sxs-lookup"><span data-stu-id="b984c-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="b984c-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b984c-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="b984c-246">[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/)に格納されている値を読み取り、 [Azure Key Vault](/azure/key-vault/key-vault-whatis)します。</span><span class="sxs-lookup"><span data-stu-id="b984c-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="b984c-247">`vaultName` (資格情報コンテナーの名前)、または資格情報コンテナーへの URI のいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="b984c-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="b984c-248">その他の属性に接続するには、どのコンテナーに関する制御を許可するで動作する環境でアプリケーションが実行されていない場合のみ必要なの`Microsoft.Azure.Services.AppAuthentication`します。</span><span class="sxs-lookup"><span data-stu-id="b984c-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="b984c-249">Azure サービスの認証ライブラリは、自動的に可能であれば、実行環境からの接続情報を選択に使用されます。</span><span class="sxs-lookup"><span data-stu-id="b984c-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="b984c-250">自動的にオーバーライドする接続文字列を提供することで接続情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="b984c-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="b984c-251">`vaultName` -必要な場合`uri`で指定されていません。</span><span class="sxs-lookup"><span data-stu-id="b984c-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="b984c-252">キー/値ペアの読み取り元となる Azure サブスクリプションでは、コンテナーの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="b984c-253">`connectionString` 使用可能な接続文字列は- [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="b984c-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="b984c-254">`uri` -指定した場合は、その他の Key Vault プロバイダーに接続する`uri`値。</span><span class="sxs-lookup"><span data-stu-id="b984c-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="b984c-255">Azure を指定しない場合 (`vaultName`) は、資格情報コンテナーのプロバイダーです。</span><span class="sxs-lookup"><span data-stu-id="b984c-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="b984c-256">`version` -Azure Key Vault はシークレットのバージョン管理機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="b984c-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="b984c-257">場合`version`を指定すると、ビルダーでは、このバージョンに一致するシークレットのみを取得します。</span><span class="sxs-lookup"><span data-stu-id="b984c-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="b984c-258">`preloadSecretNames` -既定では、このビルダー クエリ**すべて**が初期化されるときに、key vault 名をキーします。</span><span class="sxs-lookup"><span data-stu-id="b984c-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="b984c-259">すべてのキー値の読み取りを防ぐためには、この属性を設定`false`します。</span><span class="sxs-lookup"><span data-stu-id="b984c-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="b984c-260">これを設定`false`一度に 1 つのシークレットを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="b984c-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="b984c-261">シークレットの便利な場合は、コンテナーは、"Get"アクセスが、"List"にアクセスを一度に 1 つできますを読み取っています。</span><span class="sxs-lookup"><span data-stu-id="b984c-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="b984c-262">**注:** を使用する場合`Greedy`モード、`preloadSecretNames`あります`true`(既定値です)。</span><span class="sxs-lookup"><span data-stu-id="b984c-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="b984c-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b984c-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="b984c-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/)はディレクトリのファイルの値のソースとして使用する基本的な構成ビルダー。</span><span class="sxs-lookup"><span data-stu-id="b984c-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="b984c-265">ファイルの名前は、キーと、コンテンツが値。</span><span class="sxs-lookup"><span data-stu-id="b984c-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="b984c-266">この構成ビルダーは、調整されたコンテナー環境で実行するときに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="b984c-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="b984c-267">Docker Swarm と Kubernetes を提供するようにシステム`secrets`キー ファイルごとにこの方法では、その調整された windows コンテナーにします。</span><span class="sxs-lookup"><span data-stu-id="b984c-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="b984c-268">属性の詳細:</span><span class="sxs-lookup"><span data-stu-id="b984c-268">Attribute details:</span></span>

* <span data-ttu-id="b984c-269">`directoryPath` - 必須。</span><span class="sxs-lookup"><span data-stu-id="b984c-269">`directoryPath` - Required.</span></span> <span data-ttu-id="b984c-270">値を検索するパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="b984c-271">機密情報が格納されている Windows の docker、 *C:\ProgramData\Docker\secrets*既定ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="b984c-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="b984c-272">`ignorePrefix` -このプレフィックスで始まるファイルが除外されます。</span><span class="sxs-lookup"><span data-stu-id="b984c-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="b984c-273">既定値"ignore"です。</span><span class="sxs-lookup"><span data-stu-id="b984c-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="b984c-274">`keyDelimiter` 既定値は`null`します。</span><span class="sxs-lookup"><span data-stu-id="b984c-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="b984c-275">場合は、指定された構成ビルダーは、この区切り記号でキー名を構築、ディレクトリの複数のレベルを走査します。</span><span class="sxs-lookup"><span data-stu-id="b984c-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="b984c-276">この値が場合`null`ディレクトリの最上位レベルだけ構成ビルダーを検索します。</span><span class="sxs-lookup"><span data-stu-id="b984c-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="b984c-277">`optional` 既定値は`false`します。</span><span class="sxs-lookup"><span data-stu-id="b984c-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="b984c-278">ソース ディレクトリが存在しない場合に、構成ビルダーがエラーを発生するかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="b984c-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b984c-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="b984c-280">決してのパスワードを保存、機密性の高い接続文字列、またはソース コード内の他の機密データにします。</span><span class="sxs-lookup"><span data-stu-id="b984c-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="b984c-281">運用シークレットを使用する必要があります、開発やテストします。</span><span class="sxs-lookup"><span data-stu-id="b984c-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="b984c-282">.NET core プロジェクトは、構成の JSON ファイルを頻繁に使用します。</span><span class="sxs-lookup"><span data-stu-id="b984c-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="b984c-283">[SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/)ビルダーは、.NET Framework で使用する .NET Core の JSON ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="b984c-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="b984c-284">この構成ビルダーは、.NET Framework の構成の特定のキー/値の領域にフラットなキー/値のソースからの基本的なマッピングを提供します。</span><span class="sxs-lookup"><span data-stu-id="b984c-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="b984c-285">この構成ビルダーが**いない**の階層の構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="b984c-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="b984c-286">JSON ファイルは、複雑な階層オブジェクトではなく、ディクショナリに似ています。</span><span class="sxs-lookup"><span data-stu-id="b984c-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="b984c-287">複数レベルの階層的なファイルを使用できます。</span><span class="sxs-lookup"><span data-stu-id="b984c-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="b984c-288">このプロバイダー `flatten`s の各レベルを使用してプロパティ名を追加して深さ`:`区切り文字として。</span><span class="sxs-lookup"><span data-stu-id="b984c-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="b984c-289">属性の詳細:</span><span class="sxs-lookup"><span data-stu-id="b984c-289">Attribute details:</span></span>

* <span data-ttu-id="b984c-290">`jsonFile` - 必須。</span><span class="sxs-lookup"><span data-stu-id="b984c-290">`jsonFile` - Required.</span></span> <span data-ttu-id="b984c-291">読み取りに JSON ファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="b984c-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="b984c-292">`~`アプリのルートを参照する先頭の文字を使用できます。</span><span class="sxs-lookup"><span data-stu-id="b984c-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="b984c-293">`optional` -ブール値、既定値は`true`します。</span><span class="sxs-lookup"><span data-stu-id="b984c-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="b984c-294">により、JSON ファイルが見つからない場合は、例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="b984c-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="b984c-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="b984c-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="b984c-296">`Flat` が既定値です。</span><span class="sxs-lookup"><span data-stu-id="b984c-296">`Flat` is the default.</span></span> <span data-ttu-id="b984c-297">ときに`jsonMode`は`Flat`、JSON ファイルが 1 つのフラットなキー/値のソース。</span><span class="sxs-lookup"><span data-stu-id="b984c-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="b984c-298">`EnvironmentConfigBuilder`と`AzureKeyVaultConfigBuilder`も、1 つのフラットなキー/値のソース。</span><span class="sxs-lookup"><span data-stu-id="b984c-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="b984c-299">ときに、`SimpleJsonConfigBuilder`で構成されて`Sectional`モード。</span><span class="sxs-lookup"><span data-stu-id="b984c-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="b984c-300">JSON ファイルは概念的には、最上位レベルだけで複数のディクショナリに分かれています。</span><span class="sxs-lookup"><span data-stu-id="b984c-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="b984c-301">各ディクショナリは、割り当てられている最上位のプロパティ名と一致する構成セクションにのみ適用します。</span><span class="sxs-lookup"><span data-stu-id="b984c-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="b984c-302">例えば:</span><span class="sxs-lookup"><span data-stu-id="b984c-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="b984c-303">キー/値のカスタム構成ビルダーを実装します。</span><span class="sxs-lookup"><span data-stu-id="b984c-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="b984c-304">構成ビルダーが目的に合わない場合は、カスタムの 1 つを記述できます。</span><span class="sxs-lookup"><span data-stu-id="b984c-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="b984c-305">`KeyValueConfigBuilder`基底クラスが置換モードとプレフィックスのほとんどの問題を処理します。</span><span class="sxs-lookup"><span data-stu-id="b984c-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="b984c-306">実装するプロジェクトのみが必要です。</span><span class="sxs-lookup"><span data-stu-id="b984c-306">An implementing project need only:</span></span>

* <span data-ttu-id="b984c-307">基本クラスから継承し、実装をキー/値ペアの基本的なソース、`GetValue`と`GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="b984c-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="b984c-308">追加、 [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/)をプロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="b984c-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="b984c-309">`KeyValueConfigBuilder`基本クラスには、職場で一貫性のある動作の多くキー/値の間で構成ビルダー。</span><span class="sxs-lookup"><span data-stu-id="b984c-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b984c-310">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="b984c-310">Additional resources</span></span>

* [<span data-ttu-id="b984c-311">構成ビルダーの GitHub リポジトリ</span><span class="sxs-lookup"><span data-stu-id="b984c-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="b984c-312">.NET を使用して Azure Key Vault へのサービス間認証</span><span class="sxs-lookup"><span data-stu-id="b984c-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
