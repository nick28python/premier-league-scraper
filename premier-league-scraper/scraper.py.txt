import requests
from bs4 import BeautifulSoup
import csv

# Set up headers to mimic a browser
HEADERS = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
}

proxies = {
    "http": "http://40e59b7f5b9f4ca58320__cr.gb:f4ff6813e5734d78@gw.dataimpulse.com:823",
    "https": "http://40e59b7f5b9f4ca58320__cr.gb:f4ff6813e5734d78@gw.dataimpulse.com:823"
}

# Premier League 2023-24 season
URL = "https://www.worldfootball.net/all_matches/eng-premier-league-2023-2024/"

def fetch_html(url):
    """Fetch HTML content using GET request."""
    response = requests.get(url, headers=HEADERS, proxies=proxies)
    if response.status_code == 200:
        return response.text
    else:
        print("Failed to fetch page:", response.status_code)
        return None

def parse_matches(html):
    """Extract match data from HTML."""
    soup = BeautifulSoup(html, 'html.parser')
    table = soup.find('table', class_='standard_tabelle')

    data = []

    if table:
        rows = table.find_all('tr')
        for row in rows:
            cols = row.find_all('td')
            if len(cols) >= 6:
                match_date = cols[0].text.strip()
                time = cols[1].text.strip()
                home_team = cols[2].text.strip()
                away_team = cols[4].text.strip()
                score = cols[5].text.strip()

                data.append({
                    "date": match_date,
                    "time": time,
                    "home_team": home_team,
                    "away_team": away_team,
                    "score": score
                })

    return data

def save_to_csv(matches, filename="premier_league_matches.csv"):
    """Save match data to CSV file."""
    with open(filename, mode="w", newline="", encoding="utf-8") as file:
        writer = csv.DictWriter(file, fieldnames=["date", "time", "home_team", "away_team", "score"])
        writer.writeheader()
        writer.writerows(matches)
    print(f"Data saved to {filename} ✅")

def main():
    html = fetch_html(URL)
    if html:
        matches = parse_matches(html)
        for match in matches[:5]:  # Display only first 5 for preview
            print(match)
        save_to_csv(matches)

if __name__ == "__main__":
    main()
