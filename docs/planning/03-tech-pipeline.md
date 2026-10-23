# ③ 技術構成

調査日: 2026-10-23。バージョン・ライセンス条文は実装前に原典で再確認する。

## 結論（2026-10-23 更新: uLipSync 採用・ローカル実行を前提に見直し）

前提の変更: 実装と実行はローカル（GPU 付き Windows 機）で行う。口パクは uLipSync を使う。Ink は Web でも動く（inkjs）。会社の WebXR 版（CypeAgenticAcademyWeb）は Unity 版と Web 版の両方があり、実装の参考にできる。

この前提では **Unity（ScenarioXR）を第一候補**にする。当初 Web を推した最大の理由「クラウドで Claude がレンダまで検証できる」が、ローカル実行ではそもそも要らない。残る論点は「無人レンダの組み方」と「板書 UI の作りやすさ」だけで、どちらも Unity で解ける。

| 判断軸 | Unity（ScenarioXR + uLipSync + Recorder） | Web（three-vrm + Remotion） |
|---|---|---|
| 既存資産 | ScenarioXR の Ink タグ・ExecutionRunner・UniVRM・VRMA がそのまま動く。KitchenXR の Unity CLI 運用経験 | 会社の Web 版の設計（タグ・TTS 組み込み・口パク）を参考にできるが、コードは持ち出せない |
| 口パク | **uLipSync（MFCC）で音声から直接**。アライメント不要。BakedData→AnimationClip でオフライン化も可 | 強制アライメントで timing.json を作る工程が必要 |
| 字幕の時刻 | 行単位で十分（セリフ全文を 2行表示）。各行の開始＝音声クリップの開始、終了＝クリップ長。アライメント不要 | 同左（Remotion captions は語単位も可） |
| 無人レンダ | Recorder は Editor の Play モード限定・batchmode 不可。**ログイン済みの Windows 機で Editor を CLI 起動（`-executeMethod`、`-batchmode` なし）→ Play → Recorder** で回す。タスクスケジューラで毎日実行 | 完全ヘッドレス・決定論的 |
| 板書・図版 | UI Toolkit で型4種のパネルを作る（KitchenXR で使用済み） | React |
| 音声同期 | Recorder は Unity 内蔵オーディオを `AudioRenderer` で取り込み、映像と同期した MP4 を出す。「Cap frame rate」で固定ステップにしても音声は正しく録れる | `<Audio>` を並べるだけ |
| 見た目 | URP・シェーダ・揺れ物を作り込める。MToon も UniVRM で可 | MToon 対応。細かい作り込みは Unity に劣る |
| ライセンス | Unity Personal（売上 $200K 未満）。uLipSync / UniVRM MIT | Remotion 個人無料、three-vrm MIT |

**Web を選び直す条件**: 会社の Web 版を確認した結果、three-vrm＋Ink＋Irodori-TTS の統合が既に成熟しており、口パク・字幕・進行制御をそのまま参考実装にできる場合。その場合は uLipSync の代わりに「音量＋母音推定」か強制アライメントが必要になる（uLipSync は Unity 専用）。

## パイプライン全体

```
[1] 企画      topic queue (YAML) ──► Claude ──► script.json（zod 検証、出典必須）
[2] 台本      script.json ──► Ink 生成（テンプレ）──► inkjs でコンパイル検証 ──► story.ink / story.json
[3] 展開      inkjs でストーリーを走査 ──► cue list（セリフ・タグ・板書の列）
[4] 音声      Irodori-TTS（Python, 48kHz wav）× セリフごと ──► voice/NN.wav（キャッシュ: sha256(text|voice)）
[5] 口パク    uLipSync で wav から BakedData（母音の時系列）を事前生成（Editor スクリプト。任意: 実行時解析でも可）
[6] タイムライン cue list + 各 wav の長さ ──► timeline.json（各 cue の開始フレーム、字幕ページ、板書状態、表情、モーション）
[7] レンダ    Unity Editor を CLI 起動 → Play → ScenarioEngine が story.json を再生 → Recorder が 1080x1920 30fps MP4 を出力
              （代替: Web 版 = Remotion + three-vrm）
[8] 検品      自動: 尺・無音・字幕はみ出し・セーフゾーン・音量（LUFS）。人: 10〜15分レビュー
[9] 投稿      YouTube Data API videos.insert（private + publishAt, containsSyntheticMedia=true）
[10] 計測     Analytics API（engagedViews, APV, viewed/swiped）──► topic queue の優先度に反映
```

