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
