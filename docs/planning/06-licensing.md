# 利用規約・ライセンスの整理

調査日: 2026-10-23。Hugging Face と issoh.co.jp の本文は調査環境から取得できず、検索スニペットと複数の二次情報を突き合わせた。**ローカルの `Irodori-TTS` フォルダ内の LICENSE と、各モデルカードの License 節で最終確認すること。**

## Irodori-TTS（Aratako 氏）

| 対象 | ライセンス | 商用 | 条件・注意 |
|---|---|---|---|
| コード（GitHub） | MIT | 可 | README は重みのライセンスを「v4.1-Small のモデルカード参照」としている |
| v4-Small / **v4.1-Small** / v4.1-Small-MF / Quantized の重み | **MIT**（複数の二次情報とミラーが一致） | 可 | 「ライセンスに加えて」倫理条項: 本人の明示的同意のない個人（声優・著名人・公人）の声の複製・なりすまし禁止、ディープフェイク・誤情報目的の合成禁止、テキストのみから生成した声が実在人物に偶然似る可能性の注意、開発者免責 |
| **v4-Large**（2026-09-27 公開、3.29B） | **Gemma 利用規約**（エンコーダが Google T5Gemma 2 由来） | 可（条件付き） | 禁止用途ポリシーの遵守、再配布時に規約の写しと禁止用途の通知が必要。MIT にはない義務がある。X 上の「Large も MIT」という投稿は小型版との混同の可能性が高い |
| 500M-v2 / v3 / VoiceDesign | MIT と紹介する記事が多い | 可 | 古い記述（フォーク README の「非商用」）は別リリースを指している可能性。使うなら個別に確認 |
| Semantic-DACVAE-Japanese-32dim（コーデック） | MIT（モデルカード）。派生元 Meta `dacvae-watermarked` は Apache-2.0 | 可 | カード内に「SAM License」の記述が混在するとの指摘あり。要確認 |
| 生成音声の透かし | README の謝辞に SilentCipher。自動透かしが入るとする記事あり | – | 透かしの有無は動画配信に影響しない。無効化はしない |

**本企画での結論**: 配信用の声は **v4.1-Small 系**で作る（MIT）。声は VoiceDesign か LoRA で**オリジナルの声**を作り、実在人物の参照音声は使わない。v4-Large を使う場合は Gemma 規約の通知義務を概要欄・リポジトリ README に書く。概要欄に「音声合成: Irodori-TTS」を固定で記載する（MIT は著作権表示の保持が条件なので、リポジトリに LICENSE を同梱）。

## 口パク・キャラ・モーション

| 対象 | ライセンス | 注意 |
|---|---|---|
| uLipSync（hecomi） | MIT | VRM 1.0 は `uLipSyncExpressionVRM`。BakedData を AnimationClip 化すれば配布物に uLipSync 本体は不要 |
| UniVRM | MIT | |
| VRoid Studio で作った自作 VRM | 自分のモデルは商用可。VRoid Studio 利用規約: 「出力物を変形・結合してアバターを生成出力するアプリの作成禁止（2023-12 改定）」は本企画には該当しない | VRoid Hub で配布されたモデルを使う場合は各モデルの利用条件（商用・改変・暴力表現等）に従う |
| VRMA（VRoid 公式 BOOTH 無料7種） | 配布ページの規約に従う | 商用可否を個別確認 |
| Mixamo → VRMA 変換 | Mixamo のアニメーションは Adobe 規約で商用利用可（再配布不可） | 変換ツール fbx2vrma-converter は MIT |
| 「ずんだもん」等の既存キャラを併用する場合 | VOICEVOX 音源は商用可・要クレジット「VOICEVOX:ずんだもん」。キャラ利用は SSS 合同会社のガイドライン | クレジット無し商用は1キャラ 40万円＋税 |

## エンジン・ツール

| 対象 | 条件 |
|---|---|
| Unity Personal | 直近12か月の売上・資金 $200K 未満（2025-01-01〜）。超えたら Pro |
| Unity Recorder | Editor 内のみ。ライセンス上の制約なし |
| Remotion（代替案） | 個人・従業員3人以下の営利・非営利は無料（商用動画制作可）。5.0 で契約社員も人数に算入予定 |
| three-vrm / inkjs / Ink | MIT |
| フォント | Noto Sans JP / M PLUS 1 は SIL OFL。動画への埋め込み可 |
| BGM・効果音 | 使用素材ごとに YouTube 収益化可否とクレジット要否を確認（Content ID 登録曲は避ける）。候補: DOVA-SYNDROME、効果音ラボ、魔王魂 |

## YouTube 側の表記

- 概要欄テンプレ（固定）: 「この動画は AI 音声合成（Irodori-TTS）と自作 3D キャラクターで制作しています。台本は人がレビューしています。出典: …」
- API 投稿時 `status.containsSyntheticMedia=true`。
- 実在人物の顔・声を模さない（Likeness detection の対象外に保つ）。

## 出典
- Irodori-TTS GitHub https://github.com/Aratako/Irodori-TTS 、LICENSE https://github.com/Aratako/Irodori-TTS/blob/main/LICENSE
- モデルカード（要直接確認） https://huggingface.co/Aratako/Irodori-TTS-v4.1-Small 、https://huggingface.co/Aratako/Irodori-TTS-v4.1-Small-MF 、v4-Small README https://huggingface.co/Aratako/Irodori-TTS-v4-Small/blob/main/README.md
- issoh「Irodori-TTS の使い方｜v4-Large と v4.1 の違い・絵文字一覧・商用利用【2026年10月】」 https://www.issoh.co.jp/tech/details/12221/
- note アイドリ「v4-Large を一次情報から読む。ライセンスは MIT ではない」 https://note.com/ai_driven/n/n67ceaf47d881
- pc-compass「v4-Large とは」 https://pc-compass.com/irodori-tts-v4-large-toha/ 、fixit https://fixit.co.jp/insights/irodori-tts/ 、Zenn acntechjp「無料・商用可」 https://zenn.dev/acntechjp/articles/530307d5e9c459 、oflight https://www.oflight.co.jp/en/columns/irodori-tts-v4-large-japanese-tts-voice-cloning-2026
- GGUF ミラー（倫理条項を転載） https://huggingface.co/sakasegawa/Irodori-TTS-v4.1-Small-GGUF 、派生 https://huggingface.co/phasefield-audio/Irodori-TTS-v4.1-Anime
- uLipSync https://github.com/hecomi/uLipSync 、UniVRM https://github.com/vrm-c/UniVRM 、VRoid 無料 VRMA https://vroid.com/en/news/6HozzBIV0KkcKf9dc1fZGW 、VOICEVOX 規約 https://wikiwiki.jp/voicevox/%E3%81%82%E3%82%93%E3%81%93%E3%82%82%E3%82%93
- Unity 料金 https://unity.com/products 、Remotion License https://github.com/remotion-dev/remotion/blob/main/LICENSE.md
