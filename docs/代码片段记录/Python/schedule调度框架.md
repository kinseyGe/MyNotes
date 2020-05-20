
> 安装

```shell
pip install schedule
```
> ### 单任务代码示例

```python
import schedule
import time

def job():
    print("I'm working...")

schedule.every(10).minutes.do(job)
schedule.every().hour.do(job)
schedule.every().day.at("10:30").do(job)
schedule.every(5).to(10).days.do(job)
schedule.every().monday.do(job)
schedule.every().wednesday.at("13:15").do(job)

while True:
    schedule.run_pending()
    time.sleep(1)

```
> schedule其实就只是个定时器。在while True死循环中，schedule.run_pending()是保持schedule一直运行，去查询上面那一堆的任务，在任务中，就可以设置不同的时间去运行。跟crontab是类似的。


- 如果是多个任务运行的话，实际上它们是按照顺序从上往下挨个执行的。如果上面的任务比较复杂，会影响到下面任务的运行时间,其实解决方法也很简单：用多线程/多进程

> ### 多任务代码示例

```
import datetime
import schedule
import threading
import time

def job1():
    print("I'm working for job1")
    time.sleep(2)
    print("job1:", datetime.datetime.now())

def job2():
    print("I'm working for job2")
    time.sleep(2)
    print("job2:", datetime.datetime.now())

def job1_task():
    threading.Thread(target=job1).start()

def job2_task():
    threading.Thread(target=job2).start()

def run():
    schedule.every(10).seconds.do(job1_task)
    schedule.every(10).seconds.do(job2_task)

    while True:
        schedule.run_pending()
        time.sleep(1)
```
