---
title: ASP.NET Core でのキー管理の拡張性
author: rick-anderson
description: ASP.NET Core データ保護キー管理の拡張性について説明します。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896909"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>ASP.NET Core でのキー管理の拡張性

> [!TIP]
> 読み取り、[キー管理](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)これらの Api の背後にある基本的な概念の一部について説明します、このセクションを読む前にセクション。

> [!WARNING]
> 次のインターフェイスのいずれかを実装する型がスレッド セーフにする必要があります複数の呼び出し元の。

## <a name="key"></a>キー

`IKey`インターフェイスは基本的なキー暗号方式で表したものです。 用語のキーは、「暗号化キー マテリアル」の文字どおりの意味ではなく、抽象という意味では、ここに使用されます。 キーには、次のプロパティがあります。

* アクティブ化、作成、および有効期限の日付

* 失効状態

* キー識別子 (GUID)

::: moniker range=">= aspnetcore-2.0"

さらに、`IKey`公開、`CreateEncryptor`メソッドを作成するために使用できる、 [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンスは、このキーに関連付けられています。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

さらに、`IKey`公開、`CreateEncryptorInstance`メソッドを作成するために使用できる、 [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンスは、このキーに関連付けられています。

::: moniker-end

> [!NOTE]
> 生の暗号化マテリアルを取得する API はありません、`IKey`インスタンス。

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager`インターフェイスは、一般的なキーの格納、取得、および操作を担当するオブジェクトを表します。 次の 3 つの高度な操作が公開しています。

* 新しいキーを作成し、ストレージに保存します。

* 記憶域からすべてのキーを取得します。

* 1 つまたは複数のキーを取り消すし、記憶域に失効情報を保持します。

>[!WARNING]
> 書き込み、`IKeyManager`非常に高度なタスクは、開発者の大部分が試行しないでください。 代わりに、ほとんどの開発者がによって提供される機能の利点を実行する必要があります、 [XmlKeyManager](#xmlkeymanager)クラス。

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager`型がボックス内の具象実装の`IKeyManager`します。 キー エスクローおよび保存時のキーの暗号化を含むいくつかの便利な機能を提供します。 このシステム内のキーは XML 要素として表されます (具体的には、 [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview))。

`XmlKeyManager` そのタスクを実行する過程でその他のいくつかのコンポーネントに依存します。

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`を新しいキーで使用されるアルゴリズムを決定します。

* `IXmlRepository`、ストレージ キーが永続化するコントロール。

* `IXmlEncryptor` [オプション]、保存時のキーを暗号化することができます。

* `IKeyEscrowSink` [オプション]、キー エスクローのサービスを提供します。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`、ストレージ キーが永続化するコントロール。

* `IXmlEncryptor` [オプション]、保存時のキーを暗号化することができます。

* `IKeyEscrowSink` [オプション]、キー エスクローのサービスを提供します。

::: moniker-end

内でこれらのコンポーネントをまとめてワイヤードは方法を示す高度な図を次に示します`XmlKeyManager`します。

::: moniker range=">= aspnetcore-2.0"

![キーの作成](key-management/_static/keycreation2.png)

*キーの作成/CreateNewKey*

実装で`CreateNewKey`、`AlgorithmConfiguration`コンポーネントの作成に使用する一意`IAuthenticatedEncryptorDescriptor`が XML としてシリアル化し、先。 キー エスクロー シンクが存在する場合は、長期的なストレージ、未処理の XML を (暗号化されていない) が、シンクに提供されます。 暗号化されていない XML 実行し、 `IXmlEncryptor` (必要な) 場合、暗号化された XML ドキュメントを生成します。 この暗号化されたドキュメントが使用して長期的なストレージに永続化、`IXmlRepository`します。 (ない場合は`IXmlEncryptor`が構成されている場合、暗号化されていないドキュメントに保存、 `IXmlRepository`)。

![キーの取得](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![キーの作成](key-management/_static/keycreation1.png)

*キーの作成/CreateNewKey*

実装で`CreateNewKey`、`IAuthenticatedEncryptorConfiguration`コンポーネントの作成に使用する一意`IAuthenticatedEncryptorDescriptor`が XML としてシリアル化し、先。 キー エスクロー シンクが存在する場合は、長期的なストレージ、未処理の XML を (暗号化されていない) が、シンクに提供されます。 暗号化されていない XML 実行し、 `IXmlEncryptor` (必要な) 場合、暗号化された XML ドキュメントを生成します。 この暗号化されたドキュメントが使用して長期的なストレージに永続化、`IXmlRepository`します。 (ない場合は`IXmlEncryptor`が構成されている場合、暗号化されていないドキュメントに保存、 `IXmlRepository`)。

![キーの取得](key-management/_static/keyretrieval1.png)

::: moniker-end

*キーの取得/GetAllKeys*

実装で`GetAllKeys`、XML ドキュメントのキーを表すおよび失効は、基になるから読み取られる`IXmlRepository`します。 これらのドキュメントが暗号化されている場合、システムは自動的に復号化します。 `XmlKeyManager` 適切な作成`IAuthenticatedEncryptorDescriptorDeserializer`にドキュメントを逆シリアル化するインスタンスが再度`IAuthenticatedEncryptorDescriptor`インスタンスで、個々 にラップし、`IKey`インスタンス。 このコレクションの`IKey`のインスタンスが、呼び出し元に返されます。

特定の XML 要素の詳細についてで参照できる、[キー記憶域形式のドキュメント](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format)します。

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository`インターフェイスは、XML を永続化および XML をバッキング ストアから取得できる型を表します。 2 つの Api が公開しています。

* `GetAllElements` :`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

実装`IXmlRepository`通過して XML を解析する必要はありません。 上位のレイヤーを生成して、ドキュメントの解析について心配を不透明として XML ドキュメントを処理させている必要があります。

実装している 4 つの組み込み具象型がある`IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

参照してください、[キー記憶域プロバイダーのドキュメント](xref:security/data-protection/implementation/key-storage-providers)詳細についてはします。

カスタムの登録`IXmlRepository`さまざまなバッキング ストア (たとえば、Azure テーブル ストレージ) を使用する場合に適しています。

アプリケーション全体の既定のリポジトリを変更するには、登録、カスタム`IXmlRepository`インスタンス。

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
```

::: moniker-end

## <a name="ixmlencryptor"></a>IXmlEncryptor

`IXmlEncryptor`インターフェイスは、プレーン テキストの XML 要素を暗号化する型を表します。 1 つの API を公開しています。

* Encrypt(XElement plaintextElement) :EncryptedXmlInfo

場合、シリアル化された`IAuthenticatedEncryptorDescriptor`し、「暗号化を必要とする」としてマークされているすべての要素が含まれています`XmlKeyManager`経由、構成されたこれらの要素で実行される`IXmlEncryptor`の`Encrypt`メソッドをおよびそれにしたり、要素を保持ではなく、プレーン テキスト要素を`IXmlRepository`します。 出力、`Encrypt`メソッドは、`EncryptedXmlInfo`オブジェクト。 このオブジェクトはしたり、両方の結果を含むラッパー`XElement`と型を表す、`IXmlDecryptor`の対応する要素を復号化に使用できます。

実装している 4 つの組み込み具象型がある`IXmlEncryptor`:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

参照してください、 [rest のドキュメントでキーの暗号化](xref:security/data-protection/implementation/key-encryption-at-rest)詳細についてはします。

既定の保存にキーの暗号化メカニズム アプリケーション全体を変更するに登録するカスタム`IXmlEncryptor`インスタンス。

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
```

::: moniker-end

## <a name="ixmldecryptor"></a>IXmlDecryptor

`IXmlDecryptor`インターフェイスは、暗号化を解除する方法を認識する型を表す、`XElement`がしたりを使用して、`IXmlEncryptor`します。 1 つの API を公開しています。

* (XElement encryptedElement) の暗号化を解除します。XElement

`Decrypt`メソッドによって実行された暗号化を元に戻します`IXmlEncryptor.Encrypt`します。 一般に、各具象`IXmlEncryptor`対応する具体的な実装が必要があります`IXmlDecryptor`実装します。

実装する型`IXmlDecryptor`次の 2 つのパブリック コンストラクターのいずれかである必要があります。

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider`に渡されるコンストラクターを null にすることがあります。

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink`インターフェイスは、機密情報のエスクローを実行できる型を表します。 シリアル化された記述子は、(暗号化マテリアルの場合) などの機密情報を含む可能性があります、これは、導入に至った経緯を思い出してください、 [IXmlEncryptor](#ixmlencryptor)最初に入力します。 ただし、事故が発生して、キー リングが削除できるは、または破損します。

エスクロー インターフェイスを使用する、いずれかの構成によって変換される前に、生のシリアル化された XML へのアクセスを許可、緊急のエスケープ ハッチ[IXmlEncryptor](#ixmlencryptor)します。 インターフェイスは、1 つの API を公開します。

* ストア (Guid keyId、XElement 要素)

`IKeyEscrowSink`ビジネス ポリシーを使用して一貫性のある安全な方法で、指定した要素を処理するために実装します。 場所は、証明書の秘密キーはエスクローされました。 既知の企業 X.509 証明書を使用して XML 要素を暗号化するエスクロー シンクの 1 つの考えられる実装可能性があります。`CertificateXmlEncryptor`型がこれを支援するためです。 `IKeyEscrowSink`実装は、指定した要素を適切に永続化することもできます。

既定ではエスクロー メカニズムが有効でない、サーバー管理者は、[これをグローバルに構成](xref:security/data-protection/configuration/machine-wide-policy)します。 構成することもプログラムを使用して、`IDataProtectionBuilder.AddKeyEscrowSink`メソッドを次の例で示すようにします。 `AddKeyEscrowSink`メソッドのオーバー ロードのミラー、`IServiceCollection.AddSingleton`と`IServiceCollection.AddInstance`オーバー ロードとして`IKeyEscrowSink`インスタンスはシングルトンとして意図されています。 複数`IKeyEscrowSink`インスタンスが登録されて、キーを同時に複数のメカニズムをエスクローできるようにそのそれぞれがキーの生成中に呼び出されます。

API から情報を読み取るにはありません、`IKeyEscrowSink`インスタンス。 これはエスクロー メカニズムの設計理論と一致します。 キー マテリアルを信頼された証明機関にアクセスできるように意図したものと独自 escrowed マテリアルへのアクセスを必要はありません、アプリケーション自体がない信頼された証明機関であるためです。

次のサンプル コードを示しますの作成と登録、 `IKeyEscrowSink` "CONTOSODomain Admins"のメンバーだけが回復できるようにキーをエスクローする場所。

> [!NOTE]
> このサンプルを実行する Windows 8 のドメインに参加していることが必要]、[Windows Server 2012 マシン、およびドメイン コント ローラーが Windows Server 2012 またはそれ以降にする必要があります。

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
