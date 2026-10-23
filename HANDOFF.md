# HANDOFF: edu-cast 企画フェーズ → 実装フェーズ

作成: 2026-10-23（クラウドセッション `session_011oANTW3WdK1Bz1s3WuiXQR` から引き継ぎ）
ブランチ: `claude/peaceful-einstein-3vhjj6`（main 未マージ。調査ドキュメントはすべてここにある）

## 1. このプロジェクトは何か

AI 駆動で教育系 YouTube Shorts を自動生成する。LLM が台本（Ink）を書き、Irodori-TTS が声を作り、VRM キャラをエンジン（Unity / ScenarioXR）で再生して録画し、YouTube に投稿する。**動画生成 AI は使わない。「エンジンでシナリオを再生 → 動画化」が方針**。

参考にするチャンネル像: あきとんとん（数学）、英単語ショート、ずんだもん IT 解説。

## 2. 決まっていること（詳細は docs/planning/）

| 論点 | 決定 | 根拠の場所 |
|---|---|---|
| ニッチ | 「情報系の学び」1チャンネル。分けない | 01 |
| レーン | A 体系 60%（iパス/SG レベルと応用情報レベルの2段）/ B 時事 20%（ホットトピックは優先）/ C AI 用語 20% | 01 |
| 2027 年の機会 | IPA 新試験制度（応用情報は 2027-02 が最終回、「プロフェッショナルデジタルスキル」「データマネジメント」新設）。タイトルは試験名でなくトピック名 | 01 |
| コアペルソナ | 非IT企業の 20代後半〜30代、iパス/SG 受験者。隣接は B2B 研修とパイプライン販売 | 01 |
| 幅の線引き | 「新試験シラバスの単元に対応づく」か「コアペルソナが明日仕事で使う」のどちらか | 01 |
| 投稿本数 | 立ち上げ 8〜12 週は 1日2本（朝 7〜8時＝浅い体系、夜 21時＝時事優先・無ければ深い体系）。以後 1日1本＋長尺週1 | 01 |
| 収益 | Shorts 広告は当てにしない（RPM 10〜30円/1,000回、2027-02 から新規 YPP は Shorts 2,000万回/90日）。長尺自動合成・参考書アフィリエイト・自作問題集・B2B の三層 | 01 |
| 尺と型 | 30秒「1問1答」と 45秒「用語解説」から開始。60秒「比較」、15秒「3秒で」は実験枠 | 02 |
| 冒頭 | 挨拶・自己紹介・目次型を禁止。0秒から用語ドン＋フック。決め台詞は末尾 | 02 |
| 字幕・画面 | セリフ全文、13字×2行、中央帯。キャラは下段左。右 192px・下 672px は UI に隠れる | 02 |
| 話速 | TTS 6〜7文字/秒。30秒＝140〜170字 | 02 |
| レンダラ | **Unity（ScenarioXR + uLipSync + Recorder）が第一候補**。Web（Remotion + three-vrm）は代替 | 03 |
| 台本 | LLM は型別 JSON（出典必須）→ テンプレで Ink 生成 → コンパイル検証。タグは ScenarioXR 互換（`#anim` `#expression` `#voice` `#wait_event`）＋追加（`#series` `#board` `#count` `#sfx` `#camera`） | 02, 03 |
| 口パク・字幕 | 口パクは uLipSync。字幕は行単位（クリップの開始・長さ）でアライメント不要 | 03 |
| 録画 | 第1案: Editor を CLI 起動（`-batchmode` なし）→ Play → Recorder。第2案: Standalone + `Time.captureFramerate` + ffmpeg パイプ | 03 |
| 投稿 | YouTube Data API。private + publishAt、`containsSyntheticMedia=true`。API 審査を最初に申請 | 03 |
| TTS ライセンス | Irodori-TTS v4.1-Small 系は MIT＋倫理条項（二次情報）。v4-Large は Gemma 規約。配信用は v4.1-Small でオリジナル声 | 06 |

## 3. ローカル環境の情報（ユーザーから）

