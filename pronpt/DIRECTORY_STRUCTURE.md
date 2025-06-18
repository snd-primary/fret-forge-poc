# 🏗️ ディレクトリ構成設計

## 📁 Features-based + TDD対応 ディレクトリ構造

```
fret-forge-poc/
├── src/
│   ├── app/                           # Next.js App Router
│   │   ├── (practice)/               # ルートグループ: 練習関連
│   │   │   ├── setup/               # 練習設定画面
│   │   │   │   └── page.tsx
│   │   │   └── session/             # 練習セッション画面
│   │   │       └── page.tsx
│   │   ├── globals.css              # グローバルスタイル
│   │   ├── layout.tsx               # ルートレイアウト
│   │   └── page.tsx                 # ホーム画面
│   │
│   ├── features/                     # 機能別ディレクトリ（メイン）
│   │   ├── practice-setup/          # 練習設定機能
│   │   │   ├── components/          # UI コンポーネント
│   │   │   │   ├── KeySelector.tsx
│   │   │   │   ├── FormSelector.tsx
│   │   │   │   └── StartButton.tsx
│   │   │   ├── hooks/               # カスタムフック
│   │   │   │   └── usePracticeSetup.ts
│   │   │   ├── stores/              # 状態管理
│   │   │   │   └── practiceSetupStore.ts
│   │   │   ├── types/               # 型定義
│   │   │   │   └── practiceSetup.types.ts
│   │   │   ├── utils/               # ユーティリティ
│   │   │   │   └── cageFormData.ts
│   │   │   └── __tests__/           # テスト
│   │   │       ├── components/
│   │   │       │   ├── KeySelector.test.tsx
│   │   │       │   ├── FormSelector.test.tsx
│   │   │       │   └── StartButton.test.tsx
│   │   │       ├── hooks/
│   │   │       │   └── usePracticeSetup.test.ts
│   │   │       ├── stores/
│   │   │       │   └── practiceSetupStore.test.ts
│   │   │       └── utils/
│   │   │           └── cageFormData.test.ts
│   │   │
│   │   ├── fretboard/               # 指板表示機能
│   │   │   ├── components/
│   │   │   │   ├── GuitarFretboard.tsx
│   │   │   │   ├── FretPosition.tsx
│   │   │   │   ├── StringLine.tsx
│   │   │   │   └── NoteMarker.tsx
│   │   │   ├── hooks/
│   │   │   │   ├── useFretboardState.ts
│   │   │   │   └── useFretboardInteraction.ts
│   │   │   ├── stores/
│   │   │   │   └── fretboardStore.ts
│   │   │   ├── types/
│   │   │   │   └── fretboard.types.ts
│   │   │   ├── utils/
│   │   │   │   ├── fretCalculations.ts
│   │   │   │   └── notePositions.ts
│   │   │   └── __tests__/
│   │   │       ├── components/
│   │   │       ├── hooks/
│   │   │       ├── stores/
│   │   │       └── utils/
│   │   │
│   │   ├── audio-processing/        # 音声処理機能
│   │   │   ├── components/
│   │   │   │   ├── AudioPermissionRequest.tsx
│   │   │   │   ├── AudioStatusIndicator.tsx
│   │   │   │   └── PitchDisplay.tsx
│   │   │   ├── hooks/
│   │   │   │   ├── useAudioInput.ts
│   │   │   │   ├── usePitchDetection.ts
│   │   │   │   └── useAudioPermission.ts
│   │   │   ├── stores/
│   │   │   │   └── audioStore.ts
│   │   │   ├── types/
│   │   │   │   └── audio.types.ts
│   │   │   ├── utils/
│   │   │   │   ├── pitchDetector.ts
│   │   │   │   ├── frequencyToNote.ts
│   │   │   │   ├── audioContext.ts
│   │   │   │   └── silenceDetector.ts
│   │   │   └── __tests__/
│   │   │       ├── components/
│   │   │       ├── hooks/
│   │   │       ├── stores/
│   │   │       └── utils/
│   │   │
│   │   ├── practice-session/        # 練習セッション機能
│   │   │   ├── components/
│   │   │   │   ├── PracticeScreen.tsx
│   │   │   │   ├── ChallengeDisplay.tsx
│   │   │   │   ├── FeedbackOverlay.tsx
│   │   │   │   └── ProgressIndicator.tsx
│   │   │   ├── hooks/
│   │   │   │   ├── usePracticeSession.ts
│   │   │   │   ├── useChallengeGeneration.ts
│   │   │   │   └── useScoreJudgment.ts
│   │   │   ├── stores/
│   │   │   │   └── practiceSessionStore.ts
│   │   │   ├── types/
│   │   │   │   └── practiceSession.types.ts
│   │   │   ├── utils/
│   │   │   │   ├── challengeGenerator.ts
│   │   │   │   ├── scoreJudgment.ts
│   │   │   │   └── feedbackLogic.ts
│   │   │   └── __tests__/
│   │   │       ├── components/
│   │   │       ├── hooks/
│   │   │       ├── stores/
│   │   │       └── utils/
│   │   │
│   │   └── feedback/                # フィードバック機能
│   │       ├── components/
│   │       │   ├── SuccessFeedback.tsx
│   │       │   ├── ErrorFeedback.tsx
│   │       │   └── VisualEffects.tsx
│   │       ├── hooks/
│   │       │   ├── useFeedbackAnimation.ts
│   │       │   └── useFeedbackSound.ts
│   │       ├── stores/
│   │       │   └── feedbackStore.ts
│   │       ├── types/
│   │       │   └── feedback.types.ts
│   │       ├── utils/
│   │       │   ├── animationHelpers.ts
│   │       │   └── feedbackTiming.ts
│   │       └── __tests__/
│   │           ├── components/
│   │           ├── hooks/
│   │           ├── stores/
│   │           └── utils/
│   │
│   ├── shared/                      # 共通機能
│   │   ├── components/              # 共通UIコンポーネント
│   │   │   ├── ui/                 # shadcn/ui基本コンポーネント
│   │   │   │   ├── button.tsx
│   │   │   │   ├── card.tsx
│   │   │   │   ├── select.tsx
│   │   │   │   └── badge.tsx
│   │   │   └── layout/             # レイアウトコンポーネント
│   │   │       ├── Header.tsx
│   │   │       ├── Footer.tsx
│   │   │       └── Container.tsx
│   │   ├── hooks/                  # 共通カスタムフック
│   │   │   ├── useLocalStorage.ts
│   │   │   ├── useDebounce.ts
│   │   │   └── useMediaQuery.ts
│   │   ├── stores/                 # グローバル状態
│   │   │   └── globalStore.ts
│   │   ├── types/                  # 共通型定義
│   │   │   ├── common.types.ts
│   │   │   ├── music.types.ts
│   │   │   └── api.types.ts
│   │   ├── utils/                  # 共通ユーティリティ
│   │   │   ├── constants.ts
│   │   │   ├── helpers.ts
│   │   │   └── validators.ts
│   │   ├── config/                 # 設定ファイル
│   │   │   ├── app.config.ts
│   │   │   └── audio.config.ts
│   │   └── __tests__/              # 共通機能テスト
│   │       ├── components/
│   │       ├── hooks/
│   │       ├── stores/
│   │       └── utils/
│   │
│   └── __tests__/                  # 統合テスト・E2Eテスト
│       ├── integration/            # 統合テスト
│       │   ├── practice-flow.test.tsx
│       │   ├── audio-feedback-integration.test.tsx
│       │   └── state-synchronization.test.tsx
│       ├── e2e/                    # E2Eテスト（将来的に）
│       │   └── practice-session.spec.ts
│       ├── __mocks__/              # モックファイル
│       │   ├── audioContext.mock.ts
│       │   ├── pitchfinder.mock.ts
│       │   └── mediaDevices.mock.ts
│       └── setup/                  # テスト設定
│           ├── setupTests.ts
│           ├── testUtils.tsx
│           └── customMatchers.ts
│
├── public/                         # 静的ファイル
│   ├── audio/                     # 音声ファイル（フィードバック音など）
│   └── images/                    # 画像ファイル
│
├── docs/                          # ドキュメント
│   ├── api/                      # API仕様
│   ├── architecture/             # アーキテクチャ設計
│   └── development/              # 開発ガイド
│
├── vitest.config.ts              # Vitest設定
├── tailwind.config.ts            # Tailwind設定
├── tsconfig.json                 # TypeScript設定
├── next.config.js                # Next.js設定
├── package.json                  # 依存関係
├── CODING_RULES.md              # コーディングルール
├── DIRECTORY_STRUCTURE.md       # このファイル
├── todo.md                      # TODOリスト
├── rdd.md                       # 要求定義書
└── poc-tech-stack.md            # 技術スタック
```

