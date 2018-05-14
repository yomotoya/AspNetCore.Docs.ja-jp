# <a name="key-vault-configuration-provider-sample-application-aspnet-core-2x"></a>キー コンテナーの構成プロバイダーのサンプル アプリケーション (ASP.NET Core 2.x)

このサンプルは、ASP.NET Core の Azure キー資格情報コンテナーの構成プロバイダーの使用を示しています。 2.x です。 ASP.NET Core 1.x サンプルでは、次を参照してください。[キー資格情報コンテナーの構成プロバイダーのサンプル アプリケーション (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x)です。

サンプルの動作の詳細については、次を参照してください。、 [Azure Key Vault の構成プロバイダー](xref:security/key-vault-configuration)トピックです。

## <a name="using-the-sample"></a>サンプルの使用
1. Key vault の作成し、ガイダンスに従って、アプリケーションの Azure Active Directory (Azure AD) を設定[Azure Key Vault の使用開始](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)です。
   * 使用して、資格情報コンテナーに機密情報を追加、 [AzureRM キー資格情報コンテナー PowerShell モジュール](/powershell/module/azurerm.keyvault)から使用可能な[PowerShell ギャラリー](https://www.powershellgallery.com/packages/AzureRM.KeyVault)、 [Azure Key Vault REST API](/rest/api/keyvault/)、または、 [Azure ポータル](https://portal.azure.com/)です。 機密情報は、いずれかとして作成*手動*または*証明書*シークレット。 *証明書*シークレットはアプリやサービスで使用する証明書が、構成プロバイダーによってサポートされていません。 使用する必要があります、*手動*構成プロバイダーを使用するための名前と値のペアの機密情報を作成するオプションです。
     * 単純なシークレットは、名前と値のペアとして作成されます。 Azure Key Vault のシークレット名は、英数字とハイフンに制限されます。
     * 階層型の値 (構成セクション) を使用して`--`(2 つのハイフン)、サンプルでは、区切り記号として。 サブキーのセクションを区切るために通常使用されるコロン[ASP.NET Core 構成](xref:fundamentals/configuration/index)、シークレット名に許可されていません。 そのため、2 個のダッシュが使用され、コロン、シークレットは、アプリの構成に読み込まれるときに切り替わります。
     * 2 つ作成*手動*機密情報の次の名前と値のペアを使用します。 最初のシークレットは、単純な名前と値を 2 番目のシークレット セクションとシークレットの名前のサブキーを使用して秘密の値を作成します。
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Azure Active Directory とサンプル アプリを登録します。
   * Key vault にアクセスするアプリを承認します。 使用すると、 `Set-AzureRmKeyVaultAccessPolicy` 、key vault にアクセスするアプリを承認するために PowerShell コマンドレットを提供`List`と`Get`で機密データへのアクセス`-PermissionsToSecrets list,get`です。

2. アプリの更新*される appsettings.json*の値を持つファイル`Vault`、 `ClientId`、および`ClientSecret`です。
3. その構成値を取得するサンプル アプリを実行する`IConfigurationRoot`秘密の名前と同じ名前にします。
   * 非階層型の値: の値は、`SecretName`で取得した`config["SecretName"]`です。
   * 階層型の値 (セクション)。 使用`:`(コロン) 表記または`GetSection`拡張メソッド。 構成値を取得するのにには、これらのアプローチのいずれかを使用します。
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`
