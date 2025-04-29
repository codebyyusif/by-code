import requests
from bs4 import BeautifulSoup
from openpyxl import Workbook

url = "https://www.w-t.az/axtaris?s=Iphone&ms"


headers = {"User-Agent": "Mozilla/5.0"}
response = requests.get(url, headers=headers)
soup = BeautifulSoup(response.text, "html.parser")

wb = Workbook()
ws = wb.active
ws.title = "Iphone Listings"
ws.append(["Title", "Price", "Datetime", "Attributes"])


phones = soup.find_all("div", class_="products-i")

for phone in phones:
    title_tag = phone.find("div", class_="products-i__name")
    price_tag = phone.find("div", class_="product-price")
    datetime_tag = phone.find("div", class_="products-i__datetime")
    attribute_tag = phone.find("div", class_="products-i__attributes")

    title = title_tag.get_text(strip=True) if title_tag else ""
    price = price_tag.get_text(strip=True) if price_tag else ""
    datetime = datetime_tag.get_text(strip=True) if datetime_tag else ""
    attribute = attribute_tag.get_text(strip=True) if attribute_tag else ""

    if title and price and datetime and attribute:
        ws.append([title, price, datetime, attribute])

wb.save("Iphone_listings.xlsx")
print("Done: Iphone_listings.xlsx")