## 🎯 設計思想

### 1. Features-based アーキテクチャ
- **機能単位での完全な分離**: 各featureは独立して開発・テスト可能
- **コロケーション**: 関連するファイル（コンポーネント、フック、ストア、テスト）を同じディレクトリに配置
- **スケーラビリティ**: 新機能追加時は新しいfeatureディレクトリを作成するだけ

### 2. TDD最適化
- **テストファイルの近接配置**: 各featureディレクトリ内に`__tests__`を配置
- **テスト種別の明確な分離**: unit/integration/e2eの明確な分離
- **モック管理**: 共通モックは`__tests__/__mocks__`に集約

### 3. 責務の明確化
- **shared/**: 複数のfeatureで使用される共通機能
- **features/**: ビジネスロジックに特化した機能
- **app/**: ルーティングとページレイアウトのみ

## 🔄 開発フロー例

### 新機能開発時
1. `features/新機能名/` ディレクトリ作成
2. `types/` で型定義
3. `__tests__/` でテスト作成（TDD）
4. `utils/` でロジック実装
5. `hooks/` でReactロジック実装
6. `components/` でUI実装
7. `stores/` で状態管理実装

### テスト実行
```bash
# 特定のfeatureのテスト
npm test features/audio-processing

# 統合テスト
npm test integration

# 全テスト
npm test
```

## 📝 命名規則

### ディレクトリ
- **features**: kebab-case (`audio-processing`, `practice-setup`)
- **components**: PascalCase (`GuitarFretboard`, `AudioStatusIndicator`)

### ファイル
- **コンポーネント**: PascalCase (`GuitarFretboard.tsx`)
- **フック**: camelCase + use接頭辞 (`usePitchDetection.ts`)
- **ストア**: camelCase + Store接尾辞 (`audioStore.ts`)
- **型定義**: camelCase + .types接尾辞 (`audio.types.ts`)
- **テスト**: 対象ファイル名 + .test (`GuitarFretboard.test.tsx`)

---

この構成についてどう思いますか？特に調整したい部分や追加したい観点はありますか？