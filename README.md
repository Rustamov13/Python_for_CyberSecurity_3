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

Python_for_CyberSecurity_3/
│
├── amazon_price_tracker.py
├── amazon_price_history.json
└── README.md


---

## How the Script Works

1. The script sends an HTTP request to the given Amazon product URL  
2. The HTML content of the page is downloaded  
3. The product price is extracted from the page  
4. The price is printed in the terminal  
5. The price, URL, and timestamp are saved in a JSON file  

Each time the script is executed, a new entry is added to the price history file.

---



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

1. The product price will be displayed in the console
2. The price history will be saved in amazon_price_history.json

### Sample Output

```bash
URL: https://www.amazon.com/dp/B0C3PQHGR7
Price text: $199.99
Price value: 199.99
Saved to: amazon_price_history.json
```

## Important Notes

1. Amazon may block automated requests using CAPTCHA
2. If this happens, the script may not retrieve the price
3. For long-term or production use, official APIs such as the Amazon Product Advertising API are recommended

