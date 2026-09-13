---
title: IEEE论文爬虫及数据统计
date: 2022-03-19 13:51:48
tags:
  - spider 
  - PyQt
  - 多线程
categories:
  - DIY
footnotes: true
---

## 1. IEEE论文爬虫

爬虫代码网上有很多了，这部分是直接用的网上可以跑通的[^1]。使用的时候直接调用 `get_article_info()`，其中参数 `conferenceID` 需要手动在 IEEE 上查询会议的 ID 号，参数 `saceFileName` 为希望保存的 `csv` 文件名。

```python
# 获取issueNumber
def get_issueNumber(conferenceID):
    """
    Get the issueNumber from the website.
    """
    conferenceID = str(conferenceID)
    gheaders = {
        'Referer': 'https://ieeexplore.ieee.org/xpl/conhome/'+conferenceID+'/proceeding',
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36'
    }
    md_url = 'https://ieeexplore.ieee.org/rest/publication/home/metadata?pubid='+conferenceID
    md_res = requests.get(md_url, headers = gheaders)
    md_dic = json.loads(md_res.text)
    issueNumber = str(md_dic['currentIssue']['issueNumber'])
    return issueNumber

# 爬取论文及其下载链接
def get_article_info(conferenceID, saveFileName):
    """
    Collect the published paper data, and save into the csv file "saveFileName".
    """
    # 获取issueNumber
    issueNumber = str(get_issueNumber(conferenceID))
    conferenceID = str(conferenceID)

    # 记录论文数据
    dataframe = pd.DataFrame({})
    paper_title = []
    paper_author = []
    paper_year = []
    paper_citation = []
    paper_abstract = []
    paper_ieee_kwd = []

    # 从第一页开始下载
    pageNumber = 1
    count = 0
    while(True):
        # 获取会议文章目录
        toc_url = 'https://ieeexplore.ieee.org/rest/search/pub/'+conferenceID+'/issue/'+issueNumber+'/toc'
        payload = '{"pageNumber":'+str(pageNumber)+',"punumber":"'+conferenceID+'","isnumber":'+issueNumber+'}'
        headers = {
            'Host': 'ieeexplore.ieee.org',
            'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.190 Safari/537.36',
            'Referer': 'https://ieeexplore.ieee.org/xpl/conhome/'+conferenceID+'/proceeding?pageNumber='+str(pageNumber),
        }
        toc_res = requests.post(toc_url, headers = headers, data=payload)
        toc_dic = json.loads(toc_res.text)
        try:
            articles = toc_dic['records']
        except KeyError:
            break
        else:
            for article in articles:
                title = article['highlightedTitle']
                paper_link = IEEE_root_url + article['htmlLink']
                paper_info = requests.get(url=paper_link, headers=headers, timeout=10)
                soup = BeautifulSoup(paper_info.text, 'lxml')                  # 解析
                # 正则表达式 创建模式对象
                pattern = re.compile(r'xplGlobal.document.metadata=(.*?)"};', re.MULTILINE | re.DOTALL)
                script = soup.find("script", text=pattern)                     # 根据模式对象进行搜索
                try:
                    res_dic = pattern.search(script.string).group(1)+'"}'      # 配合search找到字典，匹配结尾字符串，降低文章摘要中也出现这种字符串的概率
                    # 解析异常，一般是因为文章 abstract 中出现了字符串 '"};'
                    json_data = json.loads(res_dic)                            # 将json格式数据转换为字典
                except Exception as e:
                    print(pattern.search(script.string).group(0))
                    print(res_dic)
                # 保存文章信息
                paper_title.append(title)
                paper_year.append(json_data['publicationYear'])
                print(json_data.keys())
                #a = input('input anything...')
                if 'author' in json_data.keys():
                    paper_author.append(json_data['author'])
                else:
                    paper_author.append(None)
                if 'abstract' in json_data.keys():
                    paper_abstract.append(json_data['abstract'])
                else:
                    paper_abstract.append(None)
                if 'keywords' in json_data.keys():
                    paper_ieee_kwd.append(json_data['keywords'][0]['kwd'])       # ieee有三种 key words
                else:
                    paper_ieee_kwd.append(None)
                count=count+1
                #link = 'https://ieeexplore.ieee.org/stampPDF/getPDF.jsp?tp=&arnumber='+article['articleNumber']+'&ref='
                #alf.write(title.replace('\n','')+'>_<'+link+'\n')
            
            # 写入csv文件
            dataframe = pd.DataFrame({'title':paper_title, 'year':paper_year, 'abstract':paper_abstract, 'key words':paper_ieee_kwd})
            dataframe.to_csv(saveFileName, index=True, sep=',')
            print('Page ', pageNumber, ', total ', count, 'papers.')
            pageNumber = pageNumber+1
            # 停一下防禁ip
            import time
            time.sleep(3)

    # 写入csv文件
    dataframe = pd.DataFrame({'title':paper_title, 'year':paper_year, 'abstract':paper_abstract, 'key words':paper_ieee_kwd})
    dataframe.to_csv(saveFileName, index=True, sep=',')
    return
```



