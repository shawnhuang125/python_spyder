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

### 4.1 使用 Selenium | [教學影片]() | [ChromeDriver下載網址](https://googlechromelabs.github.io/chrome-for-testing/)
- 安裝 WebDriver
- 下載對應版本的 ChromeDriver。 
- 放在系統 PATH 或指定路徑。    
- 動態爬蟲代碼
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