- Irodori-TTS: `C:\Users\machi\Documents\Claude Workspace\Tools\Irodori-TTS`
- 会社の WebXR 版（Unity 版と Web 版の両方がある）のコピー: `C:\Users\machi\Downloads\cypeagenticacademy-web`。Ink シナリオ・Irodori-TTS 組み込み・キャラ制御の実装参考。**会社のコードなので、このリポジトリにはコピーしない。設計メモだけ `docs/reference/` に残す**
- 個人リポジトリ: `kazuhiro1999/ScenarioXR`（Unity 6000.0.70f1、UniVRM 0.130.1、UniTask、Ink。`Assets/ScenarioEngine/` に InkBridge / ExecutionRunner / ExecutionTagParser / AvatarActor / VoiceExecution 等）、`kazuhiro1999/KitchenXR`（Unity CLI・バッチ運用の例、UI Toolkit）、`kazuhiro1999/AI-Manor`（`src/manor/voice.py` の TTS エンジン起動・wav キャッシュ、`face_speech.py` のモーラ時刻→口形の雛形）、`kazuhiro1999/VRoidStudio-MCP`（VRoid 自動化の調査）
- 実装・実行はローカル（GPU 付き Windows）。クラウドは調査にのみ使った

## 4. 未確認・要判断

1. **Irodori-TTS の重みライセンス**を `Irodori-TTS` フォルダの LICENSE と Hugging Face モデルカード（`Aratako/Irodori-TTS-v4.1-Small`）で最終確認する（クラウドからは本文を取得できなかった）
2. 会社の Web 版の設計（Ink ランタイム、タグ仕様、TTS 組み込み方、口パク、進行制御、フレーム決定論の可否）を読み、Unity 第一候補のままで良いか判断する
3. キャラ（VRM）をどうするか: 自作 VRoid でオリジナルを作る前提。モーション 6 種（talk / idle / point / think / happy / nod）、表情 5 種
4. YouTube の数値はほぼ第三者推計。自チャンネルの Analytics で上書きする

## 5. 次の作業（ロードマップ フェーズ 0、2 週間）

1. `cypeagenticacademy-web` を読み、設計メモを `docs/reference/cype-web-notes.md` に書く（Unity 版との差分も）
2. ScenarioXR を 1080×1920 の縦カメラにし、CLI 起動 → Play → Recorder で 10 秒の MP4 が無人で出ることを確認。所要時間を計測
3. Irodori-TTS v4.1-Small で台本 3 行を合成 → uLipSync をキャリブレーション → 口の開閉を目視確認
4. Irodori-TTS のライセンス確定（§4-1）
5. YouTube API プロジェクト作成・OAuth・審査申請（待ち時間が最長）
6. 板書 4 型（1問1答・用語・比較・3秒で）を UI Toolkit で作り、セーフゾーン内に収まることを実機確認
7. 台本 JSON スキーマ → Ink 生成 → コンパイル検証。A 型・B 型の台本 3 本ずつが通ること

リポジトリ構成案は `docs/planning/03-tech-pipeline.md` の「リポジトリ構成（案）」。

## 6. 新しいローカルセッションの最初のメッセージ（貼り付け用）

```
このリポジトリ（edu-cast）はブランチ claude/peaceful-einstein-3vhjj6 に企画ドキュメントがある。
まず HANDOFF.md を読み、次に docs/planning/README.md から各章を読んで前提を把握して。
そのうえでフェーズ 0 の作業 1 から始めたい。
C:\Users\machi\Downloads\cypeagenticacademy-web（会社の Web 版のコピー。コードはこのリポジトリに持ち込まない）を読んで、
Ink の仕組み・タグ仕様・Irodori-TTS の組み込み・口パク・進行制御・フレーム単位の決定論的実行の可否を
docs/reference/cype-web-notes.md にまとめて。Unity 版（kazuhiro1999/ScenarioXR）との差分と、
本プロジェクトで Unity を第一候補にしたままで良いかの判断も書いて。
Irodori-TTS は C:\Users\machi\Documents\Claude Workspace\Tools\Irodori-TTS にある。
LICENSE とモデルカードで重みの商用可否も確認して docs/planning/06-licensing.md を更新して。
```
