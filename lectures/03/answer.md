# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：第1回（素のLLM）は一般的な回答を返すのに対して、エージェントはツールを呼び出して適切な回答を行った。
- `--trace` で見えたツール呼び出し（ツール名と引数）：
    ```
    search_glossary({'query': 'LoRA'})\
    search_glossary({'query': 'QLoRA'})
    ```
- 1つの質問での LLM 呼び出し回数（`requests`）：2回
- 追加したツールと、docstring に書いた説明：make_quiz
    ```
    指定した回の用語から1つを選び、
    確認問題と答えを返す。
    「第2回の確認問題を出して」のように、
    復習用の問題を求められたときに使う。
    ```

## 宿題

- 取り組んだレベル：実装
- やったこと：
  - app/tools.py に save_learning_log を作り、app/agent.py で requires_approval=True を付けて登録した。
- 結果：

    ```bash
    $ AIE_MODEL=gemini-3.1-flash-lite uv run python -m app.agent --trace "第3回の講義で重要な点は何ですか？ 回答を学習ログに保存してください。"

    [model] gemini-3.1-flash-lite

    承認が必要です:
    - ツール: save_learning_log
        ID: call_462511
        引数: {'content': '第3回「AIエージェント」の重要ポイント: \n1. AIエージェントの構造は「モデル＋ループ＋ツール」。\n2. ツールの説明文の書き方が重要。\n3. エージェントループはコスト（料金・速度）に影響する。\n4. Pydantic AIでは型定義による安全なツール運用が可能。\n5. 重要な操作にはガードレールやHuman-in-the-loopが必要。'}

    この操作を実行しますか？ (y/n): y
    操作を承認します。
    第3回の講義内容を学習ログに保存しました。

    第3回では「AIエージェント」をテーマに、LLMがツールを活用して目的を達成する仕組みについて学びます。重要な点は以下の通りです。

    *   **エージェントの構造**: 「モデル・ループ・ツール」の3要素で構成されます。
    *   **ツールの役割**: モデルが説明文を読んで呼び出しを判断するため、説明文の精度が重要です。
    *   **コストの意識**: エージェントループはLLMを複数回呼ぶため、応答時間や料金に影響します。
    *   **開発の工夫**: Pydantic AIなどのフレームワークを活用し、型による安全なツール運用や、Human-in-the-loop（人の判断を挟む仕組み）によるガードレールが重要になります。

    （参照：第3回）

    --- trace ---
    [tool call]   get_lecture_summary({'lecture': 3})
    [tool return] get_lecture_summary -> # 第3回 AIエージェント

    ## この回のねらい

    - AIエージェントの基本構造が「モデル＋ループ＋ツール」であることを理解する
    - Pydantic AIで、ツールを使う学習支援エージェントを自分で作り、動かす

    ## 要点

    1.…
    [tool call]   save_learning_log({'content': '第3回「AIエージェント」の重要ポイント: \n1. AIエージェントの構造は「モデル＋ループ＋ツール」。\n2. ツールの説明文の書き方が重要。\n3. エージェントループはコスト（料金・速度）に影響する。\n4. Pydantic AIでは型定義による安全なツール運用が可能。\n5. 重要な操作にはガードレールやHuman-in-the-loopが必要。'})
    [tool return] save_learning_log -> 学習ログを保存しました: /workspaces/lecture-ai-engineering-2026/learning_log/log_20261105_161053.txt…
    [usage] RunUsage(cost=Decimal('0.00110400'), details={'text_prompt_tokens': 2514}, input_text_tokens=2514, input_tokens=2514, output_tokens=317, requests=3, tool_calls=2)
    ```

- 生成されたファイル
  - ファイル名: `learning_log/log_20261105_161053.txt`
  - 内容:
    ```text
    第3回「AIエージェント」の重要ポイント: 
    1. AIエージェントの構造は「モデル＋ループ＋ツール」。
    2. ツールの説明文の書き方が重要。
    3. エージェントループはコスト（料金・速度）に影響する。
    4. Pydantic AIでは型定義による安全なツール運用が可能。
    5. 重要な操作にはガードレールやHuman-in-the-loopが必要。
    ```

- 気づき・疑問：
  - app/tools.py に学習の記録を保存する関数を作成し、docstring にモデル向けの説明文を記載することで、エージェントから利用される。
  - GitHub Copilot を利用してコードを生成したものの、なかなか実行時のエラーが解消できなかったが、コンテキストに Pydantic AI公式ドキュメントを追加したところ、最終的に問題を解消することができた。
