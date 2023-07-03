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

- เรียกใช้งาน library ที่ได้ติดตั้งมา :
```
from bs4 import BeautifulSoup
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.keys import Keys
import pandas as pd
```
```webdriver``` คือคำสั่งสำหรับการควบคุมหน้าเว็บไซต์ ทำงานร่วมกับ chromedriver ภายในเครื่อง
- ระบุตำแหน่งและเรียกใช้งาน chromedriver ภายในเครื่องโดยคำสั่ง ```Service``` :
```
service = Service(executable_path=r'C:\Users\setthawat\.cache\selenium\chromedriver\win32\114.0.5735.90\chromedriver')
```
```executable_path ``` เป็นคำสั่งสำหรับระบุตำแหน่ง เพื่อเรียกใช้งานไดรฟเวอร์ และ ```r``` คือการระบุ raw string
- เรียกใช้งาน webdriver (chromedriver) มาเก็บไว้ในตัวแปร driver :
```
driver = webdriver.Chrome(service=service)
```
