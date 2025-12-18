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

## How to Run

### Install required libraries

```bash
pip install requests beautifulsoup4


### How to Run

Run the script using the following command:

```bash
python amazon_price_tracker.py
