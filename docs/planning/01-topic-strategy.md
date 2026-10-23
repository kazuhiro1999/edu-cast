# ① トピック戦略

調査日: 2026-10-23。「推定」と書いた数字は第三者の推計。出典は末尾。

## 結論

**「体系化を背骨にして、時事をそこに接ぎ木する」ハイブリッド**を推奨する。チャンネルの看板は「情報系の学び（ITリテラシー）」の1ニッチに固定し、その中を3レーンに分ける。

| レーン | 比率 | 内容 | 役割 |
|---|---|---|---|
| A. 体系（背骨） | 60% | 応用情報 午前の30ユニット → 2027年の新試験シラバスへ移行 | 再生リスト・シリーズとして積み上がる。検索・回遊・長尺化の資産 |
| B. 時事 | 20% | PHP2Shell、IDCフロンティア、10大脅威、モデル発表 | 発見の瞬発力。必ず A のユニットに紐づけて「あとから見ても価値がある」形にする |
| C. AI 用語 | 20% | エージェント、MCP、RAG、ハルシネーション、AI 推進法 | 新設「データマネジメント試験」の AI 利活用領域と生成 AI パスポートに接続 |

曜日で固定する案: 月・木＝A（1問1答）、火・金＝A（用語解説）、水＝C、土＝B（その週の時事）、日＝休み or 長尺。

### なぜ「バラバラ」にしないか
- Shorts は長尺よりキーワード依存が小さく、広いトレンド語に寄る。トピックが散るとチャンネルの「誰向けか」が視聴者にもアルゴリズムにも伝わらない。
- 2026-09 発表の Shorts series 機能（シーズン／エピソード化、「Watch Series」ボタン）は、系統化された連番コンテンツを明確に優遇する設計。
- 資格系は「年度＋問番号」「用語名」で検索流入が取れる。1本ごとに独立した雑学だと蓄積しない。

### なぜ「純粋な体系化」だけにしないか
- 体系化だけでは発見のスパイクが起きない。時事は「今これを知らないと困る」という損失回避フックが使え、視聴の入口になる。
- ただし時事は48時間以内に出さないと価値が落ちる。自動パイプラインの速度が活きるのはここ。

## タイミングの機会: 2027年の IPA 試験制度刷新

ユーザーの言う「2027年に新しく出る IPS」は、**IPA 情報処理技術者試験の2027年度 新制度**と解釈した（「IPS」という公式名称は確認できず。IPA の誤記か、情報処理安全確保支援士との混同の可能性）。

| 事実 | 含意 |
|---|---|
| 現行制度は2026年度で終了。**「応用情報技術者試験」名義の最終回は2026年度後期（2027-02頃）** | 「最後の応用情報」は2027-02まで需要が続く。それ以降は名称を捨てる |
| 新制度は8区分。応用情報＋高度を統合した**「プロフェッショナルデジタルスキル試験（仮称）」**（マネジメント／データ・AI／システムの3領域）と、**「データマネジメント試験（仮称）」**を新設 | 新試験には対策コンテンツがまだ無い。先行者になれる |
| 全区分 CBT、記述式なし。ITパスポート／SG／FE は2027年度春頃、新設区分と支援士は2027年度夏〜秋頃開始 | 2027年前半に「新試験サンプル問題を解く」「シラバス案を読む」シリーズを出すのが最も収穫が大きい |
| シラバス案は Ver.0.1（2026-06〜08公開）、サンプル問題は2026-06-22公開 | まだ変わる。今作る動画は「試験名」ではなく「トピック名」でタグ付けし、制度変更で陳腐化しない形にする |
| 応募者規模: FY2024 合計 741,884人（iパス 309,068人、FE 受験 133,732人、AP 春秋合計 応募 約12.5万人、支援士 約4.9万人） | 年間約100万人の受験者層が常時存在し、iパス・支援士は増加傾向 |

