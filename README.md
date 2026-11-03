# 健康紀錄

純靜態網頁，資料存在 `data/*.csv`，不需要資料庫或後端。

## 結構
```
index.html        儀表板（讀取 data/ 裡的 CSV）
CLAUDE.md         給 Claude Code 的記錄規則
.nojekyll         讓 GitHub Pages 直接提供檔案
data/
  daily.csv       每日體重、有氧、重訓
  meals.csv       飲食明細（一樣食物一列）
  foods.csv       食物庫（營養標示）
  tanita.csv      體組成測量
  targets.csv     目標值
```

## 發布到 GitHub Pages
1. 把這些檔案放在 repo 根目錄並 push。
2. GitHub repo → Settings → Pages → Source 選 `Deploy from a branch`，Branch 選 `main`、資料夾 `/ (root)`。
3. 幾分鐘後網址會是 `https://<帳號>.github.io/<repo 名稱>/`。

## 每天記錄
在 VS Code 開這個資料夾，用 Claude Code 說「早餐吃了豆漿和蛋白粉」「今天體重 112.8」，它會更新 CSV；說「推上去」就 commit + push，網頁幾分鐘內更新。

## 本機預覽
直接雙擊 index.html 會讀不到 CSV（瀏覽器限制），請執行 `python -m http.server 8000` 再開 `http://localhost:8000`。
