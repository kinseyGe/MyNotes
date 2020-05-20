```
                                    app = Flask(__name__)
                                            ↓
                                        设置路由
                                            ↓
                                启动socket服务app.run()
                                            ↓
                                第一步调用__call__()方法 
                                            ↓
                                第二步调用wsgi_app()方法_____________________
                                ↙          ↓                                ↘   
    第三步实例化RequestContext类   第六步调用push()方法                           第九步调用full_dispatch_request()方法
                ↓
    第四步调用__init()__方法       第七步实例化SecureCookieSessionInterface()类   第十步调用finalize_request()方法
                ↓
    第五步实例化Request()类        第八步调用open_session()方法                   第十一步调用process_response()方法 
                                                                                
                                                                               第十二步调用save_session方法
```
