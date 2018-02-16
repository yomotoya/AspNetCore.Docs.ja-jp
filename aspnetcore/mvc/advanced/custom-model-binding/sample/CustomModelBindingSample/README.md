# <a name="custom-model-binding-demo"></a>カスタム モデル バインド デモ

アプリケーションを実行し、base64 でエンコードされた文字列を ImageController エンドポイント (/api/image/) に投稿して `ByteArrayModelBinder` をテストすることができます。 要求の本文でファイルとファイル名のプロパティをフォーム データとして指定する必要があります (Postman または同様のツールを使用します)。 [この文字列のサンプル](Base64String.txt)を使用することができます。 結果は、指定したファイル名で wwwroot/images/upload フォルダーに保存されます。

カスタム バインドの例をテストするには、次のエンドポイントを試してください: /api/authors/1 /api/authors/2 (NOT FOUND) /api/boundauthors/1 /api/boundauthors/2 (NOT FOUND) /api/boundauthors/get/1 /api/boundauthors/get/2 (NO CONTENT) - このアクションは、null をチェックせず、Not Found を返します。
