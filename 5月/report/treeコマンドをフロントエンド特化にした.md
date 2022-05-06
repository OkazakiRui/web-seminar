---
marp: true
backgroundColor: #333
color: #f2f2f2
style: |
  * {
    color: #f2f2f2 !important;
    background: #333 !important;
    width: 100%;
  }
  header {
    padding-top: .4rem;
    font-size: .8rem;
  }
paginate: true
---

# tree コマンドをフロントエンド特化にした

## WD3A 岡崎 流依

---

## 目次

1. 作成に至った背景
2. 完成した物
3. 実際のコード
4. 解説

---

# 作成に至った背景

---

<!-- _header: 作成に至った背景 -->

### フロントエンドの環境で tree コマンドを使うと<br/> node_modules の内容が表示されディレクトリ構造が分かりづらい

---

# 完成した物

---

tree コマンドを打っても node_modules が表示されなくなった

```
$ tree
..
├── README.md
├── aspida.config.js
├── makeComponentsFile.sh
├── next-env.d.ts
├── package.json
├── src
│   ├── pages
│   │   ├── _app.tsx
│   │   ├── _document.tsx
│   │   └── index.tsx
│   └── theme
│       └── index.ts
├── tsconfig.eslint.json
├── tsconfig.json
└── yarn.lock

3 directories, 12 files
```

---

# 実際のコード

-I オプションを使用し alias を設定した、<br/>今回は設定していないが階層制限などもできる。

```
alias tree="tree -I node_modules"
```

---

# 解説

---

### tree コマンドは default で入って無いので Homebrew でインストール

```
$ brew install tree
```

### 普段使いは zsh なので、~/.zshrc に alias を設定する

```
alias tree="tree -I node_modules"
```

### 再読み込みする

```
$ source ~/.zshrc
```

---

# 完成〜！！🥳🥳🥳
