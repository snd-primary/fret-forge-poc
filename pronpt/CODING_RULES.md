# 🎯 AIギター練習アプリ - コーディングルール & 開発フロー

## 📋 コーディングルール

## 基本方針

- **TypeScript**: `tsconfig.json`に準拠し、型安全性を最優先とする
- **Linter/Formatter**: `Biome`を使用し、`biome.json`設定に準拠する
- **パッケージマネージャ**: `pnpm`を使用する。`npm`コマンドの使用を禁止する
- **Git Hooks**: `husky` + `lint-staged`でコミット時の品質チェックを自動化
- **テスト**: TDD（テスト駆動開発）を採用し、`Vitest`でテスト実行

## 品質保証

### 自動チェック（コミット時）
```bash
# pre-commitで以下が自動実行される
1. Biome format --write     # フォーマット自動修正
2. Next.js lint --fix       # ESLint自動修正  
3. vitest related --run     # 関連テストの実行
4. tsc --noEmit             # 型チェック
```

### 手動実行コマンド
```bash
pnpm format          # フォーマット実行
pnpm lint:fix        # lint修正
pnpm type-check      # 型チェック
pnpm test            # 全テスト実行
pnpm test:watch      # テストウォッチモード
pnpm test:coverage   # カバレッジ付きテスト
```

### 1. ファイル・フォルダ命名規則

#### ディレクトリ構造（Features-based）
```
src/
├── app/                           # Next.js App Router（ルーティングのみ）
├── features/                      # 機能別ディレクトリ（メイン）
│   ├── practice-setup/           # 練習設定機能
│   ├── fretboard/               # 指板表示機能
│   ├── audio-processing/        # 音声処理機能
│   ├── practice-session/        # 練習セッション機能
│   └── feedback/                # フィードバック機能
├── shared/                       # 共通機能
│   ├── components/ui/           # shadcn/ui基本コンポーネント
│   ├── hooks/                   # 共通カスタムフック
│   ├── stores/                  # グローバル状態
│   ├── types/                   # 共通型定義
│   ├── utils/                   # 共通ユーティリティ
│   └── config/                  # 設定ファイル
└── __tests__/                   # 統合テスト・E2Eテスト
    ├── integration/             # 統合テスト
    ├── __mocks__/              # 共通モック
    └── setup/                  # テスト設定
```

#### 各featureディレクトリの構造
```
features/[feature-name]/
├── components/          # UIコンポーネント
├── hooks/              # カスタムフック
├── stores/             # 状態管理（Zustand）
├── types/              # 型定義
├── utils/              # ユーティリティ
└── __tests__/          # 単体テスト
    ├── components/
    ├── hooks/
    ├── stores/
    └── utils/
```

#### ファイル命名
- **コンポーネント**: PascalCase (`GuitarFretboard.tsx`)
- **フック**: camelCase + use接頭辞 (`useAudioInput.ts`)
- **ユーティリティ**: camelCase (`pitchDetector.ts`)
- **型定義**: PascalCase + Type接尾辞 (`AudioInputType.ts`)
- **テストファイル**: 対象ファイル名 + `.test.ts(x)` (`GuitarFretboard.test.tsx`)

### 2. TypeScript規則

#### 型定義
```typescript
// インターフェース: I接頭辞なし
interface AudioConfig {
  sampleRate: number;
  bufferSize: number;
}

// 型エイリアス: 具体的な名前
type PitchDetectionResult = {
  frequency: number;
  confidence: number;
  timestamp: number;
};

// Enum: PascalCase
enum CAGEDForm {
  C = 'C',
  A = 'A',
  G = 'G',
  E = 'E',
  D = 'D'
}
```

#### 関数・変数
```typescript
// 関数: 動詞から始める
const detectPitch = (audioBuffer: Float32Array): PitchDetectionResult => {};
const validateNote = (note: string): boolean => {};

// 変数: 名詞、boolean は is/has/can 接頭辞
const currentNote = 'C';
const isCorrectPitch = true;
const hasAudioPermission = false;
```

### 3. React コンポーネント規則

#### コンポーネント構造
```typescript
// 1. インポート（外部 → 内部の順）
import React from 'react';
import { Button } from '@/components/ui/button';
import { useAudioInput } from '@/hooks/useAudioInput';

// 2. 型定義
interface GuitarFretboardProps {
  currentNote?: string;
  onNoteSelect: (note: string) => void;
}

// 3. コンポーネント本体
export const GuitarFretboard: React.FC<GuitarFretboardProps> = ({
  currentNote,
  onNoteSelect
}) => {
  // フック
  const { isListening, startListening } = useAudioInput();
  
  // ローカル状態
  const [selectedFret, setSelectedFret] = useState<number | null>(null);
  
  // エフェクト
  useEffect(() => {
    // 処理
  }, []);
  
  // イベントハンドラ
  const handleFretClick = (fret: number) => {
    setSelectedFret(fret);
  };
  
  // レンダリング
  return (
    <div className="guitar-fretboard">
      {/* JSX */}
    </div>
  );
};
```