**運用ルール**: タイトルは「セキュリティ1問1答 #12」のようにトピック名で付け、説明欄とハッシュタグで「#応用情報 #基本情報 #新試験」を併記する。試験名をタイトルに入れるのは「最後の応用情報」「新試験サンプル問題」のような時事回だけ。

### 背骨の30ユニット（応用情報 午前 → 新試験に引き継がれる領域）

午前80問はテクノロジ50／マネジメント10／ストラテジ20で、テクノロジ内では**セキュリティが約10問で最多**。Shorts の「1本1知識」に向く順に並べる。

| 分野 | ユニット |
|---|---|
| 基礎理論 | 2進数・16進数・補数 / 論理演算とビット演算 / 浮動小数点と誤差 / 計算量 O 記法 / データ構造（スタック・キュー・木・ハッシュ） / 探索・整列 |
| コンピュータシステム | CPU とパイプライン・キャッシュ / メモリ階層・仮想記憶 / RAID / 稼働率・MTBF・MTTR / 冗長化・クラスタ・負荷分散 / OS（プロセス・デッドロック） / 仮想化・コンテナ・クラウド |
| ネットワーク | TCP/IP と階層モデル / IP アドレス・サブネット・NAT / DNS・HTTP・メール / 無線 LAN・VPN |
| データベース | 正規化 / SQL 基礎 / トランザクション・ACID・ロック |
| セキュリティ | 共通鍵・公開鍵・ハイブリッド暗号 / ハッシュ・署名・PKI / 認証（多要素・FIDO・SSO） / 攻撃手法（XSS・SQLi・CSRF・ランサム） / セキュア開発・脆弱性管理（CVE・CVSS）← PHP2Shell と接続 |
| 開発・管理・戦略 | 開発モデル / テスト技法 / プロジェクト管理（WBS・EVM・クリティカルパス） / ITIL / 経営戦略・財務・知財・法務 |
| 新傾向枠 | 生成 AI・機械学習（AP Ver.7.0 の DX/AI 用語）、データ利活用（データマネジメント試験の先取り） |

高校「情報I」の4領域（共通テスト2026で平均点が11.57点下がり難化、16進数・ビット演算×画像処理が話題）も同じユニットで拾える。

### 時事レーンの直近ネタ（2026-10 時点）
- **PHP2Shell（CVE-2026-109618 / 109619）**: 2026-10-21 修正版公開、認証不要 RCE、CVSS 9.2。さくら・Cygames・pixiv が緊急メンテ。→ ユニット「脆弱性管理（CVE・CVSS）」「memory_limit とは」
- **IDCフロンティア ランサムウェア（2026-10-07）**: 495 の企業・自治体に影響、バックアップ削除。→ ユニット「3-2-1 バックアップ」「サプライチェーン攻撃」
- **バーガーキング 1,118万件流出（2026-10-15）** → 「フィッシング二次被害」
- **IPA 10大脅威 2026**: 「AI の利用をめぐるサイバーリスク」が初ランクイン → C レーンと接続
- **AI**: Gemini 4 / GPT-6 / Claude Opus 5.5・Haiku 5.5 のモデルラッシュ（発表日はまとめサイト間で食い違いあり、各社公式で確認）、AI 基本計画 第Ⅱ期（2026-07-14）

## 収益化と継続性

### 収益の現実（計画値）

| 項目 | 値 | 備考 |
|---|---|---|
| 日本の Shorts RPM | **10〜30円 / 1,000回**（保守値） | 推定の幅は 3〜100 円。教育系でも Shorts では差が小さい |
| 日本の長尺 RPM | 200〜600円 / 1,000回（推定） | 教育・ビジネスは長尺で高単価 |
| YPP 現行 | 登録者 1,000 ＋ 4,000時間/12か月 or Shorts 1,000万回/90日 | |
| **YPP 2027-02-01〜（新規）** | 登録者 1,000 ＋ **8,000時間/365日 or Shorts 2,000万回/90日** | 新規チャンネルの要件が倍増。既存パートナーは免除 |
| 拡充版 YPP | 登録者 500 ＋ 90日に3本 ＋ Shorts 300万回/90日 or 3,000時間 | メンバーシップ・投げ銭・ショッピングのみ |
| Shorts 収益の継続条件（2027〜） | 既存パートナーも Shorts 1,000万回/90日 を満たした月だけ Shorts 分が支払われる | |

