# Project Title:
Twitter Tweet Scraper for Elon Musk's Tweets

Description:
This Python script utilizes Selenium, a web automation tool, to scrape tweets from Twitter. It is specifically designed to target tweets related to Elon Musk, the CEO of Tesla and SpaceX. The script automates the process of logging into Twitter, searching for Elon Musk's profile, and extracting tweet data such as user tags, timestamps, tweet content, reply counts, retweet counts, and like counts. The scraped data is then structured into a Pandas DataFrame for further analysis or export.

Features:
Automated Tweet Scraping: The script automates the process of navigating Twitter's interface and extracting tweets related to Elon Musk.
Dynamic Content Handling: It handles dynamic web content loading on Twitter to ensure comprehensive scraping of tweets.
Data Extraction: The script extracts various attributes of each tweet, including user tags, timestamps, tweet content, reply counts, retweet counts, and like counts.
Scalability: Designed to handle large volumes of tweets by continuously scrolling the page and scraping new tweets until no new ones are found.
Data Processing: The scraped tweet data is structured into a Pandas DataFrame, enabling easy manipulation, analysis, and export.








## code

import selenium

from selenium import webdriver

from selenium.webdriver.chrome.options import Options

from selenium.webdriver.common.by import By

from selenium.webdriver.common.keys import Keys

from time import sleep

from selenium.webdriver.support.ui import WebDriverWait

from selenium.webdriver.support import expected_conditions as EC


PATH = "/Users/apple/Downloads/chromedriver-mac-x64"

### Create ChromeOptions object
chrome_options = Options()

### Set the executable path through binary_location 
chrome_options.binary_location = PATH

### Create WebDriver instance with ChromeOptions
driver = webdriver.Chrome(options=chrome_options)
driver.get("https://twitter.com/login")

subject = "Elon Musk"



#username = driver.find_element(By.XPATH,"//input[@type ='text']")

username_locator = (By.XPATH, "//input[@type ='text']")

username = WebDriverWait(driver, 10).until(EC.presence_of_element_located(username_locator))

username.send_keys("scrappingd12213")

next_button = driver.find_element(By.XPATH, "//span[contains(text(),'Next')]")

next_button.click()


password_locator = (By.XPATH,"//input[@name='password']")

password = WebDriverWait(driver, 10).until(EC.presence_of_element_located(password_locator))

password.send_keys('Immohit96@')

log_in = driver.find_element(By.XPATH,"//span[contains(text(),'Log in')]")

log_in.click()

search_box_locator = (By.XPATH,"//input[@data-testid='SearchBox_Search_Input']")

search_box = WebDriverWait(driver, 10).until(EC.presence_of_element_located(search_box_locator))

search_box.send_keys(subject)

search_box.send_keys(Keys.ENTER)

sleep(3)

people = driver.find_element(By.XPATH,"//span[contains(text(),'People')]")

people.click()

profile_locator = (By.XPATH,"//*[@id='react-root']/div/div/div[2]/main/div/div/div/div/div/div[3]/section/div/div/div[1]/div/div/div/div/div[2]/div[1]/div[1]/div/div[1]/a/div/div[1]/span/span[1]")

profile = WebDriverWait(driver, 10).until(EC.presence_of_element_located(profile_locator))

profile.click()


UserTags=[]

TimeStamps=[]

Tweets=[]

Replys=[]

reTweets=[]

Likes=[]


while True:

    articles = driver.find_elements(By.XPATH, "//article[@data-testid='tweet']")
    
    for article in articles:
    
        UserTag_locator = (By.XPATH, ".//div[@data-testid='User-Name']")
        
        UserTag = WebDriverWait(article, 10).until(EC.presence_of_element_located(UserTag_locator)).text
        
        UserTags.append(UserTag)

        TimeStamp_locator = (By.XPATH, ".//time")
        
        TimeStamp_element = WebDriverWait(article, 10).until(EC.presence_of_element_located(TimeStamp_locator))
        
        TimeStamp = TimeStamp_element.get_attribute('datetime')
        
        TimeStamps.append(TimeStamp)

        Tweet_locator = (By.XPATH, ".//div[@data-testid='tweetText']")
        
        Tweet = WebDriverWait(article, 10).until(EC.presence_of_element_located(Tweet_locator)).text
        
        Tweets.append(Tweet)

        Reply_locator = (By.XPATH, ".//div[@data-testid='reply']")
        
        Reply = WebDriverWait(article, 10).until(EC.presence_of_element_located(Reply_locator)).text
        
        Replys.append(Reply)

        reTweet_locator = (By.XPATH, ".//div[@data-testid='retweet']")
        
        reTweet = WebDriverWait(article, 10).until(EC.presence_of_element_located(reTweet_locator)).text
        
        reTweets.append(reTweet)

        Like_locator = (By.XPATH, ".//div[@data-testid='like']")
        
        Like = WebDriverWait(article, 10).until(EC.presence_of_element_located(Like_locator)).text
        
        Likes.append(Like)

    driver.execute_script('window.scrollTo(0, document.body.scrollHeight);')
    
    sleep(3)
    
    new_articles = driver.find_elements(By.XPATH, "//article[@data-testid='tweet']")
    
    if len(new_articles) == len(articles):
    
        break
        
        
#print(len(Tweets))
        
import pandas as pd

    
df = pd.DataFrame(zip(UserTags,TimeStamps,Tweets,Replys,reTweets,Likes) 
                  ,columns=['UserTags','TimeStamps','Tweets','Replys','reTweets','Likes'])

    
#df.to_excel("/Users/apple/Desktop/DOWNLOADED NOTES//promote_live.xlsx")
