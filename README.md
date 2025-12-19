# webtest
依旧一坨好吧
越想越气，不是本来就发烧写实验，然后给的这实验材料也是一坨，老师给的driver是99version，谷歌是143version，这还不是关键，他给的网站最后一个测试用例压根对不上，我真是醉了，他就是少了一个价格排序，只有默认是最新。还有这个web自动化测试，这咋截图，我只能录个视频？

#### **1.1 实验要求**

**实验目的：** 理解和掌握 Web 功能测试的基本流程与方法 。

**实验内容：** 针对“安居客”租房网站（南京站），依据功能测试需求文档，使用 Python + Selenium 编写自动化测试脚本，模拟用户进行地铁找房、条件筛选、关键词搜索及排序等操作，验证网站功能是否正常 。

**实验环境：**

- 操作系统：Windows 10/11
- 编程语言：Python 3.x
- 测试工具：Selenium WebDriver, ChromeDriver (v143) 
- 浏览器：Google Chrome

#### **1.2 实验过程**

**1. 环境搭建与配置** 安装 Python 及 Selenium 库，下载与浏览器版本（v143）匹配的 ChromeDriver 驱动，并配置驱动路径 。

**2. 自动化测试脚本编写** 根据实验指导手册及需求文档 ，编写如下 Python 脚本实现自动化操作：

#### **1.3 思考总结**

本次实验是我第一次接触 Web 自动化测试，在实验过程中遇到了不少挑战，但也收获了很多：

1. **环境配置的波折：** 一开始在运行代码时，遇到了 `SessionNotCreatedException` 错误。通过查看报错信息，我发现是我的 Chrome 浏览器自动更新到了 v143 版本，但下载的 ChromeDriver 还是旧版本。后来我去谷歌官方实验室下载了对应的驱动，并将其放置在项目文件夹下，成功解决了问题 。
2. **路径语法的坑：** 在代码中指定驱动路径时，我遇到了 `SyntaxError: (unicode error)` 报错。查阅资料后明白是因为 Windows 路径中的反斜杠 `\` 被 Python 当作转义字符处理了。我学会了在字符串前加 `r` 来声明原始字符串（如 `r'C:\Users...'`），这修正了我的代码错误。
3. **元素定位的技巧：** 实验中最耗时的部分是获取网页元素的 XPath。我学会了使用浏览器 F12 开发者工具中的 "Copy XPath" 功能来精准定位按钮和输入框。

通过这次实验，我不仅完成了对安居客网站的功能测试，更重要的是理解了“驱动”与“浏览器”配合工作的原理，这对我后续学习软件测试有很大帮助 。



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

