# <a name="custom-model-binding-demo"></a>カスタム モデル バインディング デモ

テストすることができます、`ByteArrayModelBinder`アプリケーションを実行して、ImageController エンドポイントに base64 でエンコードされた文字列を投稿して (/api/イメージ/)。 要求本文をフォーム データとしてでファイルとファイル名の proparties を指定する必要があります (Postman または同様のツールを使用)。 使用することができます[この文字列のサンプル](Base64String.txt)です。 結果は、指定したファイル名を持つ wwwroot/イメージ/アップロード フォルダーに保存されます。

カスタム バインディングの例をテストするには、次のエンドポイントを再試行してください:/api/authors/1/api/authors/2 (NOT FOUND)/api/boundauthors/1/api/boundauthors/2 (NOT FOUND)/api/boundauthors/get/1/api/boundauthors/get/2 (NO CONTENT) - この操作をチェックしませんnull および Not Found 戻り値
