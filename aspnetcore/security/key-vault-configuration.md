---
title: "Azure Key Vault の構成プロバイダー"
author: guardrex
description: "Azure キー資格情報コンテナーの構成プロバイダーを使用して、実行時に読み込まれる名前と値のペアを使用してアプリケーションを構成する方法を説明します。"
keywords: "ASP.NET Core、構成では、Azure Key Vault"
ms.author: riande
manager: wpickett
ms.date: 08/09/2017
ms.topic: article
ms.assetid: 0292bdae-b3ed-4637-bd67-19b9bb8b65cb
ms.prod: asp.net-core
uid: security/key-vault-configuration
ms.openlocfilehash: 72b6098b2a71957da338ef36beff4808201773f4
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="azure-key-vault-configuration-provider"></a>Azure Key Vault の構成プロバイダー

によって[Luke Latham](https://github.com/GuardRex)と[Andrew スタントン-看護師](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

表示または 2.x のサンプル コードをダウンロードします。

* [基本的なサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x)-をアプリに秘密の値を読み取ります。
* [キー名のプレフィックス サンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x)- 読み取り秘密の値を別の各アプリのバージョンのシークレットの値セットを読み込むことができるアプリのバージョンを表すキー名のプレフィックスを使用します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

表示または 1.x のサンプル コードをダウンロードします。

* [基本的なサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x)-をアプリに秘密の値を読み取ります。
* [キー名のプレフィックス サンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x)- 読み取り秘密の値を別の各アプリのバージョンのシークレットの値セットを読み込むことができるアプリのバージョンを表すキー名のプレフィックスを使用します。 

---

このドキュメントの使用方法を説明します、 [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)構成プロバイダーを Azure Key Vault シークレットからアプリケーションの構成値を読み込めません。 Azure Key Vault は、クラウド ベース サービス暗号化キーやアプリやサービスによって使用されるシークレットを保護するのに役立ちます。 一般的なシナリオは、機密性の高い構成データにアクセスを制御して、FIPS 140-2 の要件を満たすレベル 2 検証ハードウェア セキュリティ モジュール (HSM) の構成データを格納する場合。 この機能は、以降の ASP.NET Core 1.1 を対象とするアプリケーションで使用可能です。

## <a name="package"></a>Package
使用するには、プロバイダーへの参照を追加、 [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)パッケージです。

## <a name="application-configuration"></a>アプリケーションの構成
使用してプロバイダーを調べることができます、[アプリのサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)です。 Key vault を確立し、資格情報コンテナー内の機密情報を作成すると、サンプル アプリでは、安全にシークレットの値の構成に読み込んで web ページに表示します。

プロバイダーを追加、`ConfigurationBuilder`で、`AddAzureKeyVault`拡張機能です。 アプリでは、サンプル、拡張機能はから読み込まれた 3 つの構成値を使用して、*される appsettings.json*ファイル。

| アプリケーション設定    | 説明                    | 例                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Azure Key Vault の名前           | contosovault                                 |
| `ClientId`     | Azure Active Directory アプリ Id  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Azure Active Directory アプリ キー | g58K3dtg59o1Pa + e59v2Tx829w6VxTB2yv9sv/101di = |

[!code-csharp[プログラム](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1&highlight=2,7-10)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>シークレットの資格情報コンテナーを作成および構成値 (basic サンプル) の読み込み
1. Key vault の作成し、ガイダンスに従って、アプリケーションの Azure Active Directory (Azure AD) を設定[Azure Key Vault の使用開始](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)です。
  * 使用して、資格情報コンテナーに機密情報を追加、 [AzureRM キー資格情報コンテナー PowerShell モジュール](/powershell/module/azurerm.keyvault)から使用可能な[PowerShell ギャラリー](https://www.powershellgallery.com/packages/AzureRM.KeyVault)、 [Azure Key Vault REST API](/rest/api/keyvault/)、または、 [Azure ポータル](https://portal.azure.com/)です。 機密情報は、いずれかとして作成*手動*または*証明書*シークレット。 *証明書*シークレットはアプリやサービスで使用する証明書が、構成プロバイダーによってサポートされていません。 使用する必要があります、*手動*構成プロバイダーを使用するための名前と値のペアの機密情報を作成するオプションです。
    * 単純なシークレットは、名前と値のペアとして作成されます。 Azure Key Vault のシークレット名は、英数字とハイフンに制限されます。
    * 階層型の値 (構成セクション) を使用して`--`(2 つのハイフン)、サンプルでは、区切り記号として。 サブキーのセクションを区切るために通常使用されるコロン[ASP.NET Core 構成](xref:fundamentals/configuration)、シークレット名に許可されていません。 そのため、2 個のダッシュが使用され、コロン、シークレットは、アプリの構成に読み込まれるときに切り替わります。
    * 2 つ作成*手動*機密情報の次の名前と値のペアを使用します。 最初のシークレットは、単純な名前と値を 2 番目のシークレット セクションとシークレットの名前のサブキーを使用して秘密の値を作成します。
      * `SecretName`: `secret_value_1`
      * `Section--SecretName`: `secret_value_2`
  * Azure Active Directory とサンプル アプリを登録します。
  * Key vault にアクセスするアプリを承認します。 使用すると、 `Set-AzureRmKeyVaultAccessPolicy` 、key vault にアクセスするアプリを承認するために PowerShell コマンドレットを提供`List`と`Get`で機密データへのアクセス`-PermissionsToKeys list,get`です。
2. アプリの更新*される appsettings.json*の値を持つファイル`Vault`、 `ClientId`、および`ClientSecret`です。
3. その構成値を取得するサンプル アプリを実行する`IConfigurationRoot`秘密の名前と同じ名前にします。
  * 非階層型の値: の値は、`SecretName`で取得した`config["SecretName"]`です。
  * 階層型の値 (セクション)。 使用`:`(コロン) 表記または`GetSection`拡張メソッド。 構成値を取得するのにには、これらのアプローチのいずれかを使用します。
    * `config["Section:SecretName"]`
    * `config.GetSection("Section")["SecretName"]`

アプリを実行すると、web ページには、秘密の読み込まれた値が示されます。

![Azure キー資格情報コンテナーの構成プロバイダー経由で読み込まれているシークレットの値を表示されているブラウザー ウィンドウ](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>プレフィックスが指定された資格情報コンテナーの機密情報を作成および構成の値 (キーの名前のプレフィックス-サンプル) の読み込み
`AddAzureKeyVault`実装を受け入れるオーバー ロードも提供`IKeyVaultSecretManager`構成のキーに変換がどのキー資格情報コンテナーの機密情報を制御することができます。 たとえば、アプリの起動時に指定したプレフィックス値に基づいてシークレットの値を読み込むインターフェイスを実装することができます。 これにより、たとえば、アプリのバージョンに基づくシークレットを読み込めません。

> [!WARNING]
> 複数のアプリのシークレットを key vault に配置するか、環境のシークレットを配置する、キー vault シークレットのプレフィックスを使用しない (たとえば、*開発*verus*運用*シークレット) を 1 つ資格情報コンテナー。 別のアプリとの開発と実稼働環境を最高レベルのセキュリティのアプリの環境を分離する個別のキー コンテナーを使用することをお勧めします。

Key vault にシークレットを作成する 2 つ目のサンプル アプリを使用して`5000-AppSecret`(key vault のシークレット名で許可されていない期間) を表すバージョン 5.0.0.0 のアプリのアプリのシークレット。 シークレットを作成する別のバージョン、5.1.0.0、`5100-AppSecret`です。 各アプリのバージョンを読み込みます独自秘密の値としてその構成`AppSecret`、機密情報が読み込まれるバージョンを削除します。 サンプルの実装を次に示します。

[!code-csharp[構成ビルダー](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=12)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load`バージョンというプレフィックスが付いているものを検索する資格情報コンテナーのシークレットを反復処理するプロバイダーのアルゴリズムでメソッドが呼び出されます。 バージョン プレフィックスがで検出されたときに`Load`、アルゴリズムを使用して、`GetKey`シークレット名の構成名を返すメソッド。 シークレットの名前からバージョン プレフィックス取り除いてし、アプリの構成に名前と値のペアの読み込みのシークレット名の残りの部分を返します。

このアプローチを実装する場合。

1. 資格情報コンテナーの機密情報が読み込まれます。
2. 文字列のシークレットを`5000-AppSecret`が一致します。
3. バージョン、 `5000` (dash) のまま、キー名から取り除かは`AppSecret`シークレットの値を持つアプリの構成を読み込めません。

> [!NOTE]
> 独自に提供することも`KeyVaultClient`実装`AddAzureKeyVault`です。 カスタムのクライアントを指定するには、クライアントの構成プロバイダーと、アプリの他の部分の 1 つのインスタンスを共有することができます。

1. Key vault の作成し、ガイダンスに従って、アプリケーションの Azure Active Directory (Azure AD) を設定[Azure Key Vault の使用開始](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)です。
  * 使用して、資格情報コンテナーに機密情報を追加、 [AzureRM キー資格情報コンテナー PowerShell モジュール](/powershell/module/azurerm.keyvault)から使用可能な[PowerShell ギャラリー](https://www.powershellgallery.com/packages/AzureRM.KeyVault)、 [Azure Key Vault REST API](/rest/api/keyvault/)、または、 [Azure ポータル](https://portal.azure.com/)です。 機密情報は、いずれかとして作成*手動*または*証明書*シークレット。 *証明書*シークレットはアプリやサービスで使用する証明書が、構成プロバイダーによってサポートされていません。 使用する必要があります、*手動*構成プロバイダーを使用するための名前と値のペアの機密情報を作成するオプションです。
    * 階層型の値 (構成セクション) を使用して`--`(2 つのハイフン)、区切り記号として。
    * 2 つ作成*手動*次の名前/値ペアのシークレット。
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * Azure Active Directory とサンプル アプリを登録します。
  * Key vault にアクセスするアプリを承認します。 使用すると、 `Set-AzureRmKeyVaultAccessPolicy` 、key vault にアクセスするアプリを承認するために PowerShell コマンドレットを提供`List`と`Get`で機密データへのアクセス`-PermissionsToKeys list,get`です。
2. アプリの更新*される appsettings.json*の値を持つファイル`Vault`、 `ClientId`、および`ClientSecret`です。
3. その構成値を取得するサンプル アプリを実行する`IConfigurationRoot`プレフィックス付きのシークレットの名前と同じ名前にします。 このサンプルではプレフィックスは、アプリのバージョンは、指定した、 `PrefixKeyVaultSecretManager` Azure Key Vault の構成プロバイダーを追加するとき。 値は、`AppSecret`で取得した`config["AppSecret"]`です。 アプリによって生成される web ページは、読み込まれた値を示しています。

   ![ブラウザー ウィンドウが、アプリのバージョンが 5.0.0.0 である場合は、Azure キー資格情報コンテナーの構成プロバイダー経由で読み込まれる秘密の値を表示](key-vault-configuration/_static/sample2-1.png)

4. プロジェクト ファイルでのアプリ アセンブリのバージョンを変更`5.0.0.0`に`5.1.0.0`アプリをもう一度実行します。 このとき、返される秘密の値は`5.1.0.0_secret_value`します。 アプリによって生成される web ページは、読み込まれた値を示しています。

   ![ブラウザー ウィンドウが、アプリのバージョンが 5.1.0.0 である場合は、Azure キー資格情報コンテナーの構成プロバイダー経由で読み込まれる秘密の値を表示](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>ClientSecret へのアクセスを制御
使用して、[シークレット マネージャー ツール](xref:security/app-secrets)維持するために、`ClientSecret`プロジェクト ソース ツリーの外部でします。 シークレット マネージャーでは、アプリ シークレットは、特定のプロジェクトに関連付けるしてそれらを複数のプロジェクト間で共有します。

証明書をサポートする環境での .NET Framework アプリを開発するときは、X.509 証明書を Azure Key Vault に認証できます。 X.509 証明書の秘密キーは、オペレーティング システムによって管理されます。 詳細については、次を参照してください。[クライアント シークレットではなく、証明書による認証](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret)です。 使用して、`AddAzureKeyVault`を受け入れるオーバー ロード、`X509Certificate2`です。

```csharp
var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates.Find(X509FindType.FindByThumbprint, config["CertificateThumbprint"], false);

builder.AddAzureKeyVault(
    config["Vault"],
    config["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(env.ApplicationName));
store.Close();

Configuration = builder.Build();
```

## <a name="reloading-secrets"></a>シークレットの再読み込み
シークレットはまでキャッシュ`IConfigurationRoot.Reload()`と呼びます。 期限切れ、無効になっている、およびアプリケーションまでで、key vault に更新されたシークレットが守られていない`Reload`を実行します。

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>無効になっており、有効期限が切れたシークレット
無効になっており、期限切れのシークレットをスロー、`KeyVaultClientException`です。 アプリを防ぐためがスローされることから、アプリを交換または無効/有効期限が切れてシークレットを更新します。

## <a name="troubleshooting"></a>トラブルシューティング
アプリケーションは、プロバイダーを使用して構成の読み込みに失敗すると、エラー メッセージに書き込まれます。、 [ASP.NET のログ記録インフラストラクチャ](xref:fundamentals/logging)です。 次の条件には、構成を読み込めないは禁止します。
* アプリは、Azure Active Directory に正しく構成されていません。
* Azure Key Vault に資格情報コンテナーが存在しません。
* アプリは、key vault にアクセスする承認されていません。
* アクセス ポリシーに含まれていない`Get`と`List`アクセス許可。
* Key vault に構成データ (名前と値のペア) は、不適切な名前、無効であるか有効期限が切れて、欠落しています。
* アプリが正しくない資格情報コンテナーの名前 (`Vault`)、Azure AD アプリ Id (`ClientId`)、または Azure AD のキー (`ClientSecret`)。
* Azure AD のキー (`ClientSecret`) 有効期限が切れています。
* ロードしようとしている値のアプリの構成キー (名前) が正しくないです。

## <a name="additional-resources"></a>その他の技術情報
* <xref:fundamentals/configuration>
* [Microsoft Azure: Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Key Vault のドキュメント](https://docs.microsoft.com/azure/key-vault/)
* [Azure Key Vault のキーを生成し、HSM で保護された転送する方法](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient クラス](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
