# ④ 類似事例

調査日: 2026-10-23。GitHub のスター数は取得時点。【推定】は調査者の推測。

## 全体像

| 方式 | 代表例 | 状況 |
|---|---|---|
| テンプレ型（ゆっくり / ずんだもん 立ち絵＋字幕） | YMM4、remotion-voicevox-template | 事例が最も多く、2025年以降は **Remotion（React）系がデファクト**。「Claude Code スキル化」が2026年の主流 |
| 生成動画型（Sora / Veo） | 海外 faceless 系 SaaS | 露出は取れていたが 2026-01 に YouTube が大型 AI スロップ 16ch を停止。「AI スロップ」の本丸 |
| **エンジン再生型（台本 → VRM をエンジンで再生 → 録画）** | VrmVideoMaker（GUI）、avatarcam（CLI） | **公開事例がほぼ無い**。three-vrm のヘッドレス録画は GitHub 検索 0 件。Ink / Yarn Spinner / ティラノを動画生成に使った公開プロジェクトも 0 件 |

ユーザーの構想（Ink 台本をエンジンで再生して録画）は先行例がない空白地帯。新規性がある反面、品質基準と参照実装を自分で作ることになる。

## A. ゆっくり / VOICEVOX / ずんだもん の自動生成

| プロジェクト | ★ | 構成 | 本企画への示唆 |
|---|---|---|---|
| nyanko3141592/remotion-voicevox-template | 284 | Remotion + VOICEVOX + Claude Code。ずんだもん＆めたん掛け合い、口パク・表情付き、MIT（2026-01） | この領域で最も参照されるテンプレ。「Claude と会話して台本→TTS→Remotion」の流れはそのまま参考になる |
| motemen/markdown-to-zundamon | 67 | Markdown 台本 → Remotion（2026-02） | 台本 DSL を軽くする発想 |
| SCPgamerscp/ymm4MCP | 27 | YMM4 を MCP サーバ化して LLM から操作（活発更新中） | GUI ツールを無理に自動化する方向。筋は悪い |
| sadakenji/VOICEVOX-Movie-Factory | 0 | JSON 台本 → VOICEVOX のモーラ時刻 → Remotion で字幕・立ち絵・音声。CLI 1コマンド、GUI 編集なし | **構造が本企画に最も近い**。モーラ時刻で口パクと字幕を同期する設計は③と同じ |
| tumf/movie-generator | 5 | ブログ URL → LLM → VOICEVOX → Remotion スライド | 記事→動画の型 |
| nyu1791-collab/voiceBox-Pipeline | 1 | GitHub Actions で VOICEVOX CPU 版を並列、MoviePy で MP4 | GPU なし CI の実例 |
| doubletaro/YukkuriGenerator | 1 | ローカル LLM + VOICEVOX + ComfyUI で投稿まで全自動 | 運用チャンネルの規模は未確認 |
| dikmri/zundamon-video ほか | 0〜1 | 「Claude Code スキル」として テーマ→調査→台本→TTS→口パク→字幕→mp4（2026-09〜10 に集中出現） | 本企画も「スキル」として切り出せば配布可能 |

所見:
- どのリポジトリも**視聴成果（登録者・再生数）を公開していない**。成功したチャンネルはパイプラインを公開しない選択バイアスがある【推定】。
- YMM4 は商用利用時に AquesTalk ライセンスが必要。YMM4 Lite は商用無償。
- 参考記事: Zenn「GPT-4 でゆっくり解説を自然言語指示だけで生成」、Zenn「Python だけで YouTube 動画を全自動生成（Gemini + COEIROINK + MoviePy + 自動アップロード）」、SpeakerDeck「Claude Code スキルで台本 CSV → VOICEVOX → YMM4 Lite」。

## B. AI 量産系（対比）

- 海外: Pictory / InVideo AI / Fliki / HeyGen / Synthesia。2025-07 の YPP 改定の対象の中心がこの利用者層。
- 国内: NoLang（テキスト→解説ショート）、Vrew（字幕自動化）。「台本→VTuber 風キャラ動画」を一気通貫で提供する国内 SaaS は確認できず。
- Google NotebookLM Video Overviews（2025-07）: 資料→AI 音声付き解説動画。「スライド＋AI 音声」型はプラットフォーマーがコモディティ化しつつあり、その型で差別化するのは難しい【推定】。
- データ: Kapwing 調査（2026-01）で新規アカウントの最初の推薦 500 本中 104 本（約21%）が AI スロップ、トップ 100 トレンドチャンネル中 278ch が全 AI、合計 630 億再生。→ 露出は取れていたが 2026 年に集中的に排除され始めた。
- 「AI vs 手作り」の統制比較データは**公開されていない**。得られるのは「音声の自然さ・テンポ・フックが維持率を決め、顔出しの有無は関係ない」という定性的コンセンサスのみ。「ロボ声は維持率 70% 低い」等の数字は業者ブログ由来で一次資料未確認。

