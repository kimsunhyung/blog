---
title: "0726_crawling기초"
date: '2022-07-26'
---

from bs4 import BeautifulSoup

# index.html 파일을 불러옴
soup = BeautifulSoup(open("index.html", encoding="utf-8"), "html.parser")

# print(soup)
soup = (soup.find('div',class_ = "computer"))
# print(soup)

soup = soup.find('p')
# print(soup)

final_text =soup.get_text()
print(final_text)