中間成果物をすべてファイルに落とし、各段を再実行可能にする（音声を変えずに板書だけ直す、など）。

### [1][2] 台本: LLM は JSON を書き、Ink はテンプレートから生成する

- LLM に Ink を直接書かせると文法エラーとタグ語彙の逸脱が出る。**LLM は型（A: 1問1答 / B: 用語解説 / C: 比較 / D: 3秒で）ごとの JSON スキーマを埋める**。スキーマには `sources[]`（IPA 年度・問番号、CVE ID、URL）を必須にする。
- 変換器が JSON → Ink を生成する。Ink を残す理由は、(a) ScenarioXR と同じタグ体系で Unity 版とも互換、(b) 手書きの特別回や分岐（同じ問題の難易度違い、長尺用の拡張）を同じ形式で扱える、(c) `inkjs` の `Compiler` でコンパイル失敗＝検証失敗として機械的に弾ける。
- タグ語彙は ScenarioXR の `#category:target:action:params` を踏襲し、Shorts 用に追加する。

| タグ | 意味 | 由来 |
|---|---|---|
| `#anim:<actor>:<motion>` `#expression:<actor>:<expr>` | モーション・表情 | ScenarioXR |
| `#voice:<actor>:<clip>` | 音声（生成時は自動付与） | ScenarioXR |
| `#series:<id>:<n>` | シリーズと連番（タイトル・説明欄にも使う） | 新規 |
| `#board:title|question|choices|answer|notes|table|figure:...` | 板書の状態 | 新規 |
| `#count:<sec>` | 無音カウント | 新規 |
| `#sfx:<name>` `#bgm:<name>` | 効果音・BGM | 新規 |
| `#camera:<preset>` | カメラ（bust / close / wide） | 新規 |

### [4] 音声: Irodori-TTS の扱い

- Irodori-TTS は Aratako 氏の日本語 TTS（RF-DiT、48kHz、絵文字で感情制御、ゼロショット声クローン、LoRA）。最新は `Irodori-TTS-v4.1-Small`。コードは MIT。
- **重みのライセンスは要確認**（「非商用」の転載あり）。収益化前に確定させる。NG なら LoRA で自作声を作るか、VOICEVOX 系（商用可・要クレジット）へ切替できるように、音声段を **TTS アダプタ**で抽象化しておく。
- **音素・モーラのタイムスタンプは出さない**（duration predictor と `--seconds` のみ）。したがって口パクと字幕時刻は [5] で作る。
- 推論は Python。OpenAI 互換サーバを常駐させて HTTP で呼ぶか、`uv run` のサブプロセス。AI-Manor の `voice.py` にある「エンジン起動・停止」「wav キャッシュ（sha256(text|speaker)）」「口形予定表を wav の隣に .json で保存」の機構はそのまま流用できる。

### [5] 口パク: uLipSync を使う（代替: 強制アライメント）

uLipSync（MIT）は MFCC で音声から `aa/ih/ou/ee/oh` を推定する。VRM 1.0 は `uLipSyncExpressionVRM` で `VRM10ObjectExpression` に直結。オフライン化は BakedData（wav → 母音時系列）を Editor で事前生成し、AnimationClip か Timeline の uLipSync トラックで再生する。キャラの声でキャリブレーション（母音ごとの MFCC プロファイル登録）を一度やれば精度が安定する。

字幕は行単位で出す（セリフ全文、13字×2行）。開始時刻＝音声クリップの再生開始、終了＝クリップ長なので、音素アライメントは不要。語単位のカラオケ風強調をやりたくなった場合だけ、以下の強制アライメントを足す。

| 方法 | 評価 |
|---|---|
| **pydomino**（Dwango、CTC、CPU 可、日本語音素専用） | 語単位字幕が必要になったら。pyopenjtalk で G2P → 音素境界 |
| Qwen3-ForcedAligner-0.6B | 日本語対応、80ms 分解能。精度比較用 |
| Montreal Forced Aligner 3.0 | 日本語 CSJ で境界誤差 <15ms。環境構築が重い |
| stable-ts `align()` | whisper ベースの代替 |
| 音量ベース | 「あ」しか動かない。フォールバック |

強制アライメントを使う場合の出力 `timing.json` は `{mora, vowel, start, end}` の配列。AI-Manor の `face_speech.build_cues`（VOICEVOX の audio_query からモーラ時刻→口形）が設計の雛形。

### [7] レンダ（代替案の詳細）: Remotion + three-vrm