Shorts 100万回でも1〜3万円。**Shorts の広告収益を目的にしない**。公式データでも「YPP チャンネルの 25% 超が Shorts から収益」だが、18,000 チャンネル分析では収益の主力は長尺。

### 収益の設計（三層）
1. **獲得（Shorts）**: 登録者と「何のチャンネルか」の認知を買う。KPI は登録者転換と関連動画クリック。
2. **収益（長尺・会員・物販）**:
   - 同じパイプラインで **Shorts 5本 → 10分の長尺講義**を自動合成する（16:9 レイアウト変種）。8,000 時間要件と長尺 RPM の両方に効く。
   - 参考書アフィリエイト（Amazon アソシエイト）、自作の一問一答アプリ／問題集（note・Kindle・LINE）。先行事例は書籍（KADOKAWA 等）とアプリ連携。
   - 拡充版 YPP（500人）でメンバーシップを早期に開ける。
3. **上流（開発者としての資産）**: 「Ink 台本 → TTS → エンジン再生 → 動画」のパイプライン自体が、企業研修・社内教育向けの自動解説動画生成という B2B の商材になる。チャンネルはそのショーケース。個人開発者にとっての最大の upside はここにある可能性が高い。

### 継続性のボトルネックは制作時間ではなくファクトチェック
- 自動化で1本あたりの機械コストは数分・数十円になる。人の時間は**レビュー 10〜15分/本**を上限に設計する。
- IT・セキュリティは誤りのコストが高い。台本 JSON に出典（IPA 過去問の年度・問番号、CVE ID、公式 URL）を必須フィールドにし、出典のない断定を LLM に書かせない。
- 時事回は「一次情報（IPA/JPCERT/ベンダー公式）が出てから」をルールにする。

## ポリシーとリスク

| リスク | 内容 | 対策 |
|---|---|---|
| 「不正なコンテンツ」ポリシー（2025-07-15） | AI ツール自体は可。表面的な差しかない量産、同じナレーションのスライドショー量産が NG | 台本は毎本オリジナル。立ち絵・BGM が同じでも中身が固有であること。テンプレ文の使い回し率を管理 |
| AI 開示 | 3D キャラ＋合成音声は「明らかに非現実的」区分で概要欄開示で足りる見込み。API の `containsSyntheticMedia` は安全側で true | 概要欄に「音声: Irodori-TTS」「キャラ: 自作 VRM」を固定記載 |
| Likeness detection（2025-10〜、2026-09 に声の検出予告） | 実在人物の顔・声を模すのは NG | オリジナルキャラ。実在声優のクローンは使わない |
| 受容 | 国内調査（n=300）で「AI 生成の明示を望む」62.7% | AI であることを隠さず前面に出す。キャラの人格を一貫させる |
| **Irodori-TTS の重みライセンス** | コードは MIT だが、重みは「非商用」記述の転載もあり未確認 | **収益化前にモデルカードで商用可否を確認**。不可なら LoRA 自作声か別エンジンへ |
| 試験制度の変更 | シラバス案 Ver.0.1、まだ変わる | 試験名ではなくトピック名でタグ付け |
| 誤 BAN | 販促資料ベースだが停止事例の主張あり | 台本・制作過程の記録を保持、21日以内に異議申立て |

