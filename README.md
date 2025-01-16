# python_spyder
## 環境準備階段
### 1.1 安裝 Python
- 安裝最新版本的 Python。
- 勾選 "Add Python to PATH"。
### 1.2 安裝相關庫
- 使用 pip 安裝爬蟲相關的庫：
- 下載附帶的`requiredments.txt`
```
pip install -r requiredments.txt
```
- 確認是否都安裝正確:
- 下載附件`spyder_package_check.py`並在專案環境內執行`spyder_package_check.py`.
## 分析需求
- 要蒐集的數據會是美食評價網頁,例如說個人部落格或是自己做的部落格網頁,經過分析過後基本上都屬於靜態網頁,所以BeautifulSoup的Python爬蟲就足以提供需求
- 內文有分成兩類:一種是推薦文,推薦文的格式都會比較好,內文都包含'電話','地址','時間',其他的像是'店名','評價內容'的特徵定義比較有難度
- 第二類就是心得文:完全沒有提到以上的關鍵字,這樣的話數據清洗光是定義特徵就會恨麻煩


## 實現靜態網頁爬取階段
- 使用BeautifulSoup的python spyder
- 可輸入目標網址及自訂義儲存檔案的名稱
```
import requests
from bs4 import BeautifulSoup
import json

def connect(url):
    try:
        response = requests.get(url)  # 修正為 requests.get(url)

        if response.status_code == 200:
            print("Web connection established successfully!")
            scrape_and_save(url, response)  # 呼叫 scrape_and_save 函數並傳遞 response
        else:
            print(f"Connection failed! Status code: {response.status_code}")
    except Exception as e:
        print(f"Unknown error: {e}")

def scrape_and_save(url, response):
    soup = BeautifulSoup(response.text, 'html.parser')
    
    paragraphs = []  # 用於儲存段落

    # 提取所有段落
    for p in soup.find_all('p'):
        text = p.get_text(strip=True)
        paragraphs.append(text)

    print("\nParagraph content:")

    # 顯示段落
    for i, paragraph in enumerate(paragraphs, start=1):
        print(f"Paragraph {i}: {paragraph}")

    # 儲存到檔案
    save(paragraphs)

def save(paragraphs):
    file_name = input("\nInsert file name to store (Press enter for default): ").strip()
    if not file_name:
        file_name = "default_scraped_data"  # 提供預設檔名
    file_name = file_name + ".json"

    data_to_save = {"paragraphs": paragraphs}
    # encoding="utf-8"可直接儲存中文字,不會轉換成其他編碼
    with open(file_name, "w", encoding="utf-8") as file:
        json.dump(data_to_save, file, ensure_ascii=False, indent=4)

    print(f"\nData has been stored in '{file_name}'\n")

def main():
    while True:
        url = input("Please insert a URL (Insert 'exit' to exit): ").strip()
        if url.lower() == 'exit':
            print("Exiting... Bye!")
            break

        if not url.startswith("http://") and not url.startswith("https://"):
            print("Invalid URL! Must begin with 'http://' or 'https://'.")
            continue

        # 呼叫 connect 函數來處理連接與抓取
        connect(url)

# Ensure the block can run independently if other files import this one
if __name__ == "__main__":
    main()

```
## 設計數據清洗階段
- 通過讀取JSON檔案來處理裡面的文本數據
- 再將處理過後的文檔儲存,可自訂義儲存檔案的名稱
```
import json
import pandas as pd
file_name = input("open file:")
# 讀取 JSON 檔案
with open(file_name, "r", encoding="utf-8") as file:
    data = json.load(file)

# 提取資料
restaurants = data.get("paragraphs", [])

# 初始化容器
cleaned_restaurants = []
current_restaurant = {}

# 定義關鍵字標誌
keywords = ["地址", "時間", "電話"]
review_keywords = ["麵","菜","湯","好吃", "香濃", "推薦", "美味", "新鮮", "實在", "划算", "便宜"]  # 評價關鍵字
# 遍歷段落提取資訊
for paragraph in restaurants:
    paragraph = paragraph.strip()  # 去除前後空白
    if not paragraph:  # 跳過空行
        continue
    if "地址：" in paragraph or "地址:" in paragraph:
        current_restaurant["address"] = paragraph.split("：")[-1].strip()
    elif "時間：" in paragraph or "時間:" in paragraph:
        current_restaurant["hours"] = paragraph.split("：")[-1].strip()
    elif "電話：" in paragraph or "電話:" in paragraph:
        current_restaurant["phone"] = paragraph.split("：")[-1].strip()
    elif any(k in paragraph for k in keywords):  # 遇到關鍵欄位時忽略無效數據
        continue
    else:
        if "address" in current_restaurant:  # 如果已有地址，視為新餐廳開始
            cleaned_restaurants.append(current_restaurant)
            current_restaurant = {}
        current_restaurant["name"] = paragraph  # 當前段落視為餐廳名稱

# 添加最後一間餐廳（如未結束的）
#if current_restaurant:
#    cleaned_restaurants.append(current_restaurant)

# 將清理後的數據轉換為 DataFrame
df = pd.DataFrame(cleaned_restaurants)

# 補全缺失值
df.fillna("未知", inplace=True)
save_name = input("save file name:")
# 儲存清洗後的結果
df.to_csv(save_name, index=False, encoding="utf-8")

print(f"清洗完成，已儲存為 {save_name} ")

```
## 打包階段
## 部屬階段
