# premier-league-scraper
 # ⚽ Premier League Match Scraper - Beginner Friendly Guide

---

## 👋 Introduction

This project is for **complete beginners** who want to learn how to extract football match data from websites using Python.

Even if you've **never written code before**, this step-by-step guide will help you **understand each line of code** and how it works.

---

## 📌 What This Script Does

It connects to the website [worldfootball.net](https://www.worldfootball.net) and scrapes Premier League match data for the 2023–2024 season, including:
- Match date
- Time
- Home team
- Away team
- Final score

The script displays the first 5 matches in the terminal (you can show all if you want).

---

## 🛠️ Step-by-Step Code Explanation

### 🔹 1. Importing Libraries

```python
import requests
from bs4 import BeautifulSoup
import csv

🔹 2. Set Up Headers

HEADERS = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
}

:- This makes your script behave like a normal browser (like Chrome).

:- Some websites block bots; this helps bypass that block.

🔹 3. Setup Proxy (Optional - Advanced)

proxies = {
    "http": "http://your_proxy_here",
    "https": "http://your_proxy_here"
}

:- Proxies are like middlemen servers — they help hide your identity.

-⚠️ If you don’t have a proxy, you can remove these lines from the code.

🔹 4. Target Website URL

URL = "https://www.worldfootball.net/all_matches/eng-premier-league-2023-2024/"

:- This is the page from which we want to scrape the match data.

🔹 5. Function to Fetch HTML

def fetch_html(url):

:- Starts a function that takes a URL and returns HTML code of that page.

    response = requests.get(url, headers=HEADERS, proxies=proxies)

:- This line makes a request to the website.

:- headers make the request look like it's coming from a browser.

:- proxies are optional (only if needed),it's better to use proxy

    if response.status_code == 200:
        return response.text

:- If website successfully opens (status 200), return the HTML text.

    else:
        print("Failed to fetch page:", response.status_code)
        return None

:- If website fails to load, print an error message.

🔹 6. Function to Parse Match Data

def parse_matches(html):
    soup = BeautifulSoup(html, 'html.parser')

:- Converts raw HTML into a searchable object using BeautifulSoup.

    table = soup.find('table', class_='standard_tabelle')

:- Finds the table that contains all the match data (using its class name).

    data = []

:- An empty list where we’ll store all the scraped matches.

    if table:
        rows = table.find_all('tr')

:- Gets all the rows (<tr>) from the match table.

        for row in rows:
            cols = row.find_all('td')

:- For each row, extract all columns (<td>).

            if len(cols) >= 5:

:- Only process rows that have at least 5 columns (to skip empty rows or headers).

                match_date = cols[0].text.strip()
                time = cols[1].text.strip()
                home_team = cols[2].text.strip()
                away_team = cols[4].text.strip()
                score = cols[5].text.strip()

:- Extract individual data from each column (cleaned using .strip() to remove spaces).

                data.append({
                    "date": match_date,
                    "time": time,
                    "home_team": home_team,
                    "away_team": away_team,
                    "score": score
                })

:- Add the data for this match as a dictionary inside our list.

    return data

:- Return the final list of all matches.

🔹 7. Main Function to Run Script

def main():
    html = fetch_html(URL)
    if html:
        matches = parse_matches(html)
        for match in matches[:5]:
            print(match)

:- Call the fetch + parse functions.

:- Display only the first 5 matches on screen for preview.

🔹 8. Run the Code

if __name__ == "__main__":
    main()

:- This line tells Python to start from main() when you run the file.

📂 Sample Output

{
  "date": "19.08.2023",
  "time": "15:00",
  "home_team": "Arsenal",
  "away_team": "Liverpool",
  "score": "3:1"
}

💡 Beginner Notes

   :- HTML is like a web page's structure.

   :- Tags like <tr>, <td> are used to store table data.

   :- Python can extract these using tools like BeautifulSoup.


❓ FAQs
❓ I don’t know Python. Can I still learn?

Yes! This script is written in basic Python, with clear steps and can be learned even by non-coders.
❓ Will I get blocked?

If you scrape too fast or too many pages, yes. Use headers and delays to avoid that, it's better if you use proxy
❓ Can I save data in Excel?

Yes, use the csv module to save scraped data in .csv file (like Excel). Ask me if you need help!
