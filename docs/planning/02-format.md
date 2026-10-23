# ② ショートのフォーマット

調査日: 2026-10-23。数値のうち YouTube 公式でないものは「目安」と明記する。出典は末尾。

## 結論（先に決めること）

| 項目 | 決定 | 理由 |
|---|---|---|
| 尺 | **30秒・45秒・60秒の3型**。立ち上げは 30秒（1問1答）と 45秒（用語解説）の2型から | 教育系は 30〜60秒に収束。「尺より視聴維持率」なので短い型を先に回し、データで伸ばす |
| 1本の粒度 | **1本1メッセージ**（用語1つ／問題1問／誤解1つ） | 冒頭3秒で言い切れない粒度は離脱される |
| 冒頭 | **挨拶・自己紹介・「今日は〜を解説します」を禁止**。0秒から用語ドン＋フック台詞 | 視聴者はキャラを知らない。前置きの価値がない |
| 字幕 | セリフ全文を常時表示。1行13文字以内×最大2行、白＋縁取り、強調語だけ色変え | 無音視聴でも成立させる。Shorts は 96% がモバイル視聴 |
| 画面 | 1080×1920。右端 192px・下端 672px・上端 288px を UI 被り領域として空ける | いいね列・タイトル・登録ボタンに隠れる |
| 話速 | TTS は **6〜7文字/秒**。30秒＝本文140〜170字、45秒＝220〜260字、60秒＝300〜350字 | 放送標準（5〜6文字/秒）よりやや速め。クイズの「間」は別枠で確保 |
| 固定要素 | シリーズ名＋ナンバリング、末尾の決め台詞（CTA）、キャラ固定 | 系統化とブランド認知。固定 OP は冒頭ではなく末尾に置く |
| 配信 | 立ち上げ期は **毎日1本**、安定後は週4〜5本＋長尺週1。平日 21時前後 | 自動化で本数は出せるので、まずデータを取る |

## KPI（YouTube Studio の指標）

| 指標 | 目安 | 意味 |
|---|---|---|
| Viewed vs Swiped away | Viewed 75% 以上 | 冒頭数秒で止まってもらえたか。フックの評価 |
| 平均視聴率（APV） | 70% 以上（30秒未満なら 70〜80%） | 本文の評価。ループで 100% 超もありうる |
| エンゲージビュー | 伸び率で見る | 2025-03-31 以降、収益・YPP 要件はこれだけが対象 |
| 登録者転換 | 本数あたり | CTA・関連動画リンクの評価 |

## 3つの型（台本テンプレート）

### A. 30秒「1問1答」（クイズ型）
最も安く作れ、ループしやすく、シリーズ化しやすい。試験系（応用情報・新試験）の主力。

| 秒 | 内容 | 画面 |
|---|---|---|
| 0–2 | 「問題。」＋問題文 | 問題文を全画面テロップ、キャラは指差し |
| 2–8 | 選択肢 A/B/C を読み上げ | 3択をカード表示 |
| 8–11 | 3秒カウント（無音＋SE） | カウントダウン、キャラは考え顔 |
| 11–14 | 正解発表 | 正解カードだけ色が変わる、キャラは happy |
| 14–27 | 解説。なぜ他が違うかを一言ずつ | 板書に要点3点まで |
| 27–30 | CTA「全問正解ならコメントで『満点』」＋次回予告 | シリーズ名・次の番号 |

誤答は「迷わせる誤答」にする。不正解だった人ほど解説を見る傾向がある。

### B. 45秒「用語解説」（Hook→誤解→答え→理由→CTA）
用語・仕組みの説明。AI 用語、セキュリティ用語の主力。

| 秒 | 内容 |
|---|---|
| 0–3 | フック（型2・3・4 のどれか）＋結論ワードを画面に |
| 3–10 | よくある誤解・問題の提示「多くの人はこう思っている」 |
| 10–20 | 答えを1文で言い切る |
| 20–38 | 理由・仕組み。図1枚、箇条3点まで |
| 38–45 | CTA＋「明日は〇〇」。関連動画リンクで長尺へ |

