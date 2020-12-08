from html.parser import HTMLParser
from urllib.request import urlopen
from urllib import parse
import re

class LinkParser(HTMLParser):

    def handle_starttag(self, tag, attrs):
        if tag == 'a':
            for (key, value) in attrs:
                if key == 'href':
                    newUrl = parse.urljoin(self.baseUrl, value)
                    self.links = self.links + [newUrl]

    def getLinks(self, url):
        self.links = []
        self.baseUrl = url
        response = urlopen(url)
        if response.getheader('Content-Type')=='text/html':
            htmlBytes = response.read()
            htmlString = htmlBytes.decode('utf-8')
            self.feed(htmlString)
            return htmlString, self.links
        else:
            return "",[]

def spider(url, word, maxPages):
    pagesToVisit = [url]
    numberVisited = 0
    count = 0
    while numberVisited < maxPages and pagesToVisit != []:
        numberVisited = numberVisited + 1
        url = pagesToVisit[0]
        pagesToVisit = pagesToVisit[1:]
        try:
            print(numberVisited, "Visiting", url)
            parser = LinkParser()
            data, links = parser.getLinks(url)
            pagesToVisit = pagesToVisit + links
            regex = r"(Global Confirmed)"
            match = re.search(regex, data)
            # print(match.group())
            match.group()
            count += 1
            print("The word", word, "was found at", url)
        except:
            print("The word wasn't found")
    else:
        print("Word never found")
    print("The word", word, "was found at", count, "different links")

spider("https://coronavirus.jhu.edu/", "Global Confirmed", 200)