## C. エンジン再生型

### AITuber 系スタック（流用価値あり、そのままは使えない）
- tegnike/aituber-kit（★1,129、TS/Next.js）: pixiv ChatVRM の系譜。VRM/Live2D、TTS は VOICEVOX / AivisSpeech / Style-Bert-VITS2 / ElevenLabs など。`speakCharacter.ts` に「前処理→エンジン選択→合成→キュー→口パク・表情」のパイプライン。口パクは音量ベース。
- shinshin86/aituber-onair（★292）: `@aituber-onair/voice` で感情対応 TTS を抽象化（VOICEVOX / AivisSpeech / OpenAI / MiniMax）。③の「TTS アダプタ」の参考。
- Neuro-sama（C#/Unity + Python）: 2026-01 に Twitch 最多サブ。国内は紡ネン（Pictoria、2024-11 登録者10万）。
- **AITuber スタックはリアルタイム対話に最適化されており、「台本を決定論的に再生して録画」には向かない**（音声キュー・非同期生成前提）。TTS 抽象化・VRM 口パク・表情制御のコードは流用できる。「配信からショートを自動生成」する専用 OSS は無し。

### エンジン / レンダラ駆動の動画生成
- **VrmVideoMaker**（BOOTH、無料）: YMM4 風 UI で VRM にセリフ・モーションを指定。VOICEVOX 連携、台本→音声＋口パク（RMS 解析）、複数キャラ、カメラワーク、透過 WebM 出力、AI 台本用プロンプト生成。→ ユーザー構想に最も近い既存プロダクトだが GUI ツールで無人バッチ不可。
- **avatarcam**（OpenClaw スキル）: 音声＋VRM→MP4 の CLI。Electron 描画、Linux は xvfb-run、canvas→WebM→FFmpeg。
- screenwriter / vidgen / avideo: ヘッドレス Chromium + CDP 仮想時間でフレーム単位撮影。「ブラウザを決定論的に録画する」パターン。
- Remotion 系（explainer-shorts-engine、remotion-explanation-generator、ai-voice-comic-maker）: 立ち絵＋字幕が中心。VRM＋Remotion は見当たらない。
- sugakushiki-pipeline: Manim + VOICEVOX + Claude + Gemini で数学史ドキュメンタリーを自動生成（2026-06〜）。「図版をコードで生成する」教育系の例。
- Unity Recorder: Editor 内限定、`-batchmode -nographics` では RenderTexture が黒くなる報告。無人化は Standalone で RenderTexture→ffmpeg を自作するのが現実的【推定】。
- 国内の「AI キャラ解説動画」SaaS は該当なし（Cotomo は音声会話アプリ、Tolgee は i18n ツールで無関係）。

## D. 成功と失敗を分ける要因（各ソースの定性的コンセンサス）

1. **台本の独自性・一次情報性**: 2026 年の YouTube の基準は「人間の創造的関与が見えるか」。ニュース・Wikipedia の読み上げやテンプレ構成は inauthentic 判定の典型。
2. **音声の自然さ**: 維持率の最大要因としてほぼ全ソースが挙げる。VOICEVOX は「ずんだもん」というキャラ文脈で許容されているが、無名キャラの合成音声は「AI 臭」として減点されやすい【推定】。Irodori-TTS の品質がそのまま成否に直結する。
3. **視覚的バリエーション**: 「同一立ち絵＋字幕」固定は量産感。カメラ・表情・モーション・背景が台本に連動して変わることが**エンジン再生型の本来の強み**。固定ショットだけならテンプレ型と同じリスク。
4. **ニッチ一貫性＋人間キュレーション**: 成功例（紡ネン、Neuro-sama、大手ずんだもん ch）はいずれも人格設計・企画・編集に人が深く関与。「全自動投稿」を公言するリポジトリで成果を示せたものは皆無。
5. **ジャンル制約（2026-07-16 YouTube 明確化）**: 収益化不可の3類型＝テンプレ的量産、感情操作狙い、**AI ペルソナが健康・金融・法律を語る動画**。「台本・編集に AI を使うこと自体は YPP 適格性に影響しない」と明言。IT・セキュリティ・資格は安全圏だが、応用情報ストラテジ系の「財務・法務」ユニットは**試験知識の説明にとどめ、助言の形にしない**。

