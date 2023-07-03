# Webscaping-IMDB
## Webscaping with python
- ติดตั้ง library selenium and BeautifulSoup :
```
!pip install selenium
!pip install bs4
```
```selenium``` คือ library สำหรับการควบคุมหน้าเว็บไซต์ Chrom
<br />```bs4``` คือ library สำหรับการดึงข้อมูลจากหน้า HTML
<br />
<br />

- เรียกใช้งาน library ที่ได้ติดตั้งมา :
```
from bs4 import BeautifulSoup
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.keys import Keys
import pandas as pd
```
```webdriver``` คือคำสั่งสำหรับการควบคุมหน้าเว็บไซต์ ทำงานร่วมกับ chromedriver ภายในเครื่อง
<br />
<br />

- ระบุตำแหน่งและเรียกใช้งาน chromedriver ภายในเครื่องโดยคำสั่ง ```Service``` :
```
service = Service(executable_path=r'C:\Users\setthawat\.cache\selenium\chromedriver\win32\114.0.5735.90\chromedriver')
```
```executable_path ``` เป็นคำสั่งสำหรับระบุตำแหน่ง เพื่อเรียกใช้งานไดรฟเวอร์ และ ```r``` คือการระบุ raw string
<br />
<br />

- เรียกใช้งาน webdriver (chromedriver) มาเก็บไว้ในตัวแปร driver :
```
driver = webdriver.Chrome(service=service)
```
<br />

- ระบุตำแหน่ง url เพื่อเข้าถึงเว็บไซต์ :
```
driver.get('https://www.imdb.com/search/title/?genres=animation&sort=user_rating,desc&title_type=feature&num_votes=25000,&pf_rd_m=A2FGELUUNOQJNL&pf_rd_p=94365f40-17a1-4450-9ea8-01159990ef7f&pf_rd_r=FNEZZAA0SHJRKNZW97RQ&pf_rd_s=right-6&pf_rd_t=15506&pf_rd_i=top&ref_=chttp_gnr_3')
```
<br />

- สร้างตัวแปรเพื่อเก็บ object โดยใช้คำสั่ง ```BeautifulSoup``` :
```
soup = bs4.BeautifulSoup(data) 
```
<br />

- สร้าง loop เพื่อดึงข้อมูลที่ต้องการมาเก็บไว้ใน list :
```
# สร้าง list ตามจำนวนข้อมูลที่ต้องการจัดเก็บ
All_Name = []
All_Years = []
All_Type = []
All_Rate = []
All_Gross = []
for i in range(4) :
    data = driver.page_source
    soup = bs4.BeautifulSoup(data) 
    Name_source = soup.find_all('div',{'class':'lister-item-content'})
    for source in Name_source :
        All_Name.append(source.find('h3',{'class':'lister-item-header'}).find('a').text)
        All_Years.append(int(source.find('span',{'class':'lister-item-year text-muted unbold'}).text
                             .replace('(','').replace(')','').replace('I','')))
        All_Type.append(source.find('span',{'class':'genre'}).text.replace('            ',''))
        All_Rate.append(float(source.find('div',{'class':'inline-block ratings-imdb-rating'})
                              .find('strong').text))    
        All_Gross.append(source.find('p',{'class':'sort-num_votes-visible'}).text.split('\nVotes:')[1][18:-1])
    if  len(All_Name) == 50 :
        next_page = driver.find_element('xpath','//*[@id="main"]/div/div[4]/a')
        next_page.click()
    else :
        next_page = driver.find_element('xpath','//*[@id="main"]/div/div[4]/a[2]')
        next_page.click()
```
<br />