## 2. IEEE论文数据统计





## 3. 写一个图形界面

### 3.1 弹出提示窗口

在写代码过程中有时候需要测试功能是否成功实现，于是想要加一个弹出窗口的函数可以显示调试信息，用以验证想要的功能是否正常实现。主要难点在于根据内容自动调整窗口大小，以获得较好的显示效果。

采用的方法是利用 `QLabel.adjust()` 函数获取文本显示的宽度，并据此调整窗口的大小[^2]。

```python
from PyQt6.QtWidgets import (QWidget, QDialog, QLabel, QPushButton)
from PyQt6.QtCore import (QSize, QRect)

class PaperCollector(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()
        
    def initUI(self):
        self.dialog_btn = QPushButton('Click')
        self.dialog_btn.clicked.connect(self.click_callback)
        self.setGeometry(300, 300, 300, 200)
        self.setWindowTitle('IEEE paper collector (by Glooow)')
        self.show()

    def click_callback(self):
        self.show_dialog('You clicked me!')
        
    def show_dialog(self, info):
        """
        Pop up dialogs for debug.
        """
        hint_dialog = QDialog()
        hint_dialog.setWindowTitle('Hint info')
        #hint_dialog.setWindowModality(PyQt6.QtCore.Qt.NonModal)

        hint_info = QLabel(info, hint_dialog)
        hint_info.adjustSize()
        padding = 20
        max_width = 360
        # set the maximum width
        if hint_info.size().width() > max_width:
            hint_info.setGeometry(QRect(0, 0, max_width, 80))
            hint_info.setWordWrap(True)
        hint_info.move(padding, padding)

        hint_dialog.resize(hint_info.size() + QSize(padding*2, padding*2))
        hint_dialog.exec()
```



### 3.2 文本框显示爬取日志

我希望在窗口中增加一个文本框，将爬取过程中的日志信息打印出来，便于用户实时监测。

采用的思路是定义一个 `logging.Logger`，将其日志信息同时输出到窗口的文本框和控制台中打印，通过自定义 `logging.Handler` 可以实现这一功能[^3][^5][^6]。实现方式为：

1. 继承 `logging.Handler` 类，并初始化阶段将整个窗口(`QWidget`类)作为参数传入，便于后续修改窗口的信息；

2. 自定义实现 `emit` 函数，在 `emit` 函数中将 log 信息同时输出到窗口文本框、打印到控制台；

3. 创建 `logger` 的时候设置 Handler[^4]

   ```python
   ex = PaperCollector()
   logger = logging.getLogger("logger")
   handler = LogHandler(ex)
   logger.addHandler(handler)
   ```

下面是这部分功能相关的代码。

