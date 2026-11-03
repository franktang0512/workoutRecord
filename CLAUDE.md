# 健康紀錄專案說明（給 Claude Code）

這個資料夾是 Frank 的每日運動與飲食紀錄。他會用聊天的方式告訴你吃了什麼、做了什麼運動，由你負責更新 `data/` 裡的 CSV。`index.html` 是讀取這些 CSV 的儀表板。

## 檔案
- `data/daily.csv`：每天一列。`date,weight_kg,cardio_min,strength,water_ml,supplements,note`
  - `strength` 只能是：空白、`自主`、`教練課`
  - `water_ml`：當天喝水總量（ml），同一天多次要相加
  - `supplements`：當天有吃的保健品，填 `supplements.csv` 的 `short` 名稱，用「、」分隔，例如 `肌酸、維他命`
  - 同一天已有資料時，更新那一列，不要新增重複的日期；有氧分鐘同一天多次要相加
- `data/supplements.csv`：要追蹤的保健品清單。`short,name,dose,note`。網頁會依這份清單顯示打勾項目；新增保健品就加一列。保健品熱量可忽略，不記到 meals。
- `data/meals.csv`：一樣食物一列。`date,meal,food,qty,unit,kcal,protein_g,category`
  - `meal`：早餐／午餐／晚餐／點心／宵夜
  - `category`：`正常` 或 `亂吃`（零食、甜點、含糖飲料、宵夜炸物通常算亂吃；不確定就問）
  - `kcal`、`protein_g` 存的是「這次吃的份量」算好的數值
- `data/foods.csv`：食物庫，每種食物的標示登記一次。`food,base_qty,unit,kcal,protein_g,note,source`
  - 包裝怎麼標就怎麼存（每 100 g、每個、每份都可以）
  - `source`：`包裝標示`、`官方資料`、`網路整理數據` 或 `估計`
- `data/tanita.csv`：體組成測量紀錄
- `data/targets.csv`：目標值

## 記錄流程
1. 日期沒說就用今天（台灣時間）。
2. 先查 `foods.csv` 有沒有這個食物；有就照基準量換算。
3. 沒有的話：使用者給了包裝標示就照標示；外食連鎖店先查官方營養資訊；查不到才估計，並在 `source` 標 `估計`。新食物要加進 `foods.csv`。
4. 份量或品項不明確時（例如「大杯咖啡」是美式還是拿鐵），先問清楚，或明確說出你的假設。
5. 寫入後，回報：這次新增的品項與熱量、今天累計熱量與蛋白質、離目標還差多少。
6. CSV 欄位裡有逗號時要用雙引號包起來；保持 UTF-8 編碼。

## 回覆風格
- 繁體中文，簡潔。
- 數值都是估算，不要假裝精確；估計的品項要說是估計。
- 不要建議極端節食或補償性運動；偶爾亂吃就正常記錄，下一餐回到正常即可。

## Git
- 這個資料夾是 GitHub repo，透過 GitHub Pages 發布 `index.html`。
- 使用者說「推上去」「同步」時，執行：`git add data && git commit -m "紀錄 YYYY-MM-DD" && git push`。沒說就不要自動 push。
