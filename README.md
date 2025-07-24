import requests
from bs4 import BeautifulSoup
import csv

URL = "https://books.toscrape.com/"
response = requests.get(URL)

if response.status_code == 200:
    soup = BeautifulSoup(response.text, "html.parser")
    books = soup.find_all("article", class_="product_pod")

    data = []
    for book in books:
        title = book.h3.a["title"]
        price = book.find("p", class_="price_color").text
        data.append([title, price])

    # CSV schreiben
    with open("books.csv", "w", newline="", encoding="utf-8") as f:
        writer = csv.writer(f)
        writer.writerow(["Titel", "Preis"])
        writer.writerows(data)

    print("CSV erfolgreich erstellt!")
else:
    print("Fehler beim Abrufen der Seite:", response.status_code)