```python
import logging

class LogHandler(logging.Handler):
    def __init__(self, parent):
        super().__init__()
        self.parent = parent

    def emit(self, record):
        try:
            print(self.format(record))
            self.parent.print_log(self.format(record))
            QApplication.processEvents()
        except Exception:
            self.handleError(record)
            
class PaperCollector(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()
        
    def initUI(self):
        """
        Define the UI playout.
        """
        # button to start crawing
        self.startCrawling_button = QPushButton('Start')
        self.startCrawling_button.setToolTip('Click and wait for collecting published paper data.')
        self.startCrawling_button.clicked.connect(self.start_collect_paper)
        # print log
        self.process = QTextEdit(readOnly=True)
        self.process.setFont(QFont("Source Code Pro",9))
        
        grid = QGridLayout()
        grid.setSpacing(10)
        grid.addWidget(self.startCrawling_button, 1, 0)
        grid.addWidget(self.process, 2, 0, 3, 3)
        self.setLayout(grid)
        
        self.setGeometry(300, 300, 700, 300)
        self.setWindowTitle('IEEE paper collector (by Glooow)')
        self.show()
        
    def start_collect_paper(self):
        global logger
        #self.show_dialog('start!')
        get_article_info(self.conferenceID_edit.text(), self.saveFile_edit.text(), logger)
        
    def print_log(self, s):
        self.process.append(s)
        
logger = None

def main():
    app = QApplication(sys.argv)
    ex = PaperCollector()

    global logger
    logger = logging.getLogger("logger")
    logger.setLevel(logging.INFO)
    formater = logging.Formatter(fmt="%(asctime)s [%(levelname)s] : %(message)s"
                ,datefmt="%Y/%m/%d %H:%M:%S")
    handler = LogHandler(ex)
    handler.setFormatter(formater)
    logger.addHandler(handler)

    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

爬取论文的主函数如下，其中一个参数为 `logger`，在函数内部需要打印日志信息的地方添加 `logger.info(...)` 即可。

```python
def get_article_info(conferenceID, saveFileName, logger):
    logger.info('collecting paper......')
```



### 3.3 多线程避免卡顿

上述打印日志的方法不能做到实时输出信息到窗口文本框，而是会等到所有论文爬取完毕之后再一股脑的更新，这是因为PyQt的界面线程是主线程，当爬虫开始工作时，也是运行在主线程中，这时主界面就无法更新，看起来就像是卡死了。解决方法就是开一个子线程运行爬虫工作[^7]。

具体实现细节为：

1. 新建类 `SpiderThread` 继承 `QObject`，自定义 `run` 函数，在其中运行爬虫程序；
2. 在 `SpiderThread` 类中定义一个 `_spider_finish = pyqtSignal()`，该信号用于告知主线程爬虫子线程已完成工作
3. 在 `PaperCollector` 类中定义一个 `_start_spider = pyqtSignal(str, str, logging.Logger)`，该信号用于启动爬虫子线程[^8][^9]；
4. 通过 `pyqtSignal.connect` 分别将各个信号连接到对应的槽（处理函数）上；

下面是这部分功能相关的代码。

```python
from PyQt6.QtCore import (QObject, pyqtSignal, QThread)

class SpiderThread(QObject):
    _spider_finish = pyqtSignal()

    def __init__(self):
        super().__init__()
        self.flag_running = False

    def __del__(self):
        print('>>> __del__')

    def run(self, conference_ID, save_filename, logger):
        get_article_info(conference_ID, save_filename, logger)
        self._spider_finish.emit()

class PaperCollector(QWidget):
    _start_spider = pyqtSignal(str, str, logging.Logger)
    
    def __init__(self):
        super().__init__()
        self.initUI()
        #sys.stdout = LogStream(newText=self.onUpdateText)

        self.spiderT = SpiderThread()
        self.thread = QThread(self)
        self.spiderT.moveToThread(self.thread)
        self._start_spider.connect(self.spiderT.run)        # 只能通过信号槽启动线程处理函数
        self.spiderT._spider_finish.connect(self.finish_collect_paper)
        
    def start_collect_paper(self):
        if self.thread.isRunning():
            return
        
        self.startCrawling_button.setEnabled(False)
        self.startCrawling_button.setToolTip('I\'m trying very hard to collect papers >_<')
        # 先启动QThread子线程
        self.thread.start()
        # 发送信号，启动线程处理函数
        # 不能直接调用，否则会导致线程处理函数和主线程是在同一个线程，同样操作不了主界面
        global logger
        self._start_spider.emit(self.conferenceID_edit.text(), self.saveFile_edit.text(), logger)

    def finish_collect_paper(self):
        self.startCrawling_button.setEnabled(True)
        self.startCrawling_button.setToolTip('Click and wait for collecting published paper data ^o^')
        self.thread.quit()
        
    def stop_collect_paper(self):
        if not self.thread.isRunning():
            return
        self.thread.quit()      # 退出
        self.thread.wait()      # 回收资源
        self.show_dialog('stop!')
