# Diction-and-dictionary-scrapping
Diction & dictionary scrapping. A script for scrapping a website and counting of unique words

Installation Use the package manager pip to install foobar.

pip install beautifulsoup4
Usage
from bs4 import BeautifulSoup
import requests

import re
from collections import Counter

#IMPORT CSV LIBRARY
import  csv
#OPEN A NEW CSV FILE. 
file = open('scraped_quotes.csv','w')

#OPEN A NEW TXT FILE. 
file = open('scraped_quotes.txt','w')


#CREATE A VARIABLE FOR WRITING TO THE CSV
writer = csv.writer(file)


#CREATE THE HEADER ROW OF THE CSV
writer.writerow(['Quote','Author'])

#REQUEST WEBPAGE AND STORE IT AS A VARIABLE
page_to_scrape = requests.get("https://quotes.toscrape.com/")

#USE BEAUTIFULSOUP TO PARSE THE HTML AND STORE IT AS A VARIABLE
soup = BeautifulSoup(page_to_scrape.text,'html.parser')

#FIND ALL THE ITEMS IN THE PAGE WITH A CLASS ATTRIBUTE OF 'TEXT'
#AND STORE THE LIST AS A VARIABLE
quotes = soup.findAll('span', attrs={'class':'text'})

#FIND ALL THE ITEMS IN THE PAGE WITH A CLASS ATTRIBUTE OF 'AUTHOR'
#AND STORE THE LIST AS A VARIABLE
authors= soup.findAll('small', attrs={'class':'author'})

#LOOP THROUGH BOTH LISTS USING THE 'ZIP' FUNCTION
#AND PRINT AND FORMAT THE RESULTS
for quote,author in zip (quotes,authors):
    print(quote.text+"-"+author.text)
    writer.writerow([quote.text ,author.text])
    file.write(quote.text+"-"+author.text)

#CLOSE THE CSV and TXT FILE
file.close()

#FUNCTION TO COUNT UNIQUE WORDS
def count_words(path):

#OPEN THE TXT FILE SAVED FROM THE SCAPPED WEBSITE
with  open(path, encoding='unicode_escape') as file:

    #FIND WORDS IN TEXT USING REGULAR EXPRESSION
    all_words = re.findall(r"[0-9a-zA-Z-']+",file.read())

    #CONVERT WORDS FOUND TO UPPERCASE
    all_words = [word.upper() for word in all_words] 

    #DISPLAY NUMBER OF WORDS FOUND
    print('\nTotalwords:',len(all_words))

    word_count = Counter()
    #ITERATE THE WORDS FOUND
    #INCREAMENT WHEN A NEW WORD IS FOUND
    for word in all_words:
        word_count[word]+=1
        
    print('\n Words:')
    for word in word_count.most_common():
        print(word[0], '\t', word[1])


#CALL FUNCTION
count_words('scraped_quotes.txt')

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.
