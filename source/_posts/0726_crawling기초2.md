---
title: "0726_crawling기초2"
date: '2022-07-26'
---

# import requests
from bs4 import BeautifulSoup

# url = "http://www.naver.com"

# req = requests.get(url)
# print(req.status_code)
# 200 이 뜨면 웹페이지가 살아 있다는 것 404가 뜰 경우 잘 못들어 간 것 이거나 사이트가 사라진 것
# print(req.text)


# 네이버에서 기사 가져오기
import requests
from bs4 import BeautifulSoup


def crawling(soup):
    div = soup.find("div", class_="list_issue") 
# 사이트에서 크롤링할 것에 대해 검사 창을 열어 요소를 확인하고 대입해야 함
    # print(div)
    result = []

    for a in div.find_all("a"):
        # print(a.get_text())
        result.append(a.get_text())

    links = []
    for a in div.find_all("a"):
        # print(a['href'])
        # result.append(a.get_text())
        links.append(a['href'])

    print(result)
    print(links)

    return None


def main():
    url = "https://www.naver.com/"
    req = requests.get(url)
    soup = BeautifulSoup(req.text, "html.parser")

    print(crawling(soup))


if __name__ == "__main__":
    main()


# 벅스 차트 가져오기
import requests
from bs4 import BeautifulSoup


def crawling(soup): 
    tbody = soup.find("tbody")
    # print(tbody)

    result = []

    for p in tbody.find_all("p", class_="title"):
        # print(p.find("a").get_text())
        result.append(p.find("a").get_text())

    return result


def main():
    url = "https://music.bugs.co.kr/chart"
    req = requests.get(url)
    soup = BeautifulSoup(req.text, "html.parser")

    print(crawling(soup))


if __name__ == "__main__":
    main()