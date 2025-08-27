# aws-sop（最小可用範例）

## 快速開始（本機）
```bash
pip install mkdocs mkdocs-material
mkdocs serve
# 瀏覽 http://127.0.0.1:8000
```

## 佈署到 GitHub Pages（自動）
1. 建立 GitHub repo，預設分支 `main`
2. 將本目錄 push 上去
3. 於 GitHub → Settings → Pages → Branch 選擇 `gh-pages`
4. 之後每次 push，GitHub Actions 會自動建置並發佈

> 若不使用 Actions，也可本機執行：`mkdocs gh-deploy`。