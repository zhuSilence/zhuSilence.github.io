### 通过Spring上下文获取指定名称的Bean

在进行Spring或者SpringBoot项目开发的时候，往往有很多情况我们需要在工具类或者其他
非Spring注解的类中用到某个注解类（Service），这个时候就比较麻烦了，因为当前类不是
通过Spring注解来的，也就是类名上面没有配置注解，如果现在要用到其他注解类就不行了。
很多人第一反应就是new一个需要的注解类，但是这样是不行的，运行的时候会报空指针异常
所以这里就需要先获取Spring的上下文了，通过Spring的上下文，然后在上下文中就可以拿到
指定名称的注解类了。。。示例如下

```
package com.coocaa.salad.node.interceptor;

import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;
import org.springframework.stereotype.Component;

/**
 * <br>
 * <b>功能：</b>设置Spring的上下文<br>
 * <b>作者：</b>silence<br>
 * <b>日期：</b>2016-11-16 23:49<br>
 * <b>详细说明：</b>获取Spring上下文中指定名称的Bean<br>
 */
@Component
public class ApplicationContextProvider implements ApplicationContextAware {

    private static ApplicationContext context;

    private ApplicationContextProvider() {
    }

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        context = applicationContext;
    }

    public static Object getBean(String name, Class clazz) {
        return context.getBean(name, clazz);
    }
}

```

用法如下：

```
private AdScheduleService scheduleService = (AdScheduleService) ApplicationContextProvider.getBean("adScheduleService", AdScheduleService.class);
```

> 其中AdScheduleService就是一个注解类，在进行强转一下就可以了，getBean()方法的第一个参数是注解类的名称，默认是注解类名称的第一个字母小写，可以自己在注入的时候指定，第二个参数就是该注解类
