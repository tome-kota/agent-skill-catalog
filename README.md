# my-agent-skills

## このリポジトリについて

このリポジトリは、エージェントで使うスキルを育てていくための置き場です。

ここで扱うスキルは、単なる作業手順ではなく、実務で繰り返し出てくる判断の難しさを扱うことを意識しています。たとえば、どこを深くレビューすべきか、どう分割すれば安全に出せるか、設計やリファクタリングの論点をどう整理するか、といった場面です。

各スキルの実行仕様は `SKILL.md` に置き、README には人がそのスキルを使う価値を判断しやすくするための情報を置いています。

## スキル一覧

| スキル | 一言説明 | こんな人・場面に向く |
| --- | --- | --- |
| [core-logic-review-prioritizer](./.agents/skills/core-logic-review-prioritizer/README.md) | 大きな変更の中から、人が深く見るべき業務ロジックを絞り込む | 大きな diff を前に、どこにレビュー時間を使うべきか迷いやすい人 |
| [delivery-slice-planner](./.agents/skills/delivery-slice-planner/README.md) | 大きな実装案を、安全に進めやすい delivery slice（安全に進める作業単位）と PR 列に分ける | 実装順、PR 分割、リリース順を先に整理したい人 |
| [quality-requirements-elicitation-coach](./.agents/skills/quality-requirements-elicitation-coach/README.md) | 要件が薄い依頼に対して、実装前に確認すべき品質観点を引き出す | すぐ作れそうだが、後で要件漏れが出がちな相談を扱う人 |
| [refactoring-review-router](./.agents/skills/refactoring-review-router/README.md) | リファクタリングの悩みを、適切な診断レンズ（切り分けの見方）に振り分ける | 状態の複雑さ、責務の混線、依存の広がりのどれが本丸か見極めたい人 |
| [review-orchestrator](./.agents/skills/review-orchestrator/README.md) | コードレビューを 5 つの視点に分けて、見落としを減らす | AI 生成コードや大きめの変更を、観点漏れなくレビューしたい人 |
| [software-design-review-router](./.agents/skills/software-design-review-router/README.md) | 設計レビューの論点を、最小限の適切なレンズ（評価軸）に整理する | 設計の違和感はあるが、何を軸に評価すべきかまだ曖昧な人 |
| [tdd](./.agents/skills/tdd/README.md) | ふるまいテストの赤から最小の実装を積み上げ、変更を安全に進める | バグ修正や機能追加を、後付けテストではなく観測できる振る舞いから実装したい人 |

## このリポジトリの考え方

このリポジトリでは、次のような考え方を大事にしています。

- AI に単に「うまくやって」と任せるのではなく、判断の型を明示する
- 汎用的すぎるチェックリストより、困りごとに直結するスキルを作る
- 詳しい運用ルールは `SKILL.md` に寄せ、人が選ぶための情報は README に寄せる
- 似たテーマのスキルでも役割を分け、使い分けしやすくする

そのため README は仕様書の要約ではなく、「なぜこのスキルがあるのか」「自分に必要そうか」を判断するための入口として書いています。
