# python_spyder
## 環境準備 
### 1.1 安裝 Python
- 安裝最新版本的 Python。
- 勾選 "Add Python to PATH"。
### 1.2 安裝相關庫
- 使用 pip 安裝爬蟲相關的庫：

```
pip install requests beautifulsoup4 lxml
```
- 如需動態爬取，額外安裝：

```
pip install selenium
```
- 需要高效動態網頁爬取：

```
pip install playwright
playwright install
```

## 爬蟲需求分析
- 確認數據來源：確定需要爬取的網站和數據（如新聞標題、商品價格等）。
- 檢查網站結構：
- 使用瀏覽器開發者工具（按 F12）。
- 檢查 HTML 標籤及層次結構。
- 如果數據來自 API，找到其請求的接口和數據格式。

## 實現靜態網頁爬蟲
- 步驟
### 3.1 發送 HTTP 請求
- 使用 requests 發送請求：

```
import requests

url = "https://example.com"
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36"
}
response = requests.get(url, headers=headers)

if response.status_code == 200:
    print("請求成功")
else:
    print(f"請求失敗，狀態碼: {response.status_code}")
```
### 3.2 解析 HTML 數據
- 使用 BeautifulSoup 解析 HTML：

```
from bs4 import BeautifulSoup

soup = BeautifulSoup(response.text, "lxml")
```
# 提取特定數據
titles = soup.find_all("h1")  # 找到所有 h1 標籤
for idx, title in enumerate(titles, 1):
    print(f"標題 {idx}: {title.text.strip()}")

## 動態網頁爬取
如果數據是通過 JavaScript 動態加載，需要使用 Selenium 或 Playwright。

### 4.1 使用 Selenium
#### Selenium的瀏覽器驅動器安裝 | [教學影片](https://youtu.be/mRkL0zl1OTQ) | [ChromeDriver下載網址](https://googlechromelabs.github.io/chrome-for-testing/)
- 新增到 Path 系統變數
- 返回到變數列表

- 在當前窗口，點擊 取消 回到系統變數列表。
- 找到 Path 變數（在系統變數中），選中它後，點擊 編輯。
- 編輯 Path

- 在彈出的窗口中點擊 新建。
- 添加以下路徑（假設 ChromeDriver 安裝在 C:\Program Files\ChromeDriver\）：
```
C:\Program Files\ChromeDriver\
```
- 保存設置

- 點擊 確定 保存變更。

- 按下`WIN`+`R`,輸入cmd.exe進入終端
- 輸入
```
chromedriver --version
```
- 應顯示
```
ChromeDriver 131.0.xxxx.xx
```
- 進入anaconda,開啟新專案
- ![image](https://github.com/user-attachments/assets/2929fa17-0d9e-4273-8ddb-9db3aeafd373)
- 開啟新專案後,先按`ctrl`+`s`給專案命名並以`.py`的形式儲存
- ![image](https://github.com/user-attachments/assets/99d46e66-e7d1-4009-80da-e84a1c816806)

- 到右邊的黑色終端區輸入

```
pip install selenium
```
- ![image](https://github.com/user-attachments/assets/e7f0c336-8d0d-4464-809f-8c553c0948b7)

- 等待安裝完畢
- 安裝完之後點擊以下重啟`kernel`
- ![image](https://github.com/user-attachments/assets/971e5d05-a4a9-4ec1-99f4-65560e0ead90)
- 寫入程式測試已安裝成功
```
import selenium
print("Selenium 已成功安裝！")
```
- 有安裝成功顯示
```
Selenium 已成功安裝！
```
- **動態爬蟲代碼**
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options

# 配置 Selenium 瀏覽器

options = Options()
options.add_argument("--headless")  # 無頭模式
service = Service("path/to/chromedriver")  # 替換為 ChromeDriver 路徑

driver = webdriver.Chrome(service=service, options=options)

# 打開網頁

url = "https://example.com"
driver.get(url)

# 提取動態數據

elements = driver.find_elements(By.TAG_NAME, "h1")
for idx, element in enumerate(elements, 1):
    print(f"標題 {idx}: {element.text}")

driver.quit()
```
## 數據存儲
## 爬蟲優化
## 部署與自動化
