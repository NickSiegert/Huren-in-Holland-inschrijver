import time
from tkinter import messagebox, Tk
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from bs4 import BeautifulSoup


driver = webdriver.Firefox()

global email
global password
email= "XxxxX"      #email acount of your: "huren in holland account"
password= "XxxxX"   #password of your: "huren in holland account"


def selenium_sleep_CSS(x): #Sleep until certain CCS element is found on the webpage
    WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.CSS_SELECTOR, x)))
    time.sleep(0.5)

def info_box(x,y):
    top = Tk()
    top.geometry("100x100")
    messagebox.showinfo(x, y)

def inschrijven():
    driver.get("https://www.hureninhollandrijnland.nl/aanbod/te-huur#?gesorteerd-op=reactiedatum-")
    selenium_sleep_CSS(".dropdown-selector")

    html = driver.page_source
    soup = BeautifulSoup(html, 'html.parser')
    x = soup.find("div", class_="object-list-items-container")

    for i, child in zip(range(reacties), x.findChildren("section", recursive=False)): #the zip makes the loop, loop for: "reacties" amount of times
        if "U heeft gereageerd" in child.text:
            continue
        else:
            house_id = child.find("div", {'class': 'ng-scope'}).get("id")
            driver.find_element_by_id(house_id).click()

            selenium_sleep_CSS("h1.ng-binding > span:nth-child(1)") #wait till browser is loaded
            driver.find_element_by_xpath("/html/body/div[1]/main/div[1]/div/div/div/div/div[3]/div/section[9]/div/div[1]/div/form/input").click()
            driver.get("https://www.hureninhollandrijnland.nl/aanbod/te-huur#?gesorteerd-op=reactiedatum-")
            selenium_sleep_CSS("label.ng-scope") #wait till browser is loaded

def login():

    driver.get("https://www.hureninhollandrijnland.nl/mijn-omgeving/inloggen")
    selenium_sleep_CSS(".default-form > input:nth-child(2)") #wait till browser is loaded


    driver.find_element_by_id("username").send_keys(email)
    driver.find_element_by_id("password").send_keys(password)
    driver.find_element_by_css_selector(".default-form > input:nth-child(2)").send_keys(Keys.ENTER)

    selenium_sleep_CSS(".icon-br_mijn_reacties") #wait till browser is loaded
    driver.find_element_by_xpath("/html/body/div[1]/main/div[2]/div[1]/div/dashboard/div/div[2]/div[1]/ul[1]/li[2]/a/span[2]").click() #click op "Mijn reacties"

    time.sleep(3) #wait till browser is loaded
    html = driver.page_source
    soup = BeautifulSoup(html, 'html.parser')
    x = soup.find('span', {'class':'titel ng-scope'}).text
    print(x)

    if x in ("0 van de maximaal 3 actieve reacties", "1 van de maximaal 3 actieve reacties", "2 van de maximaal 3 actieve reacties"):
        global reacties
        reacties = 3-int(x[0]) #grabs the first number of the string that is produced by "x"
        inschrijven()
    else:
        print("Maximaal aantal reacties bereikt, exit programma")
        info_box("Woningnet inchrijver", "Het maximaal aantal reacties is bereikt, exit programma")

login()



