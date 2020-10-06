# Selenium with Python

[ref: https://pylessons.com/Selenium-with-python-basic-introduction/]



##### Introduction tutorial part #1

Web UI Automation means the automatic execution of the actions performed in a web browser window like navigating to a website, filling forms that include dealing with text boxes, buttons and drop downs, submitting the forms, browsing through web pages, handling pop-ups and so on. Selenium WebDriver is the one that can automate all these tasks. It can interact with all types of Web browsers available till date like Firefox, Internet Explorer, Safari, Chrome, Opera and etc.

Selenium is an open source and its library is available in different programming languages to perform the Web UI Automation testing, and Python is one of them.

Selenium WebDriver Client Library for Python enables us to utilize all the features available with Selenium WebDriver and interact with Selenium Standalone Server to perform Automated testing (both remote and distributed testing) of browser-based applications.

I should mention that Selenium Webdriver library is compatible with a series of Python versions that includes Python 2.6, 2.7, and 3.2-3.7. Almost all Python versions.

So I should mention that installing Selenium Webdriver on Python is as simple as doing that with other libraries, simply use following command:

```bash
pip install selenium
```



After installing Selenium, you should decide which browser you will use for your script. When I am creating something with Selenium, I am using Firefox, but everything should work same way on Chrome, so it's up to you what you'll use. When you decided what browser you will use, you should download selenium driver for your browser. Open [**THIS**](https://www.seleniumhq.org/download/) link find GeckoDriver for Firefox or Chrome driver for Google Chrome, or choose whatever you are using. If you installed browser to default location these drivers should be placed in same folder where you are writing your selenium script, other way you should add them to your environment variables. I am not sure how it works on Linux or iOS, but it shouldn't be hard to find it out.

So bellow is my basic example script where we open a browser, going to "www.google.com" website and searching for Pylessons.com, next we are taking 10 of found elements and printing them out. Finally we are closing our browser. Detailed code explanation is bellow example.

```python
from selenium import webdriver

# create a new Firefox session
driver = webdriver.Firefox()
driver.implicitly_wait(30)
driver.maximize_window()

# Navigate to the application home page
driver.get("http://www.google.com")

# get the search textbox
search_field = driver.find_element_by_name('q')

# enter search keyword and submit
search_field.send_keys("Pylessons.com")
search_field.submit()

# get the list of elements which are displayed after the search
# currently on result page using find_elements_by_class_name method
lists = driver.find_elements_by_class_name("LC20lb")

# get the number of elements found
print("Found "+str(len(lists))+" searches:")

# print not more than 10 list elements
i = 0
for listiems in lists:
    print(listiems.text)
    i += 1
    if(i > 10):
        break

# end session and close the browser window
driver.quit()
```



##### Decoding The Above Script

Let’s discuss the script line by line to get a better understanding of the Selenium WebDriver statements in brief. There will be lot more about different Selenium WebDriver functions in upcoming tutorials.

##### Step 1.

The selenium webdriver module implements the classes that support different browsers including Firefox, Chrome, Internet Explorer, Safari, others and RemoteWebDriver too to test on browsers available on remote machines. So We need to import webdriver from the Selenium package to use the Selenium WebDriver methods as:

```python
from selenium import webdriver
```

##### Step 2.

Next, we need the object of the browser which we’ll use to load the web pages. The browser object provides a programmable interface to communicate with the browser using the Selenium commands. In the test script, we are using Firefox. We can create an instance of the Firefox as shown in the following code:

```python
driver = webdriver.Firefox()
```

On executing above statement, a new Firefox window will launch. We are making the following settings for the driver instance:

```python
driver.implicitly_wait(30)
driver.maximize_window()
```

This way we are configuring a timeout for Selenium to launch browser or open new tab in 30 seconds. Next statement maximizes the browser to full window.

##### Step 3.

Next I am navigating to "http://www.google.com" passing the given URL to the driver.get() method. After making a call to the get() method, Webdriver waits until the page gets rendered in the browser window and sends the control back to the script.

After the page gets loaded, Selenium will interact with various elements on the page. Next, in the test script, we will be looking at different Selenium WebDriver functions that search an HTML object, send a text to the web component, simulate keypress event, click buttons and select from drop downs, etc. Let’s see all these functions getting used in other steps.

##### Step 4.

First of all, we’ll locate the Google Search textbox to supply the text input for the Search. The Search text box has a name attribute as "q", and you can identify it from the code given below:

```python
search_field = driver.find_element_by_name('q')
```

##### Step 5.

After locating the Search text box, we are trying to interact with the textbox element by using the send_keys() method to provide a new value. Subsequently calling the submit() method will forward the search request for processing.

```python
search_field.send_keys("Pylessons.com")
search_field.submit()
```

After submitting the search request, Firefox driver will display the result page returned by Google. The result page shows a list of entries that match the searched text. Each of the entry div element has a class of "LC20lb" and can be accessed using "find_elements_by_class_name" method. This way we'll get a list of element.

```python
lists = driver.find_elements_by_class_name("LC20lb")
```

The list of items may expand to to many pages, so we are restricting our code to print first ten entries captured:

```python
i = 0
for listiems in lists:
    print(listiems.text)
    i += 1
    if(i > 10):
        break
```

------

##### Introduction tutorial part #2

We will begin with starting browser function. So we call this function with argument "browser=1" when we'll use our default browser and "browser=2" when we want to use our custom browser. I tried this code with [Google Chromium](https://www.chromium.org/)Google Chromium, but you can try whatever you want, just write correct path to browser location and browser driver. I commented few lines of code, if you want you can remove comment sign from them, but I wont use them in my code. And at the last line we return "driver" because we'll use this in all our future functions to control a browser. We call StartBrowser() function from main code, when we want to start browsing session.

```python
def StartBrowser(browser=1, browser_location='', driver_path=''):
    if browser == 1:
        driver = webdriver.Firefox()
    if browser == 2:
        options = webdriver.ChromeOptions()
        options.binary_location = browser_location
        driver = webdriver.Chrome(executable_path=driver_path, chrome_options=options)
    #driver.implicitly_wait(30)
    #driver.set_window_size(1920,1080)
    #driver.maximize_window()
    return driver
```



Mostly when we are browsing something on internet we would like to use new tabs, so we'll write a function to open it. Selenium webdriver don't have integrated command to open new tabs on internet, so we'll write our own by using script. Before opening new tab we are checking how many tabs we have currently opened: "len(driver.window_handles)". Then we execute our script to open a new tab, and we are waiting for it, by every 500ms checking our tabs count, when our tabs count increase by one it means that new tab opened and we may switch to it:

```
def OpenNewTab(driver, Link):
    window_count = len(driver.window_handles)
    driver.execute_script('''window.open("'''+Link+'''","_blank");''')
    while len(driver.window_handles) != window_count+1:
        time.sleep(0.5)
    driver.switch_to.window(driver.window_handles[-1])
```



And lastly, we would usually we would like to know how to close these tabs. It's quite easy, we will write a new function for that, with simple command: "driver.close()". In this function we'll handle our first exception, which occurs when we are trying close not existing window. For example when we close one window and we trying to close it again, we get this exception:

```
def DriverClose(driver):
    try:
        driver.close()
    except NoSuchWindowException:
        print("NoSuchWindowException in driver.close()")
        return False
    return True
```



Final tutorial code:

```
from selenium import webdriver
import time
from selenium.common.exceptions import NoSuchWindowException

def DriverClose(driver):
    try:
        driver.close()
    except NoSuchWindowException:
        print("NoSuchWindowException in driver.close()")
        return False
    return True

def StartBrowser(browser=1, browser_location='', driver_path=''):
    if browser == 1:
        driver = webdriver.Firefox()
    if browser == 2:
        options = webdriver.ChromeOptions()
        options.binary_location = browser_location
        driver = webdriver.Chrome(executable_path=driver_path, chrome_options=options)
    #driver.implicitly_wait(30)
    #driver.set_window_size(1920,1080)
    #driver.maximize_window()
    return driver

def OpenNewTab(driver, Link):
    window_count = len(driver.window_handles)
    driver.execute_script('''window.open("'''+Link+'''","_blank");''')
    while len(driver.window_handles) != window_count+1:
        time.sleep(0.5)
    driver.switch_to.window(driver.window_handles[-1])

browser_location = "C:/Users/HOME/Desktop/chrome-win/chrome.exe"
driver_path = "C:/Users/HOME/Desktop/chrome-win/chromedriver.exe"
driver = StartBrowser(2, browser_location, driver_path)
time.sleep(1)
driver.get("https://www.google.com")
time.sleep(1)
OpenNewTab(driver, "https://www.google.com")
```



In above code we are starting a new browser session with "driver = StartBrowser(2, browser_location, driver_path)" line, if you would use it with Firefox browser, change it to "driver = StartBrowser(1)". Then we open google search page and in one second we open same page in a new tab. If you would like to see how we close these tabs, check my YouTube video tutorial.