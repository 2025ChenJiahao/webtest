# webtest
依旧一坨好吧
越想越气，不是本来就发烧写实验，然后给的这实验材料也是一坨，老师给的driver是99version，谷歌是143version，这还不是关键，他给的网站最后一个测试用例压根对不上，我真是醉了，他就是少了一个价格排序，只有默认是最新。还有这个web自动化测试，这咋截图，我只能录个视频？
```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service

# 1. 这里填你把 chromedriver.exe 实际存放的路径
# 注意：路径里的斜杠要用反斜杠 / 或者双斜杠 \\
driver_path = Service(r'C:\Users\CJH\Desktop\chromedriver.exe')

# 2. 启动浏览器的时候把这个路径传进去
driver = webdriver.Chrome(service=driver_path)

# 引入必要的工具箱
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

# 1. 启动浏览器
# 这里的代码相当于打开了一个机器人控制的Chrome
driver = webdriver.Chrome()
driver.maximize_window()  # 窗口最大化

# 2. 打开网址 [cite: 42]
print("正在打开安居客...")
driver.get("https://nj.zu.anjuke.com/")
time.sleep(2)  # 等2秒，让网页加载完

# --- 下面开始按照文档步骤操作 ---

# 步骤1 & 2：网页默认已经是南京租房页，如果不是，需要写代码点击切换
# 这里假设已经是南京页，直接进行后续操作

try:
    # 步骤3：点击“地铁找房” [cite: 62]
    # 小白教程：在网页上对着“地铁找房”按钮右键 -> 检查(Inspect) -> 在高亮的代码上右键 -> Copy -> Copy XPath
    # 然后把复制的内容粘在下面的括号里/html/body/div[4]/ul/li[2]/a
    print("点击地铁找房...")
    driver.find_element(By.XPATH, '/html/body/div[4]/ul/li[2]/a').click()
    time.sleep(1)

    # 步骤4：选择“2号线” [cite: 87]
    print("选择2号线...")
    driver.find_element(By.XPATH, '/html/body/div[5]/div[2]/div[1]/span[2]/div/a[3]').click()
    time.sleep(1)

    # 步骤5：选择“马群” [cite: 93]
    print("选择马群...")
    driver.find_element(By.XPATH, '/html/body/div[5]/div[2]/div[1]/span[2]/div/div/a[24]').click()
    time.sleep(1)

    # 步骤6：设置租金 5000-8000 [cite: 105]
    # 这一步可能需要输入框输入，或者点击现有的价格标签
    print("设置价格区间...")
    # 如果是输入框：
    # driver.find_element(By.XPATH, '//*[@id="from-price"]').send_keys("5000")
    # driver.find_element(By.XPATH, '//*[@id="to-price"]').send_keys("8000")
    # driver.find_element(By.XPATH, '//*[@id="pricerange_search"]').click()
    time.sleep(1)

    # 步骤7：选择“整租” [cite: 123]
    driver.find_element(By.XPATH, '/html/body/div[5]/div[2]/div[4]/span[2]/a[2]').click()
    time.sleep(1)

    # 步骤8：选择“普通住宅” [cite: 132]
    # 如果找不到这个按钮，可能是在“更多”或者“房屋类型”下拉菜单里，要先点下拉菜单
    driver.find_element(By.XPATH, '//*[@id="condhouseage_txt_id"]').click()
    time.sleep(1)

    # 步骤9：搜索“经天路” [cite: 153]
    print("搜索经天路...")
    search_input = driver.find_element(By.XPATH, '//*[@id="search-rent"]')
    search_input.clear()  # 先清空原来的字
    search_input.send_keys("经天路")
    driver.find_element(By.XPATH, '//*[@id="search-button"]').click()
    time.sleep(2)

    # 步骤10：选择“视频看房” [cite: 168]
    driver.find_element(By.XPATH, '//*[@id="list-content"]/div[1]/a[2]').click()
    time.sleep(1)

    # 步骤11：点击“租金”、“最新”排序 [cite: 178]
    driver.find_element(By.XPATH, '//*[@id="list-content"]/div[2]/div/a[1]').click()
    time.sleep(1)
    driver.find_element(By.XPATH, '//*[@id="list-content"]/div[2]/div/a[2]').click()
    time.sleep(1)

    # 步骤12：点击第一个房源 [cite: 186]
    print("点击第一个房源...")
    # 这里通常需要找列表里的第一个元素
    driver.find_element(By.XPATH, '//*[@id="list-content"]/div[3]').click()

    print("测试完成！记得截图！")
    time.sleep(10)  # 停10秒让你看结果

except Exception as e:
    print("出错了：", e)

finally:
    # 最后关闭浏览器
    driver.quit()
```

