```Java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
* @Description: Log proxy object generator（dynamicProxy）
**/
public class LogProxyHandler implements InvocationHandler{

    private static final Logger LOG = LoggerFactory.getLogger(LogProxyHandler.class);

    private Object target;

    public LogProxyHandler(Object target) {
        super();
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Object result = null;
        try {
            LOG.info("[Before] Do something...");
            result = method.invoke(target, args);
            LOG.info("[AfterReturning] Do something...");
        } catch (Exception e) {
            LOG.error("Something happened {}", e.getMessage(), e);
        } finally {
            LOG.info("[After] Do something...");
        }
        return result;
    }

    /*
     * 传入目标对象，创建其代理器
     */
    public static Object createProxy(Object target) {
        ClassLoader classLoader = target.getClass().getClassLoader();
        Class<?>[] interfaces = target.getClass().getInterfaces();
        LogProxyHandler logProxyHandler = new LogProxyHandler(target);
        Object proxy = Proxy.newProxyInstance(classLoader, interfaces, logProxyHandler);
        return proxy;
    }

    /*
     * 测试方法
     */
    public static void main(String[] args) {
        BaseServiceImpl baseServiceImpl = new BaseServiceImpl();
        BaseService baseService = (BaseService)LogProxyHandler.createProxy(baseServiceImpl);
        baseService.doSomthing();
    }
}

interface BaseService {
    void doSomthing();
}

class  BaseServiceImpl implements BaseService{
    @Override
    public void doSomthing() {
        System.err.println(".....");
    }
}
```
### 运行结果：
![image](https://s1.ax1x.com/2020/05/28/tZV4YQ.png)