### 4. 音声処理・リアルタイム処理の特別ルール

#### パフォーマンス重視
```typescript
// ❌ 避けるべき: 音声処理内での重い処理
const processAudio = (buffer: Float32Array) => {
  const result = heavyCalculation(buffer); // NG: UIブロックの原因
  return result;
};

// ✅ 推奨: Web Workers or 軽量化
const processAudio = (buffer: Float32Array) => {
  return requestIdleCallback(() => {
    return lightweightPitchDetection(buffer);
  });
};
```

#### メモリリーク対策
```typescript
// ✅ 必須: cleanup処理
useEffect(() => {
  const audioContext = new AudioContext();
  const analyser = audioContext.createAnalyser();
  
  return () => {
    // 必ずcleanup
    audioContext.close();
    analyser.disconnect();
  };
}, []);
```

#### エラーハンドリング
```typescript
// ✅ 音声処理は必ずtry-catch
const detectPitch = async (buffer: Float32Array) => {
  try {
    return pitchfinder.YIN()(buffer);
  } catch (error) {
    console.error('Pitch detection failed:', error);
    return null; // graceful degradation
  }
};
```

### 5. CSS/Tailwind規則

#### クラス名順序
```typescript
// 1. レイアウト (display, position, etc.)
// 2. サイズ (width, height, padding, margin)
// 3. 外観 (color, background, border)
// 4. その他 (transition, animation)
<div className="flex flex-col w-full h-screen p-4 bg-gray-100 border rounded-lg transition-all duration-300">
```

#### カスタムCSS
```css
/* BEM記法を使用 */
.guitar-fretboard {
  /* ベース */
}

.guitar-fretboard__string {
  /* エレメント */
}

.guitar-fretboard__string--highlighted {
  /* モディファイア */
}
```

## 🔄 TDD開発フロー

### 1. Red-Green-Refactorサイクル

```bash
# 1. Red: 失敗するテストを書く
pnpm test:watch  # ウォッチモードで開始

# 2. Green: 最小限の実装でテストを通す
# 3. Refactor: コードを改善する
# 4. 繰り返し
```

### 2. Features-based TDD

```typescript
// 1. feature/[name]/types/ で型定義
// 2. feature/[name]/__tests__/ でテスト作成
// 3. feature/[name]/utils/ でロジック実装
// 4. feature/[name]/hooks/ でReactロジック
// 5. feature/[name]/components/ でUI実装
// 6. feature/[name]/stores/ で状態管理
```

### 3. テスト実行戦略

```bash
# 開発中: 関連テストのみ
pnpm test features/audio-processing

# コミット前: 全テスト
pnpm test

# CI: カバレッジ付き
pnpm test:coverage
```

### 2. テスト戦略

#### テストの種類と範囲
- **Unit Test (70%)**: 個別関数・コンポーネントのテスト
- **Integration Test (20%)**: コンポーネント間の連携テスト
- **E2E Test (10%)**: ユーザーシナリオのテスト

#### テスト命名規則
```typescript
// describe: 対象の説明
// it/test: 期待する動作
describe('PitchDetector', () => {
  describe('detectPitch', () => {
    it('should return correct frequency for A4 note', () => {
      // テスト内容
    });
    
    it('should handle empty audio buffer gracefully', () => {
      // テスト内容
    });
  });
});
```

### 3. Git フロー

#### ブランチ戦略
```
main
├── develop
│   ├── feature/SETUP-01-nextjs-init
│   ├── feature/AUDIO-01-web-audio-api
│   └── feature/UI-01-fretboard-component
└── hotfix/critical-bug-fix
```

#### コミットメッセージ
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Type:**
- `feat`: 新機能
- `fix`: バグ修正
- `test`: テスト追加・修正
- `refactor`: リファクタリング
- `docs`: ドキュメント更新
- `style`: コードフォーマット
- `chore`: ビルド・設定変更

**例:**
```
feat(audio): implement real-time pitch detection

- Add Pitchfinder integration
- Implement Web Audio API setup
- Add pitch to note conversion logic

Closes #AUDIO-01
```

### 4. コードレビュー基準

#### チェックポイント
- [ ] テストが適切に書かれているか
- [ ] 型安全性が保たれているか
- [ ] パフォーマンスに問題がないか
- [ ] アクセシビリティが考慮されているか
- [ ] コーディングルールに準拠しているか

## 🛠️ 開発ツール設定

### ESLint + Prettier
```json
// .eslintrc.json
{
  "extends": ["next/core-web-vitals", "@typescript-eslint/recommended"],
  "rules": {
    "prefer-const": "error",
    "no-unused-vars": "error"
  }
}
```

### Vitest設定
```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    setupFiles: ['./src/setupTests.ts'],
  },
});
```

---

## 📝 更新履歴
- 2024/XX/XX: 初版作成