```



### 3.4 流畅中止子线程

有时候我们需要中途停止爬虫工作，比如发现会议ID设置错误、希望先对已经爬取的部分数据进行统计分析等。在上面的实现中，尽管线程正常运行很流畅，但是如果在爬虫运行中途点击停止按钮，程序就会卡死。

在原本的爬虫脚本中，`get_article_info()` 函数内部的爬虫采用了 `while(True)` 死循环，主线程中直接用 `self.thread.quit()` 强制退出，从控制台来看这样确实可以停掉，但是Qt窗口却总是会卡死。原因我也不太清楚，采用的解决方法是：

1. 定义一个爬虫类 `IEEESpider`，设置成员变量 `flag_running`，将函数 `get_article_info` 也设置为类成员函数；
2. 将 `get_article_info` 中的循环改为 `while(self.flag_running)`；
3. 在主线程中想要停止爬虫子线程的时候，只需要首先设置 `flag_running=False`，那么爬虫子线程在当前一次循环结束后就自动结束，这个时候主线程调用 `self.thread.quit()` 就不会导致界面卡死。需要注意的是设置 `flag_running=False` 一定要 `sleep` 一段时间，以保证爬虫子线程能够结束当前循环，否则还是容易卡死。

下面是这部分功能的代码。

```python
class IEEESpider:
    def __init__(self):
        self.flag_running = False
        
    def get_article_info(self, conferenceID, saveFileName, logger):
        while(self.flag_running):
            pass
```

```python
class SpiderThread(QObject):
    def __init__(self):
        super().__init__()
        #self.flag_running = False
        self.ieee_spider = IEEESpider()
        
    def run(self, conference_ID, save_filename, logger):
        self.ieee_spider.flag_running = True
        self.ieee_spider.get_article_info(conference_ID, save_filename, logger)
        self._spider_finish.emit()
        
class PaperCollector(QWidget):
    def stop_collect_paper(self):
        if not self.thread.isRunning():
            return
        self.spiderT.ieee_spider.flag_running = False
        time.sleep(15)
        self.thread.quit()      # 退出
        #self.thread.wait()      # 回收资源
        self.show_dialog('stop!')
```



### 3.5 增加侧边导航栏

前面只有爬取论文的页面，现在我想加上数据分析的页面，那么就需要设置一个侧边导航栏，以切换两种不同的任务。

实现方式为左侧设置多个按钮，右侧添加一个 `QTabWidget()`，将不同的页面设置为子标签页，通过按钮的点击回调函数切换不同的标签页[^10]。

```python
class PaperCollector(QWidget):
    def sidebarUI(self):
        """
        Define the UI playout of sidebar.
        """
        self.sidebar_btn_1 = QPushButton('Collector', self)
        self.sidebar_btn_1.clicked.connect(self.sidebar_button_1)
        self.sidebar_btn_2 = QPushButton('Analyzer', self)
        self.sidebar_btn_2.clicked.connect(self.sidebar_button_2)
        self.sidebar_btn_3 = QPushButton('Reserved', self)
        self.sidebar_btn_3.clicked.connect(self.sidebar_button_3)

        sidebar_layout = QVBoxLayout()
        sidebar_layout.addWidget(self.sidebar_btn_1)
        sidebar_layout.addWidget(self.sidebar_btn_2)
        sidebar_layout.addWidget(self.sidebar_btn_3)
        sidebar_layout.addStretch(5)
        sidebar_layout.setSpacing(20)

        self.sidebar_widget = QWidget()
        self.sidebar_widget.setLayout(sidebar_layout)
        
    def sidebar_button_1(self):
        self.right_widget.setCurrentIndex(0)

    def sidebar_button_2(self):
        self.right_widget.setCurrentIndex(1)

    def sidebar_button_3(self):
        self.right_widget.setCurrentIndex(2)
        
    def initUI(self):
        """
        Define the overall UI playout.
        """
        self.sidebarUI()
        self.spiderUI()
        self.analyzerUI()
        self.reservedUI()
        
        # 多个标签页
        self.right_widget = QTabWidget()
        self.right_widget.tabBar().setObjectName("mainTab")

        self.right_widget.addTab(self.spider_widget, '')
        self.right_widget.addTab(self.analyzer_widget, '')
        self.right_widget.addTab(self.reserved_widget, '')

        # 隐藏标签部件的标签并初始化显示页面
        self.right_widget.setCurrentIndex(0)
        self.right_widget.setStyleSheet('''QTabBar::tab{width: 0; height: 0; margin: 0; padding: 0; border: none;}''')

        # overall layout
        main_layout = QHBoxLayout()
        main_layout.addWidget(self.sidebar_widget)
        main_layout.addWidget(self.right_widget)
        main_layout.setStretch(0, 40)
        main_layout.setStretch(1, 200)
        self.setLayout(main_layout)

        self.setGeometry(300, 300, 850, 300)
        self.setWindowTitle('IEEE paper collector (by Glooow)')
        self.show()
