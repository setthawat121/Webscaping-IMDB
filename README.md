# Webscraping-IMDB
## Webscraping with python
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
# สร้าง for loop พร้อมกับระบุ range เป็น 4 หมายถึงการ loop 4 ครั้ง เพื่อวนเก็บข้อมูลทั้งหน้าให้ครบ 4 หน้า
for i in range(4) :
    data = driver.page_source
    soup = bs4.BeautifulSoup(data) 
    Name_source = soup.find_all('div',{'class':'lister-item-content'}) #ระบุแท็กหัวเรื่องพร้อมกับชื่อคลาสของตำแหน่งข้อมูลที่ต้องการเก็บ
    for source in Name_source : # for loop สำหรับวนเก็บข้อมูลทุกหัวข้อที่ต้องการ
        All_Name.append(source.find('h3',{'class':'lister-item-header'}).find('a').text)
        All_Years.append(int(source.find('span',{'class':'lister-item-year text-muted unbold'}).text
                             .replace('(','').replace(')','').replace('I',''))) # replace คือ การแทนที่ข้อมูลที่ไม่ต้องการด้วยค่าว่าง
        All_Type.append(source.find('span',{'class':'genre'}).text.replace('            ',''))
        All_Rate.append(float(source.find('div',{'class':'inline-block ratings-imdb-rating'})
                              .find('strong').text))    
        All_Gross.append(source.find('p',{'class':'sort-num_votes-visible'}).text.split('\nVotes:')[1][18:-1]) 
        # split คือการแบ่งข้อมูลโดยจะเริ่มจากข้อความ '\nVotes:' ไปทางด้านขวาก็คือ [1] นับถัดไป 18 ตัวอักษร และ ลบข้อความด้านหลัง 1 ตัวอักษร
        # โดย split นี้จะใช้เป็นการระบุตำแหน่งข้อมูลที่ต้องการ
    if  len(All_Name) == 50 : # เป็นเงื่อนไขสำหรับการขึ้นหน้าใหม่ เพื่อเก็บข้อมูลในหน้าถัดไป
        next_page = driver.find_element('xpath','//*[@id="main"]/div/div[4]/a')
        next_page.click()
    else :
        next_page = driver.find_element('xpath','//*[@id="main"]/div/div[4]/a[2]')
        next_page.click()
```
การทำ webscraping จำเป็นต้องศึกษาและทำความเข้าใจหน้าเว็บไซต์ และปรับเปลี่ยนโค้ตให้เข้ากับหน้าเว็บที่ต้องการจะเก็บ
<br />