- `@remotion/three` の `<ThreeCanvas>` 内で `@pixiv/three-vrm`（3.5.x、VRM 0.x/1.0）と `@pixiv/three-vrm-animation`（VRMA）を使う。**`useFrame`/`clock`/`requestAnimationFrame` は使わず、`useCurrentFrame()` から `mixer.setTime(frame/fps)` と `expressionManager.setValue()` を毎フレーム決定論的に設定**する。
- VRMA は VRoid 公式 BOOTH の無料7種、Mixamo→VRMA 変換（`fbx2vrma-converter`）。必要モーションは talk / idle / point / think / happy / nod の6種程度。
- 字幕は `@remotion/captions`。空白区切り前提なので日本語は [5] の文節で自前ページ分割。縁取りは `-webkit-text-stroke` と多重 `text-shadow`、フォントは Noto Sans JP / M PLUS 1 の Bold。
- GL: ローカルは `--gl=angle`、GPU 付きクラウドは `angle-egl`、GPU なし CI は `swangle`（SwiftShader、CPU。VRM 1体で数 fps 程度の想定、60秒クリップで 10〜30分）。`npx remotion benchmark` で実測。長尺はメモリリーク対策で分割レンダ。
- 音声は `<Audio>` を timeline の開始フレームに並べるだけで ffmpeg ミックスされる。

### [9] 投稿: YouTube Data API

| 項目 | 内容 |
|---|---|
| コスト | `videos.insert` は 1,600 ユニット。既定クォータ 10,000/日 → 6本/日まで。増枠は Compliance Audit |
| OAuth | `youtube.upload` スコープ。**未審査プロジェクトからのアップロードは強制 private**。自動公開には API 審査が必要なので最初に申請する（律速） |
| 予約 | `status.privacyStatus=private` ＋ `status.publishAt`（ISO 8601）。未公開動画のみ可 |
| AI 開示 | `status.containsSyntheticMedia=true`（2024-10-30 追加）。安全側で true |
| Shorts 判定 | 縦 9:16 かつ 3分以内で自動。`#Shorts` は任意。サムネイル不要 |

## Unity（ScenarioXR）での設計（第一候補）

ScenarioXR の `InkBridge → ExecutionRunner → Scene/Actor` はそのまま使える。足すのは以下。

1. **録画は 2段構え**。
   - **第1案（まずこれ）: Editor + Recorder を CLI から起動**。`Unity.exe -projectPath . -executeMethod EduCast.Editor.ShortsRecorder.Run -episode NN`（`-batchmode` を付けない。Recorder は batchmode で動かない）。エディタスクリプトが `RecorderController` を設定 → `EditorApplication.EnterPlaymode` → ScenarioEngine が story.json を再生 → 終了タグで Recorder 停止 → Editor 終了。Recorder は Unity 内蔵オーディオを取り込むので uLipSync の実行時解析と音声が同期した MP4 が出る。KitchenXR の `editor-layout-backup.ps1` と同じくレイアウト退避が要る。
   - **第2案（第1案が不安定なら）: Standalone プレイヤーをレンダラにする**。`ShortsRenderer.exe --scenario story.json --timeline timeline.json --out NN.mp4`。`Time.captureFramerate = 30` で固定ステップにし、RenderTexture を読み出して ffmpeg にパイプ（keijiro/FFmpegOut 相当）。音声はエンジン内で鳴らさず timeline.json の開始フレームから ffmpeg で後合成する。この場合 uLipSync は BakedData を使う（実時間の AudioSource に依存しないため）。
2. **板書 UI**: UI Toolkit（KitchenXR で使用済み）でテンプレ4型分のパネルを作る。
3. **口パク**: uLipSync。第1案なら実行時解析、第2案なら BakedData → AnimationClip。`ExpressionController` が `aa/ih/ou/ee/oh` を受け取る口を1つ用意しておけば、将来 timing.json 駆動にも差し替えられる。
4. **運用**: Windows 機のタスクスケジューラから起動。GitHub Actions は不可（GPU・ディスプレイなし、Personal ライセンスの手動認証廃止）。

ScenarioXR への追加タグ（`#series` `#board` `#count` `#sfx` `#camera`）は `ExecutionTagParser.Register` に Factory を足すだけで済む。`#wait_event:<actor>.voice.complete` の仕組みはそのまま字幕の切り替えにも使える。

## 長尺への展開