```



### 3.6 next ...

接下来考虑：写数据分析页面 ......

# Referencce

这里关于参考文献的部分，本来我想按照下面格式来写，希望实现的效果是都像[^2][^10]一样，每一条引用列出来的是超链接，而不是直接写出来链接地址，但是我发现除了第[^2][^10]条，其他条这么写话都会像现在的第[^7]条一样，格式会乱，也不知道为什么。有人知道的话可以告诉我嘛 >_<

```
[^1]:[Python爬虫——爬取IEEE论文 - 乐 ShareLe的博客 - CSDN博客](https://blog.csdn.net/wp7xtj98/article/details/112711465)
[^2]:[PyQt 中文教程 (gitbook.io)](https://maicss.gitbook.io/pyqt-chinese-tutoral/)
[^3]:[python日志：logging模块使用 - 知乎](https://zhuanlan.zhihu.com/p/360306588)
[^4]:[python3 自定义logging.Handler, Formatter, Filter模块 - 太阳花的小绿豆的博客 - CSDN博客](https://blog.csdn.net/qq_37541097/article/details/108317762)
[^5]:[python logging output on both GUI and console - stackoverflow](https://stackoverflow.com/questions/41176319/python-logging-output-on-both-gui-and-console)
[^6]:[How to dynamically update QTextEdit - stackoverflow](https://stackoverflow.com/questions/24371274/how-to-dynamically-update-qtextedit)
[^7]:[PyQt - 使用多线程避免界面卡顿 - bailang zhizun的博客 - CSDN博客](https://blog.csdn.net/bailang_zhizun/article/details/109240670)
[^8]:[pyqt 带单个参数/多个参数信号&槽总结 - gong xufei的博客 - CSDN博客](https://blog.csdn.net/gong_xufei/article/details/89786272)
[^9]:[PyQt5 pyqtSignal: 自定义信号传入的参数方法 - Mic28的博客 - CSDN博客](https://blog.csdn.net/qq_39560620/article/details/105711799)
[^10]:[PyQt5 侧边栏布局 • Chang Luo (luochang.ink)](https://www.luochang.ink/posts/pyqt5_layout_sidebar/)
```


[^1]:https://blog.csdn.net/wp7xtj98/article/details/112711465
[^2]:[PyQt 中文教程 (gitbook.io)](https://maicss.gitbook.io/pyqt-chinese-tutoral/)
[^3]:https://zhuanlan.gitbook.io/p/360306588
[^4]:https://blog.csdn.net/qq_37541097/article/details/108317762
[^5]:https://stackoverflow.com/questions/41176319/python-logging-output-on-both-gui-and-console
[^6]:https://stackoverflow.com/questions/24371274/how-to-dynamically-update-qtextedit
[^7]:[PyQt - 使用多线程避免界面卡顿 - bailang zhizun的博客 - CSDN博客](https://blog.csdn.net/bailang_zhizun/article/details/109240670)
[^8]:https://blog.csdn.net/gong_xufei/article/details/89786272
[^9]:https://blog.csdn.net/qq_39560620/article/details/105711799
[^10]:[PyQt5 侧边栏布局 • Chang Luo (luochang.ink)](https://www.luochang.ink/posts/pyqt5_layout_sidebar/)