### C. 60秒「間違えやすい〇〇」（比較型）
認証と認可、暗号化とハッシュ、のような対比。左右2分割の表で見せる。

フック（違い言える？）→ 誤った理解を1つ → 正解 → 左右対比表 → 覚え方を一言 → CTA

### D. 15〜20秒「3秒で」（ループ型・実験枠）
フック→答え→理由1つ。最後の一言が冒頭につながる文にして2周目に入らせる。

## 冒頭の一言（フック型一覧）

本企画向けの例。「知ってる？」「実は」「9割」「秒数」「問題。」の5型を基本とし、同じ型が3日続かないようにローテーションする。

| # | 型 | 例 |
|---|---|---|
| 1 | 知ってる？ | 「パスキーって知ってる？」 |
| 2 | 実は〜（逆説） | 「実は、鍵マークがあっても Wi-Fi は安全じゃない」 |
| 3 | 9割（多数派否定） | 「9割が間違えてる『HTTPS＝安全』」 |
| 4 | 秒数 | 「30秒で AI の『トークン』がわかる」 |
| 5 | クイズ即出し | 「問題。この3つのパスワード、いちばん弱いのはどれ？」 |
| 6 | 誤答提示 | 「VPN＝匿名になると思ってない？」 |
| 7 | 損失回避 | 「知らないと口座が空になるフィッシングの手口」 |
| 8 | 断言 | 「同じパスワードの使い回しは今日やめて」 |
| 9 | 1日1語 | 「今日の IT 用語：ゼロデイ」（0.5秒で用語を画面ドン） |
| 10 | ビフォー/アフター | 「2段階認証をオンにする前と後で、乗っ取り率はこう変わる」 |
| 11 | あるある | 「カフェのフリー Wi-Fi でネットバンク開いた人、危険です」 |
| 12 | 数値インパクト | 「去年の個人情報漏えい、1,000万件超」（実データに差し替え） |
| 13 | 比較 | 「認証と認可の違い、言える？」 |
| 14 | Yes/No | 「これ、安全？危険？」（2択を画面に出して3秒カウント） |
| 15 | 告白 | 「実は私も〇〇で失敗した」（キャラの一人称） |

アンチパターン: 挨拶、自己紹介、目次型導入、数字も対象もない曖昧な「実は…」、冒頭3秒に文字が出ない、1本に複数メッセージ。

## 画面レイアウト（9:16, 1080×1920）

```
y=0    ┌──────────────────────────┐
       │  (UI被り: 上 288px)        │  シリーズ名・番号だけ薄く
y=288  ├──────────────────────────┤
       │  板書エリア                │  図・3択カード・対比表
       │  （タイトル/問題文/図）     │  横は 120〜888px に収める
y=1000 ├──────────────────────────┤
       │  字幕帯（13字×2行）        │  縦 45〜65% の帯
y=1250 ├──────────────────────────┤
       │  キャラ（胸上〜腰上）       │  左寄せ。右 192px は空ける
       │  (UI被り: 下 672px)        │  登録ボタン・タイトルに隠れる
y=1920 └──────────────────────────┘
```

- キャラは下段左に配置し、板書と字幕を邪魔しない。右列はいいね・コメント・共有に隠れる。
- 字幕はセリフ全文。読める速度は 1秒4文字程度なので、1カットの字幕は 2行 26字＝6〜7秒以内で切り替える。
- 最終確認は実機プレビューで行う（セーフゾーンの数値は非公式）。

## Ink での台本表現（案）

ScenarioXR の既存タグ（`#anim` `#expression` `#voice` `#wait_event`）を流用し、ショート用に板書・字幕・効果音・カウントのタグを足す。LLM が生成するのは Ink そのものではなく、この型に沿った構造化 JSON（問題文・選択肢・解説など）で、Ink はテンプレートから機械生成する方が安定する（③で詳述）。

