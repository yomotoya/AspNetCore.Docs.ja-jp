---
title: ASP.NET core コンシューマー Api の概要
author: rick-anderson
description: さまざまなコンシューマーの ASP.NET Core データ保護のライブラリ内で使用可能な Api の概要が表示されます。
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: ff9badb55813cae0aa72d3a95dc53792332f109b
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837380"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>ASP.NET core コンシューマー Api の概要

`IDataProtectionProvider`と`IDataProtector`インターフェイスは、コンシューマーを使用するには、データ保護システムを使用して基本的なインターフェイスです。 ファイルに配置している、 [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/)パッケージ。

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

プロバイダーのインターフェイスは、データ保護システムのルートを表します。 保護またはデータを保護解除を直接使用できません。 代わりに、コンシューマーがへの参照を取得する必要があります、`IDataProtector`呼び出して`IDataProtectionProvider.CreateProtector(purpose)`目的は、目的のコンシューマーのユース ケースを説明する文字列です。 参照してください[目的文字列](xref:security/data-protection/consumer-apis/purpose-strings)のこのパラメーターは、適切な値を選択する方法の目的に関する詳細な情報。

## <a name="idataprotector"></a>IDataProtector

プロテクター インターフェイスがへの呼び出しによって返される`CreateProtector`とそのコンシューマーは、実行に使用できるこのインターフェイスは、保護し、操作の保護を解除します。

データの一部を保護するデータを渡す、`Protect`メソッド。 どの変換 byte[] byte]-> [メソッドを定義する基本的なインターフェイスがある文字列に変換する (拡張メソッドとして提供) オーバー ロードが文字列を -> もします。 2 つの方法で提供されるセキュリティは同じです。開発者は、どちらのオーバー ロードは、お客様のユース ケースに最適な選択する必要があります。 アプリケーションを使用すると、信頼されていないクライアントに送信できますおよび選択すると、オーバー ロードの保護によって返される値に関係なく (したり、改ざんされ使用可能なメソッドは保護ようになりました。

データの保護されていた一部の保護を解除する保護されたデータを渡す、`Unprotect`メソッド。 (Byte[] がある-開発者にとって利便性に基づいており、文字列ベースのオーバー ロードします)。保護されたペイロードは、事前に呼び出したによって生成された場合`Protect`この同じ`IDataProtector`、`Unprotect`メソッドは、元の保護されていないペイロードを返します。 保護されたペイロードが改ざんされたかどうか、または別で生成された`IDataProtector`、 `Unprotect` CryptographicException をメソッドがスローされます。

異なると同じ概念`IDataProtector`ties が目的の概念をバックアップします。 2 つの場合`IDataProtector`インスタンスが同じルートから生成された`IDataProtectionProvider`への呼び出しで異なる目的の文字列を使用したが、`IDataProtectionProvider.CreateProtector`がいると見なされます[異なるプロテクター](xref:security/data-protection/consumer-apis/purpose-strings)、して保護を解除する 1 つはできません他のによって生成されるペイロードです。

## <a name="consuming-these-interfaces"></a>これらのインターフェイスを使用

DI に対応したコンポーネントでは、使用目的は、コンポーネントが受け取る、`IDataProtectionProvider`コンス トラクターにパラメーターと、コンポーネントがインスタンス化されるとき、DI システムがそのこのサービスを自動的に提供します。

> [!NOTE]
> 一部のアプリケーション (コンソール アプリケーション、ASP.NET 4.x アプリケーションなど) があります DI に対応したため、ここで説明されているメカニズムを使用することはできません。 これらのシナリオを参照してください、[非 DI 対応シナリオ](xref:security/data-protection/configuration/non-di-scenarios)ドキュメントのインスタンスの取得の詳細については、 `IDataProtection` DI を経由せずプロバイダー。

次の例では、3 つの概念を示しています。

1. [データ保護システムを追加](xref:security/data-protection/configuration/overview)サービス コンテナーにする

2. インスタンスを受信する DI を使用して、`IDataProtectionProvider`と

3. 作成、`IDataProtector`から、`IDataProtectionProvider`保護およびデータの保護を解除するために使用するとします。

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Microsoft.AspNetCore.DataProtection.Abstractions パッケージには、拡張メソッドが含まれています。`IServiceProvider.GetDataProtector`開発者にとっての利便性として。 単一の操作として取得の両方をカプセル化が、`IDataProtectionProvider`サービス プロバイダーと呼び出しから`IDataProtectionProvider.CreateProtector`します。 次の例では、その使用法を示します。

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> インスタンス`IDataProtectionProvider`と`IDataProtector`は複数の呼び出し元のスレッド セーフです。 意図したものをコンポーネントへの参照を取得したら、`IDataProtector`呼び出しに`CreateProtector`、複数の呼び出しの参照が使用されます`Protect`と`Unprotect`します。 呼び出し`Unprotect`CryptographicException 保護されたペイロードの検証または解読できない場合がスローされます。 一部のコンポーネントがエラーを無視することが中に保護を解除します。認証 cookie を読み取るコンポーネント可能性がありますこのエラーを処理しがなかった cookie すべての場合と、要求を処理ではなくも、最初からの要求は失敗します。 この動作をするコンポーネントは、すべての例外の飲み込みではなく CryptographicException を明示的にキャッチする必要があります。
