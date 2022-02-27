import datetime
from time import sleep
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys

# ---------- CONFIG ----------
username = ""
password = ""
username_microsoft = ""
password_microsoft = ""
delay = 2


# ---------- FUNC -----------
class TimesheetItem:
    def __init__(self, s, e, b, date):
        self.s = s
        self.e = e
        self.b = b
        self.d = datetime.datetime.strftime(datetime.datetime.strptime(date, "%d/%m/%Y"), "%d-%b-%Y")


def tab(count):
    for tab_counter in range(count):
        sleep(0.1)
        tab_actions = ActionChains(driver)
        tab_actions.send_keys(Keys.TAB)
        tab_actions.perform()


def shift_tab(count):
    for tab_counter in range(count):
        sleep(0.1)
        tab_actions = ActionChains(driver)
        tab_actions.key_down(Keys.SHIFT)
        tab_actions.send_keys(Keys.TAB)
        tab_actions.key_up(Keys.SHIFT)
        tab_actions.perform()


# ---------- START ----------
# Selenium launch
driver = webdriver.Chrome()

# Get TS
driver.get("https://to-do.live.com/tasks/")

sleep(delay)

elem = driver.find_element(by=By.ID, value="actionButton")
elem.click()

sleep(delay * 2)

elem = driver.find_element(by=By.NAME, value="loginfmt")
elem.clear()
elem.send_keys(username_microsoft)

elem = driver.find_element(by=By.ID, value="idSIButton9")
elem.click()

sleep(delay * 2)

elem = driver.find_element(by=By.NAME, value="passwd")
elem.clear()
elem.send_keys(password_microsoft)

elem = driver.find_element(by=By.ID, value="idSIButton9")
elem.click()

sleep(delay)

elem = driver.find_element(by=By.ID, value="idSIButton9")
elem.click()

sleep(delay * 4)

elem = driver.find_element(by=By.CSS_SELECTOR, value="[aria-label*='Timesheets']")
elem.click()

sleep(delay)

# Get elements and parse TS
elem = driver.find_elements(by=By.CLASS_NAME, value="taskItem-title")
els = (el.get_attribute('innerHTML').replace("<span>", "").replace("</span>", "").replace(" ", "").replace("[TS]", "")
       for el in elem)

str_els = []
for el in els:
    # Manual parse because I'm too lazy to deal with Regex
    if not el.find("[HiQ]"):
        str_els.append(
            str(el).replace("[HiQ]", "").replace("-", "").replace("(", "").replace(")", "").replace("[", "").replace(
                "]", ""))

ts_els = []
for el in str_els:
    ts_els.append(TimesheetItem(el[0:5], el[5:10], el[10:14], el[14::]))

# Navigate to SC
driver.get("https://qutvirtual4.qut.edu.au/group/staff/home")

sleep(delay)

# Inject credentials
elem = driver.find_element(by=By.NAME, value="username")
elem.clear()
elem.send_keys(username)

elem = driver.find_element(by=By.NAME, value="password")
elem.clear()
elem.send_keys(password)

elem = driver.find_element(by=By.ID, value="kc-login")
elem.click()

sleep(delay)

# Navigate to form
driver.get("https://staffconnect.qut.edu.au/")

sleep(delay * 3)

tab(8)

for i in range(6):
    sleep(0.1)
    actions = ActionChains(driver)
    actions.send_keys(Keys.DOWN)
    actions.perform()

sleep(0.1)

actions = ActionChains(driver)
actions.send_keys(Keys.RETURN)
actions.perform()

sleep(0.1)

actions = ActionChains(driver)
actions.send_keys(Keys.RETURN)
actions.perform()

# Create new entry
tab(15)

sleep(0.1)

actions = ActionChains(driver)
actions.send_keys(Keys.RETURN)
actions.perform()

sleep(delay)

tab(4)

sleep(0.1)

# Insert first date in TS
actions = ActionChains(driver)
actions.send_keys(ts_els[len(ts_els)-1].d)
actions.perform()

sleep(0.1)
tab(1)
sleep(0.1)

tab(11)

sleep(0.1)

actions = ActionChains(driver)
actions.send_keys(Keys.SPACE)
actions.perform()

sleep(0.1)

tab(3)

actions = ActionChains(driver)
actions.send_keys(Keys.ENTER)
actions.perform()

sleep(delay)

tab(11)

# Start input loop
for i in range(min(5, len(ts_els))):
    actions = ActionChains(driver)
    actions.send_keys(ts_els[i].d)
    actions.perform()

    tab(2)

    actions = ActionChains(driver)
    actions.send_keys(ts_els[i].s)
    actions.perform()

    tab(1)

    actions = ActionChains(driver)
    actions.send_keys(ts_els[i].e)
    actions.perform()

    tab(1)

    actions = ActionChains(driver)
    actions.send_keys(ts_els[i].b)
    actions.perform()

    tab(2)

    actions = ActionChains(driver)
    actions.send_keys("SAL")
    actions.perform()

    tab(6)

# Add more rows if needed
if len(ts_els) > 5:
    tab(1)
    for i in range(len(ts_els)-5):
        actions = ActionChains(driver)
        actions.send_keys(Keys.ENTER)
        actions.perform()
        sleep(0.1)
    shift_tab(1)
    for i in range(len(ts_els)-5):
        shift_tab(12)
        sleep(0.1)
    for i in range(5, len(ts_els)):
        actions = ActionChains(driver)
        actions.send_keys(ts_els[i].d)
        actions.perform()

        tab(2)

        actions = ActionChains(driver)
        actions.send_keys(ts_els[i].s)
        actions.perform()

        tab(1)

        actions = ActionChains(driver)
        actions.send_keys(ts_els[i].e)
        actions.perform()

        tab(1)

        actions = ActionChains(driver)
        actions.send_keys(ts_els[i].b)
        actions.perform()

        tab(2)

        actions = ActionChains(driver)
        actions.send_keys("SAL")
        actions.perform()

        tab(6)