```ink
// A型: 1問1答 #12 サブネットマスク
#series:ap-quiz:12
#board:title:サブネットマスク

#anim:miko:point
#board:question:192.168.1.0/26 のホスト数は？
miko: 問題。スラッシュ26のネットワーク、使えるホストはいくつ？

#board:choices:A 62|B 64|C 126
miko: A 62、B 64、C 126。

#anim:miko:think
#count:3

#board:answer:A
#expression:miko:happy
miko: 正解は A の 62。

#board:notes:ホスト部は 32-26=6ビット|2^6=64|ネットワークとブロードキャストを引いて 62
miko: ホスト部は 6 ビットで 64 通り。そこから全部ゼロと全部イチを引くから 62。
miko: B は引き忘れ、C はスラッシュ25の答え。

#expression:miko:neutral
miko: 全問正解した人はコメントで「満点」。次は #13、CIDR の読み方。
```

`miko:` の行は TTS に渡す本文で、字幕にもそのまま使う。感情・モーションはタグで明示し、LLM に自由記述させない。

## タイトル・説明欄・ハッシュタグ

- タイトルは Shorts 棚で約40文字しか見えない。先頭にキーワード＋フック。例: `#12【9割が誤解】HTTPS＝安全ではない｜セキュリティ1問1答`
- ハッシュタグは 3〜5個。`#shorts` ＋ジャンル2つ（`#応用情報` `#セキュリティ`）。
- 説明欄: 100文字前後の要約＋長尺リンク＋音声クレジット（自作 TTS なら「音声: Irodori-TTS」）。
- Shorts series 機能（2026-09 発表、YPP 参加者向け・段階提供）でシーズン/エピソード化できる。それまでは再生リスト＋ナンバリングで代用。

## 出典

- Engadget: Shorts 3分化 https://engadget.com/entertainment/youtube/youtube-shorts-can-now-run-up-to-three-minutes-160002081.html
- CoSchedule: 2025-03-31 視聴回数定義変更 https://coschedule.com/social-media-marketing/youtube-metrics
- YouTube Analytics API: engagedViews https://developers.google.com/youtube/analytics/metrics?hl=ja
- CreatorEssentials: Viewed vs Swiped https://www.creatoressentials.com/glossary/viewed-vs-swiped-away/
- Shortimize: 維持率ベンチマーク https://www.shortimize.com/blog/youtube-shorts-retention-rate
- vidIQ: ループ / 投稿頻度 / 投稿時間 https://vidiq.com/blog/post/double-youtube-watch-time-looping-shorts/ , https://vidiq.com/blog/post/How-Often-Post-on-Youtube
- Metricool 2026 YouTube トレンド https://metricool.com/youtube-trends/
- tatap: セーフゾーン https://tatap.jp/knowledge/short-video-size/ , Reap https://reap.video/blog/short-form-video-safe-zones
- 0120.co.jp: テロップ / 台本 / 構成 https://0120.co.jp/blog/video-125/ , https://0120.co.jp/blog/video-110/ , https://0120.co.jp/blog/video-13/
- note 僭越次郎: 冒頭30選 https://note.com/senetsujirou/n/n408624b625c5 、瀬戸ユウキ: 冒頭3秒 https://note.com/yuki_seto_ai/n/n596443e62450
- kamenochie: 話速と文字数 https://kamenochie.com/kikanunotane-time-wordcount/ 、luft 台本尺計算 https://www.luft.co.jp/cgi/youtube-script-duration.php
- gluglu: あきとんとんの戦略 https://gluglu.jp/1348094 、コクリコ https://cocreco.kodansha.co.jp/cocreco/general/study/iRymt
- ずんだもん IT 用語1分解説 https://www.youtube.com/shorts/t7XHFXMphhc 、ずんだもんコメンタリー https://zundacommentary.github.io/
- 1日1語 IT パスポート https://kgrtmno0507.github.io/
- TechCrunch: Shorts series https://techcrunch.com/2026/09/23/youtubes-new-short-series-feature-brings-episodic-viewing-to-shorts/
- YouTube 公式: 関連動画リンク https://blog.youtube/creator-and-artist-stories/youtube-related-videos-traffic-guide/
- 高知工科大: クイズ正誤と解説閲覧 https://www.kochi-tech.ac.jp/library/ron/pdf/2023/03/15/a1240548.pdf
- pamxy: 投稿時間 / ハッシュタグ https://pamxy.co.jp/marke-driven/sns-marketing/youtube/youtube-short-time/
