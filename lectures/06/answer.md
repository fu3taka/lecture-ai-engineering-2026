# 第6回 解答

## 演習

<!-- [tool call] の行は、表示された行をそのまま貼り付けてください。終わらなかった STEP は、空欄のままでかまいません。 -->

- STEP 1 のツールの名前：glossary_search_glossary
- STEP 2 の [tool call]：glossary_search_glossary({'query': '3レベル'})
- STEP 3 の [tool call]：fetch_fetch({'url': 'https://modelcontextprotocol.io/'})
- STEP 4 の [tool call]：aws_aws___search_documentation({'topics': ['general'], 'search_phrase': 'Amazon S3 versioning overview'})
- 気づき・疑問：MCPサーバーを利用すると、エージェントのコードを変更することなくツールを追加することができた。

## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：基礎
- やったこと：app/mcp_server.py に `server.tool()(tools.get_lecture_summary)` を追加することでツールを公開した。
- 結果：`--list-tools` に `glossary_get_lecture_summary` が追加され、「第2回では何を学びますか？」という質問で適切な回答が返って来た。
- 気づき・疑問：mcp_server.py にツールを登録すると、エージェントのコードを変更することなくツールを追加することができ、エージェントが起動時にサーバーからツール一覧を取得するためだと理解した。
