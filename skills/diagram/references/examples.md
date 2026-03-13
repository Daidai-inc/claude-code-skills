# 図解サンプル集

良い出力が生成されたら、ここに追加して品質を継続的に向上させる。

## Example 1: タイムライン（ロードマップ）

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:wght@400" />
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;700&display=swap" />
  <script>
    tailwind.config = {
      theme: { extend: { fontFamily: { sans: ['Noto Sans JP', 'sans-serif'] } } }
    }
  </script>
</head>
<body class="w-[1280px] h-[720px] m-0 p-0 overflow-hidden font-sans">
  <div class="w-full h-full bg-slate-900 flex flex-col items-center justify-center p-12 text-white">
    <h1 class="text-3xl font-bold mb-2">AXIS ロードマップ 2026</h1>
    <p class="text-slate-400 mb-8">大会運営SaaS → JTBバイアウトへの道筋</p>
    <div class="flex items-start gap-4 w-full max-w-5xl">
      <div class="flex-1 bg-slate-800 rounded-xl p-5 border-l-4 border-cyan-400">
        <div class="flex items-center gap-2 mb-2">
          <span class="material-symbols-outlined text-cyan-400">apartment</span>
          <span class="text-sm text-cyan-400 font-bold">Q1: 3-4月</span>
        </div>
        <p class="text-lg font-bold mb-1">法人設立</p>
        <p class="text-sm text-slate-300">IP移転・資本金設定・登記完了</p>
      </div>
      <div class="flex-1 bg-slate-800 rounded-xl p-5 border-l-4 border-blue-400">
        <div class="flex items-center gap-2 mb-2">
          <span class="material-symbols-outlined text-blue-400">handshake</span>
          <span class="text-sm text-blue-400 font-bold">Q2: 5-6月</span>
        </div>
        <p class="text-lg font-bold mb-1">JTB契約</p>
        <p class="text-sm text-slate-300">LOI/MOU締結・AI機能実装完了</p>
      </div>
      <div class="flex-1 bg-slate-800 rounded-xl p-5 border-l-4 border-violet-400">
        <div class="flex items-center gap-2 mb-2">
          <span class="material-symbols-outlined text-violet-400">rocket_launch</span>
          <span class="text-sm text-violet-400 font-bold">Q3: 7-8月</span>
        </div>
        <p class="text-lg font-bold mb-1">本番準備</p>
        <p class="text-sm text-slate-300">負荷テスト・運用マニュアル整備</p>
      </div>
      <div class="flex-1 bg-slate-800 rounded-xl p-5 border-l-4 border-amber-400">
        <div class="flex items-center gap-2 mb-2">
          <span class="material-symbols-outlined text-amber-400">emoji_events</span>
          <span class="text-sm text-amber-400 font-bold">Q4: 9-10月</span>
        </div>
        <p class="text-lg font-bold mb-1">アジア競技大会</p>
        <p class="text-sm text-slate-300">9/19〜10/4 本番運用・実績作り</p>
      </div>
    </div>
  </div>
</body>
</html>
```

ポイント:
- ダークテーマ（slate-900）で高級感
- 各フェーズに色分けされたボーダーとアイコン
- 情報量は4ブロックに制限、1ブロック3行以内
- 時系列が左→右で直感的

## Example 2: 比較図（Before/After）

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:wght@400" />
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;700&display=swap" />
  <script>
    tailwind.config = {
      theme: { extend: { fontFamily: { sans: ['Noto Sans JP', 'sans-serif'] } } }
    }
  </script>
</head>
<body class="w-[1280px] h-[720px] m-0 p-0 overflow-hidden font-sans">
  <div class="w-full h-full bg-gray-50 flex flex-col items-center justify-center p-12">
    <h1 class="text-3xl font-bold text-gray-800 mb-8">大会運営の変革</h1>
    <div class="flex gap-8 w-full max-w-5xl">
      <div class="flex-1 bg-white rounded-2xl p-8 shadow-lg border-2 border-red-200">
        <div class="flex items-center gap-3 mb-6">
          <span class="material-symbols-outlined text-red-500 text-4xl">dangerous</span>
          <span class="text-xl font-bold text-red-600">Before: 従来型</span>
        </div>
        <div class="space-y-4">
          <div class="flex items-center gap-3">
            <span class="material-symbols-outlined text-red-400">close</span>
            <span class="text-gray-700">Excel管理で属人化</span>
          </div>
          <div class="flex items-center gap-3">
            <span class="material-symbols-outlined text-red-400">close</span>
            <span class="text-gray-700">ボランティア配置に3日</span>
          </div>
          <div class="flex items-center gap-3">
            <span class="material-symbols-outlined text-red-400">close</span>
            <span class="text-gray-700">リアルタイム状況把握不可</span>
          </div>
        </div>
      </div>
      <div class="flex items-center">
        <span class="material-symbols-outlined text-5xl text-blue-500">arrow_forward</span>
      </div>
      <div class="flex-1 bg-white rounded-2xl p-8 shadow-lg border-2 border-emerald-200">
        <div class="flex items-center gap-3 mb-6">
          <span class="material-symbols-outlined text-emerald-500 text-4xl">verified</span>
          <span class="text-xl font-bold text-emerald-600">After: AXIS導入</span>
        </div>
        <div class="space-y-4">
          <div class="flex items-center gap-3">
            <span class="material-symbols-outlined text-emerald-400">check_circle</span>
            <span class="text-gray-700">クラウドで一元管理</span>
          </div>
          <div class="flex items-center gap-3">
            <span class="material-symbols-outlined text-emerald-400">check_circle</span>
            <span class="text-gray-700">AIが最適配置を30分で提案</span>
          </div>
          <div class="flex items-center gap-3">
            <span class="material-symbols-outlined text-emerald-400">check_circle</span>
            <span class="text-gray-700">ダッシュボードでリアルタイム監視</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</body>
</html>
```

ポイント:
- 赤 vs 緑で直感的なBefore/After対比
- アイコン（close/check_circle）で状態を即座に伝達
- 中央に矢印で変化の方向を示す
- 白背景+シャドウでカード感
