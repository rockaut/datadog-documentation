---
further_reading:
- link: /tracing/api_catalog/
  tag: ドキュメント
  text: API Catalog
is_beta: true
title: トラブルシューティング
---

Datadog API カタログで想定外の挙動に遭遇した場合、このガイドが問題解決の助けとなるかもしれません。問題が解決しない場合は、[Datadog サポート][1]にお問い合わせください。

## エンドポイントの欠落

API カタログは APM トレーシングに基づいているため、最初のステップはサービスを確実にインスツルメントすることです。アプリの **Learn More** をクリックし、**Troubleshoot** を選択してこのことを確認します。
{{< img src="tracing/api_catalog/api-catalog-discovery-learn-more.png" alt="アプリの Learn More ボタン" style="width:30%;text-align: left;" >}}

サービスがインスツルメントされているにもかかわらずエンドポイントが表示されない場合、2 つの選択肢があります。
- **OpenAPI ファイルをアップロードする**: 仕様ファイルを **Catalog** ページにアップロードすると、トラフィックに見られるトレースが一致するエンドポイントに自動的に接続されます。トラフィックによっては、アップロード後すぐに **Explorer** ページにエンドポイントが表示されます。
- **セットアップウィザードを使用して、サービスのオートディスカバリーを有効にする**: すべてのインスツルメンテーションライブラリがサポートされているわけではないため、**Setup** ページのウィザードを使用して、アップグレードが必要か、または不足しているタグの追加方法を確認できます。

## OpenAPI ファイルのテレメトリーデータがない

API カタログは APM トレーシングに基づいているため、エンドポイントのトレースがある場合にのみトラフィックからの情報が表示されます。OpenAPI ファイルをアップロードすると、Datadog がエンドポイントのスパンを取り込んだ後に、デプロイメントデータ (Last Seen、環境など) が表示されます。

## 新しいモニターのデータがない

API カタログは APM トレーシングに基づいているため、エンドポイントのトレースがある場合にのみトラフィックからの情報が表示されます。モニターグラフにデータが表示されない場合は、以下のオプションのいずれかが適用される可能性があります。
- エンドポイントは、OpenAPI を通じて登録/アップロードされて以来、まだ使用されていません。
- トレースは Agent 側でサンプリングされます。詳しくは[取り込みコントロール][3]を参照してください。

## 参考資料

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/help/
[2]: /ja/tracing/trace_collection/
[3]: /ja/tracing/trace_pipeline/ingestion_controls/