## E. 3方式の比較（ソロ開発・毎日1本の教育 Shorts）

| | テンプレ型 | 生成動画型 | **エンジン再生型** |
|---|---|---|---|
| 制作コスト/本 | 最小。OSS 豊富 | API 費高・待ち・再現性なし | 初期構築は重いが限界コストはほぼゼロ |
| 決定論性・修正 | 高 | 低（再生成ガチャ） | 高（台本差し替えで再録） |
| 視覚の多様性 | 低 | 高（一貫性は崩れる） | 中〜高（台本駆動でカメラ・モーション・環境が変わる） |
| キャラ IP | 借り物（規約・飽和） | 不安定 | 自キャラ IP を蓄積。3D 資産を XR・配信・ゲームに流用 |
| inauthentic リスク | 高 | 中〜高 | 中（固定ショットのみならテンプレ型と同じ） |
| 先行事例 | 多 | 多（低品質） | ほぼ無し |
| 技術リスク | 低 | 低 | 無人レンダ、口パク精度、縦画面の 3D 演出設計 |

## F. 3D VRM と 2D（ゆっくり / ずんだもん）の受容
- 直接比較した調査データは無い。にじさんじ COO は「2D と 3D で視聴体験は大差なく、配信そのものが中心」。
- ずんだもん人気の要因は「可愛さ・声・不遇キャラ設定・素材の自由さ」で、形式ではなく**キャラ文脈の共有**。SSS 公式は 2025-08 にずんだもんのミニ 3D モデル（VRM/MMD）を無料配布し、3D 利用を公式に許容。
- 解説ジャンルで 3D VRM 単独キャラの成功チャンネルは特定できず。国内の解説 Shorts 視聴者は「見慣れたキャラ＋字幕＋テンポ」に最適化されており、無名 3D キャラは初期の認知コストが高い【推定】。
- 段階設計の案: 最初は正面固定ショット＋大きな板書で「テンプレ型と同じ見慣れた画」から入り、認知が付いてからカメラ・モーション・環境のバリエーションを増やす。あるいは規約内で「ずんだもん 3D VRM」を併用して入口にする。

## 出典
- GitHub: https://github.com/nyanko3141592/remotion-voicevox-template 、https://github.com/motemen/markdown-to-zundamon 、https://github.com/SCPgamerscp/ymm4MCP 、https://github.com/sadakenji/VOICEVOX-Movie-Factory 、https://github.com/tumf/movie-generator 、https://github.com/nyu1791-collab/voiceBox-Pipeline 、https://github.com/doubletaro/YukkuriGenerator 、https://github.com/dikmri/zundamon-video 、https://github.com/tegnike/aituber-kit 、https://github.com/shinshin86/aituber-onair 、https://github.com/kimjammer/Neuro 、https://github.com/sugakushiki/sugakushiki-pipeline 、https://github.com/rukataka0505/explainer-shorts-engine
- 記事: Zenn moons https://zenn.dev/moons/articles/6e07928a3eb159 、Zenn neuvecom https://zenn.dev/neuvecom/articles/f823265da90b7f 、motemen https://motemen.hatenablog.com/entry/2026/02/markdown-to-zundamon 、VrmVideoMaker https://booth.pm/ja/items/8600192 、YMM4 FAQ https://www.manjubox.net/ymm4/faq/etc/without-aquestalk 、ChatVRM https://gigazine.net/news/20230508-chatvrm/
- YouTube 政策: Plagiarism Today https://www.plagiarismtoday.com/2025/07/08/youtube-targets-inauthentic-content/ 、Tubefilter 16ch 停止 https://tubefilter.com/2026/01/29/youtube-ai-slop-channel-crackdown-bans/ 、TechCrunch 3類型 https://techcrunch.com/2026/07/20/youtube-clarifies-policies-around-ai-slop-and-upsetting-videos/ 、coconala https://coconala.com/blogs/5189435/608217 、アニメ!アニメ! https://animeanime.jp/release/dreamnews/20260525/288669.html
- データ: SlashGear Kapwing https://www.slashgear.com/2068701/youtube-video-ai-slop-study/ 、マネーポスト https://www.moneypost.jp/1158766 、Tubefilter Neuro-sama https://tubefilter.com/2026/01/05/neuro-sama-vedal987-most-subscribed-hype-train-record 、Forbes JAPAN https://forbesjapan.com/articles/detail/104485 、NotebookLM https://workspaceupdates.googleblog.com/2025/07/video-overviews-studio-panel-updates-notebooklm.html 、ITmedia https://www.itmedia.co.jp/news/spv/1804/23/news076_2.html 、でんふぁみ https://news.denfaminicogamer.jp/news/250822j
