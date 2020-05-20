- 作用：**为每个线程开辟一个独立的空间，使得线程对自己空间中的数据进行操作(数据隔离)。**

- 应用：DButils

    - 获取每个线程的唯一标识
    
    ```
    import threading
    print(threading.get_ident())
    ```

    - 根据字典自定一个类似threading.local的功能
    
    ```
    import threading
    import time
    DIC = {}
    def task(i)
        ident = threading.get_ident()#获取线程的唯一标识
        if ident in DIC:
            DIC[ident]['xxxx'] = i #如果字典中的存在该线程则向其中添加数据i
        else:
            DIC[ident] = {'xxxx':i}#如果字典中不存在该线程则向其中添加该线程以及该线程对应的数据i
        time.sleep(2))
        print(DIC[ident]['xxxx'],i) #延迟两秒并打印隔离后的线程标识以及它对应的数据
        
    for i in range(10):
        t = threading.Thread(target=task,args=(i,))
        t.start()
    ```

    - 根据线程字典定义一个为协程开辟空间进行存储数据，进行数据隔离
    
    ```
    import greenlet
    #将上述的ident = threading.get_ident()修改为：
    ident = greenlet.getcurrent())#获取协程的唯一标识
    ```

    - getattr 当实例化类后再使用.调用的时候回执行此方法
    
    ```
    def __getattr__(self,item):
        pass
    ```
    
    - setattr 当实例化类后再使用.调用后，然后向其中存入值时执行此方法
    
    ```
    def __setattr__(self,key,value):
        pass
    ```

    
    