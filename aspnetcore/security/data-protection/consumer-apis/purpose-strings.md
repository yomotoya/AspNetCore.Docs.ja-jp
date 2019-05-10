---
title: ASP.NET Core での目的の文字列
author: rick-anderson
description: ASP.NET Core データ保護 Api で目的の文字列を使用する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087544"
---
# <a name="purpose-strings-in-aspnet-core"></a>ASP.NET Core での目的の文字列

<a name="data-protection-consumer-apis-purposes"></a>

コンポーネントを消費しない`IDataProtectionProvider`を一意に渡す必要があります*目的で*パラメーターを`CreateProtector`メソッド。 目的*パラメーター*は、データ保護システムのセキュリティを本質的なルートの暗号化キーが同じ場合でも、暗号化のコンシューマー間の分離を提供します。

コンシューマーは、目的を指定する場合、目的の文字列内のルートの暗号化キーと共にそのコンシューマーに一意の暗号化のサブキーを派生に使用されます。 これにより、アプリケーションで他のすべての暗号化サービス コンシューマーからコンシューマーを排除します。 その他のコンポーネントは、そのペイロードを読み取ることではありませんし、その他のコンポーネントのペイロードを読み取ることができません。 この分離には、コンポーネントに対する攻撃の実行不可能の全体カテゴリも表示します。

![目的の図の例](purpose-strings/_static/purposes.png)

上記の図で`IDataProtector`インスタンス A と B**ことはできません**ペイロードを読み取る他ののみ独自です。

秘密にする目的の文字列はありません。 適切に動作の他のコンポーネントがこれまでしないことを目的の文字列と同じ意味で一意である単純にする必要があります。

>[!TIP]
> 目安、実際にこの情報が競合しないようには、データ保護 Api を利用して、コンポーネントの名前空間と型名を使用します。
>
>Minting ベアラー トークンを担当する contoso 社製のコンポーネントは、その目的の文字列として Contoso.Security.BearerToken を使用する可能性があります。 -さらに良い点 - Contoso.Security.BearerToken.v1、目的の文字列として使用可能性があることもします。 Contoso.Security.BearerToken.v2 を目的として使用する将来のバージョンは、バージョン番号を追加し、さまざまなバージョン互いから完全に分離されたペイロードの移動しします。

目的のパラメーターから`CreateProtector`文字列配列では、上記でしたした代わりとして指定されている`[ "Contoso.Security.BearerToken", "v1" ]`します。 これにより、目的の階層を確立して、データ保護システムでのマルチ テナント シナリオの可能性を開きます。

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> コンポーネントは、目的のチェーンの入力の唯一のソースを信頼できないユーザー入力を使用することはできません。
>
>たとえば、コンポーネントは、セキュリティで保護されたメッセージを格納する責任を負います Contoso.Messaging.SecureMessage を検討してください。 セキュリティで保護されたメッセージング コンポーネントを呼び出す場合`CreateProtector([ username ])`、悪意のあるユーザーを呼び出すコンポーネントを取得するためにユーザー名"Contoso.Security.BearerToken"を持つアカウントを作成する可能性がありますし、 `CreateProtector([ "Contoso.Security.BearerToken" ])`、誤っているため、セキュリティで保護されたメッセージング認証トークンとして認識される可能性があります mint ペイロードするシステムです。
>
>メッセージング コンポーネントの向上のためチェーンになります`CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`、適切な分離を提供します。

によって提供される分離との動作`IDataProtectionProvider`、 `IDataProtector`、目的では、次のとおりとします。

* 指定された`IDataProtectionProvider`オブジェクト、`CreateProtector`メソッドは、作成、`IDataProtector`両方にオブジェクトが一意に関連付けられている、`IDataProtectionProvider`メソッドに渡された目的でパラメーターを作成するオブジェクト。

* 目的のパラメーターを null にするにはできません。 (場合は、配列としての目的を指定すると、つまり長さ 0 の配列がすることはできません、配列のすべての要素は null 以外である必要があります。)空の文字列の目的は、技術的には許可されていますをお勧めします。

* 2 つの目的の引数は、同じ順序では、(序数の比較子を使用して)、同じ文字列が含まれている場合にのみ等価です。 1 つの目的の引数は、対応する 1 つの要素の目的で配列と同じです。

* 2 つ`IDataProtector`相当から作成されている場合にのみ、オブジェクトが等しい`IDataProtectionProvider`オブジェクトと同じ目的でパラメーターを使用します。

* 指定された`IDataProtector`オブジェクトへの呼び出し`Unprotect(protectedData)`元に戻ります`unprotectedData`場合にのみ`protectedData := Protect(unprotectedData)`相当する`IDataProtector`オブジェクト。

> [!NOTE]
> いくつかのコンポーネントが意図的に別のコンポーネントと競合が知られている目的の文字列を選択、ケースを考慮していないことはできます。 このようなコンポーネントは本質的に悪意のあると見なさ悪意のあるコードがワーカー プロセス内で既に実行されていること、セキュリティ保証を提供するこのシステムはありません。