## 競合の薄さ
IT 特化の VOICEVOX/ずんだもん系で登録者1万超は確認できなかった。ITパスポート系の既存大手（ITすきま教室 8.5万、せかチャン 9万）は長尺＋書籍が中心で、Shorts 特化かつ応用情報・新試験レベルは空いている。英語系（矢作とアイク 70万、ライアン鈴木 120万）は「人物＋大字幕＋1表現＋毎日定時」で、フォーマットの参考になる。

## 出典
- IPA 新制度（2026-03-31） https://www.ipa.go.jp/shiken/syllabus/henkou/2025/20260331.html 、シラバス案（2026-06-30） https://www.ipa.go.jp/shiken/syllabus/henkou/2026/20260630.html 、サンプル問題（2026-06-22） https://www.ipa.go.jp/shiken/syllabus/henkou/2026/20260622.html
- 日経xTECH「新試験は2027年夏以降 記述式なし」 https://xtech.nikkei.com/atcl/nxt/column/18/00001/11639/ 、TAC https://www.tac-school.co.jp/kouza_joho/joho_shiken_2027.html
- IPA 応募者統計 FY2024 https://www.ipa.go.jp/shiken/reports/ip-oubo2024.html 、AP 2025 春 https://www.ipa.go.jp/news/2025/shiken_20250703.html 、AP 2025 秋 https://www.ipa.go.jp/news/2025/shiken_20251225.html
- IPA AP シラバス Ver.7.0 https://www.ipa.go.jp/shiken/syllabus/nq6ept000000149q-att/syllabus_ap_ver7_0.pdf 、ITEC 出題分析 https://www.itec.co.jp/examination/ap/contents/h30a_ap_exam-guide.pdf
- PHP2Shell: IPA https://www.ipa.go.jp/security/security-alert/2026/pre-alert20261021.html 、Flatt Security https://blog.flatt.tech/entry/php2shell 、日経xTECH https://xtech.nikkei.com/atcl/nxt/column/18/00001/12112/
- IDCフロンティア https://atmarkit.itmedia.co.jp/ait/articles/2610/09/news036.html 、バーガーキング https://www.watch.impress.co.jp/docs/news/2148486.html 、10大脅威 2026 https://www.ipa.go.jp/security/10threats/10threats2026.html
- YPP 2027 改定: 公式ブログ https://blog.youtube/news-and-events/youtube-partner-program-updates-2027-new-opportunities-earn/ 、TechCrunch https://techcrunch.com/2026/08/10/youtube-now-requires-creators-to-have-twice-as-many-watch-hours-to-start-earning-money/ 、ヘルプ https://support.google.com/youtube/answer/12843009?hl=en
- YPP 資格 https://support.google.com/youtube/answer/72851?hl=ja 、拡充版 https://support.google.com/youtube/answer/13429240?hl=ja 、Shorts 収益ポリシー https://support.google.com/youtube/answer/12504220?hl=ja
- 不正なコンテンツ改定 https://www.socialmediatoday.com/news/youtube-clarifies-monetization-update-inauthentic-repeated-content/752892/ 、AI ラベル https://www.techno-edge.net/article/2026/05/28/5112.html 、Likeness detection https://techcrunch.com/2025/10/21/youtubes-likeness-detection-technology-has-officially-launched
- RPM 推定 https://bloomeria.jp/blog/youtube-shorts-monetization-guide 、https://stock-sun.com/column/youtube-ad-revenue/ 、AIR 18,000ch 分析 https://air.io/en/audience-growth/do-youtube-shorts-help-your-long-form-videos-grow-data-from-18000-channels
- 受容調査 https://ecnomikata.com/ecnews/51052/ 、チャンネル規模 https://yutura.net/channel/23631/chart/ 、ITすきま教室 https://www.watch.impress.co.jp/docs/pa/impress/1554924.html
- Irodori-TTS https://github.com/Aratako/Irodori-TTS 、GIGAZINE https://gigazine.net/news/20260607-irodori-tts-v3/