同じ timeline.json から 16:9 のレイアウト変種（板書を左、キャラを右）をレンダすれば、Shorts 5本を 10分の講義にまとめられる。章間に `#board:title` のカードを挟むだけ。8,000 時間要件と長尺 RPM（①参照）の両方に効くので、最初からレイアウトを 2 種持つ前提で composition を設計する。

## リポジトリ構成（案）

```
edu-cast/
  packages/
    script/     スキーマ（zod）、型テンプレ、JSON→Ink 変換、inkjs 検証
    voice/      TTS アダプタ（irodori / voicevox）、キャッシュ
    align/      Python: pyopenjtalk + pydomino → timing.json
    timeline/   cue list + timing → timeline.json
    render/     Remotion project（compositions: shorts-9x16, lecture-16x9）
    publish/    YouTube upload / analytics
  content/
    topics.yaml   企画キュー（レーン、ユニット、優先度、出典）
    episodes/NN/  script.json, story.ink, voice/, timing.json, timeline.json, out.mp4
  assets/
    vrm/ vrma/ fonts/ sfx/ bgm/
  apps/cli        edu-cast make <topic> | render NN | publish NN
```

## 最初の2週間で検証すること

1. Remotion `template-three` に VRM を読み、`useCurrentFrame` で VRMA と `aa` を駆動して 10秒レンダ。`--gl=angle` と `swangle` の両方で時間を計測。
2. Irodori-TTS の出力 + pydomino で母音時刻を取り、口の開閉が自然か目視確認。VOICEVOX（モーラ時刻つき）との比較。
3. Irodori-TTS 重みの商用可否をモデルカードで確定。
4. YouTube API プロジェクトの審査申請（待ち時間が最長）。
5. 板書4型（1問1答・用語・比較・3秒で）の React コンポーネントを作り、セーフゾーン内に収まることを実機で確認。

## 出典
- Irodori-TTS https://github.com/Aratako/Irodori-TTS 、モデルカード https://huggingface.co/Aratako/Irodori-TTS-500M-v3
- three-vrm https://github.com/pixiv/three-vrm 、VRMA https://vrm.dev/en/vrma/ 、fbx2vrma https://cdn.jsdelivr.net/npm/fbx2vrma-converter@1.0.2/README.md 、VRoid 無料 VRMA https://vroid.com/en/news/6HozzBIV0KkcKf9dc1fZGW
- Remotion three https://remotion.dev/docs/three 、GL options https://www.remotion.dev/docs/gl-options 、GPU https://www.remotion.dev/docs/gpu 、captions https://convert.remotion.dev/docs/captions/caption 、License https://github.com/remotion-dev/remotion/blob/main/LICENSE.md 、template-three https://github.com/remotion-dev/template-three
- Unity Recorder Known Issues https://docs.unity3d.com/Packages/com.unity.recorder@5.1/manual/KnownIssues.html 、CLI 録画 https://docs.unity3d.com/Packages/com.unity.recorder@5.0/manual/CommandLineRecorder.html 、FFmpegOut https://github.com/keijiro/FFmpegOut/releases 、uLipSync https://github.com/hecomi/uLipSync 、UniVRM https://github.com/vrm-c/UniVRM
- GameCI 認証変更 https://discussions.unity.com/t/unity-no-longer-supports-manual-activation-of-personal-licenses/926760 、Unity 料金 https://unity.com/products
- pydomino https://dmv.nico/en/articles/domino_phoneme_transition/ 、Qwen3-ForcedAligner https://huggingface.co/Qwen/Qwen3-ForcedAligner-0.6B-hf 、MFA 3.0 https://papers.cool/arxiv/2606.18466 、stable-ts https://pypi.org/project/stable-ts/
- inkjs https://github.com/inkle/inkjs 、Ink https://inklestudios.com/ink/
- YouTube quota https://developers.google.com/youtube/v3/determine_quota_cost 、videos.insert https://developers.google.com/youtube/v3/docs/videos/insert 、revision history（containsSyntheticMedia） https://developers.google.com/youtube/v3/revision_history 、監査 https://developers.google.com/youtube/data/guides/quota_and_compliance_audits
- 先行実装: remotion-voicevox-template https://github.com/nyanko3141592/remotion-voicevox-template 、VOICEVOX-Movie-Factory https://github.com/sadakenji/VOICEVOX-Movie-Factory 、AITuberKit https://github.com/tegnike/aituber-kit
- クラウド GPU 相場 https://tech-insider.org/runpod-vs-lambda-vs-vast-ai-2026/ 、GitHub Actions ランナー https://docs.github.com/en/actions/reference/runners/github-hosted-runners
