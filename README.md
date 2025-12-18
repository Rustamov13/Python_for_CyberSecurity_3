# Amazon Price Tracker (Python)

## Project Description

This project is a simple Python application that tracks product prices on the Amazon platform.  
The script retrieves an Amazon product page, extracts the current product price (including discounted prices), prints the price to the console, and stores the price history in a JSON file.

This project is created for educational and laboratory purposes.

---

## Objectives

- Retrieve HTML content from an Amazon product page  
- Extract the current product price  
- Correctly handle discounted and regular prices  
- Print the price as console output  
- Save price data with timestamps for tracking  

---

## Technologies Used

- Python 3  
- Requests  
- BeautifulSoup (bs4)  
- JSON  

---

## Project Structure

- Python_for_CyberSecurity_3/
- │
- ├── amazon_price_tracker.py
- ├── amazon_price_history.json
- └── README.md


---

## How the Script Works

1. The script sends an HTTP request to the given Amazon product URL  
2. The HTML content of the page is downloaded  
3. The product price is extracted from the page  
4. The price is printed in the terminal  
5. The price, URL, and timestamp are saved in a JSON file  

Each time the script is executed, a new entry is added to the price history file.

---

## Python code

```python
import json
import re
from datetime import datetime, timezone
from pathlib import Path

import requests
from bs4 import BeautifulSoup

URL = "https://www.amazon.com/DEWALT-Cordless-Batteries-Included-DCK277D2/dp/B0C3PQHGR7/"
HISTORY_FILE = Path("amazon_price_history.json")

def normalize_price(text):
    if not text:
        return None
    s = text.strip().replace("\u00a0", " ").replace(",", "")
    m = re.search(r"(\d+(?:\.\d+)?)", s)
    if not m:
        return None
    try:
        return float(m.group(1))
    except ValueError:
        return None

def fetch_html(url):
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0 Safari/537.36",
        "Accept-Language": "en-US,en;q=0.9",
        "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
    }
    r = requests.get(url, headers=headers, timeout=25)
    r.raise_for_status()
    return r.text

def extract_price(html):
    soup = BeautifulSoup(html, "html.parser")

    deal_selectors = [
        "#priceblock_dealprice",
        "#priceblock_saleprice",
        "#corePriceDisplay_desktop_feature_div span.a-price span.a-offscreen",
        "#corePriceDisplay_desktop_feature_div span.aok-offscreen",
        "#corePrice_feature_div span.a-price span.a-offscreen",
        "#corePrice_feature_div span.aok-offscreen",
        "[data-a-color='price'] span.a-offscreen",
        "span.a-price[data-a-color='price'] span.a-offscreen",
    ]

    for sel in deal_selectors:
        for el in soup.select(sel):
            if el.find_parent(class_="a-text-price"):
                continue
            t = el.get_text(strip=True)
            p = normalize_price(t)
            if p is not None:
                return p, t

    regular_selectors = [
        "#priceblock_ourprice",
        "span.a-price span.a-offscreen",
        "span.aok-offscreen",
    ]

    for sel in regular_selectors:
        for el in soup.select(sel):
            if el.find_parent(class_="a-text-price"):
                continue
            t = el.get_text(strip=True)
            p = normalize_price(t)
            if p is not None:
                return p, t

    return None, None

def load_history():
    if HISTORY_FILE.exists():
        try:
            return json.loads(HISTORY_FILE.read_text(encoding="utf-8"))
        except Exception:
            return []
    return []

def save_history(history):
    HISTORY_FILE.write_text(json.dumps(history, indent=2, ensure_ascii=False), encoding="utf-8")

def main():
    html = fetch_html(URL)
    low = html.lower()
    if "captcha" in low or "enter the characters you see below" in low:
        print("Amazon blocked the request (CAPTCHA).")
        return

    price_value, price_text = extract_price(html)
    if price_text is None:
        print("Price not found.")
        return

    ts = datetime.now(timezone.utc).strftime("%Y-%m-%dT%H:%M:%SZ")

    history = load_history()
    history.append({"ts": ts, "url": URL, "price_text": price_text, "price_value": price_value})
    save_history(history)

    print("URL:", URL)
    print("Price text:", price_text)
    print("Price value:", price_value)
    print("Saved to:", str(HISTORY_FILE))

if __name__ == "__main__":
    main()

```

---

## How to Run

### Install required libraries

```bash
pip install requests beautifulsoup4

``` 

### Run the script

```bash
python amazon_price_tracker.py
```

## After running the script:

* The product price will be displayed in the console
* The price history will be saved in amazon_price_history.json

### Sample Output

```bash
URL: https://www.amazon.com/dp/B0C3PQHGR7
Price text: $199.99
Price value: 199.99
Saved to: amazon_price_history.json
```

## Important Notes

* Amazon may block automated requests using CAPTCHA
* If this happens, the script may not retrieve the price
* For long-term or production use, official APIs such as the Amazon Product Advertising API are recommended

