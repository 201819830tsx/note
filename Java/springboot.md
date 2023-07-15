## 一.attention

##### 1.spring boot进行定时任务

**基于注解 (@Scheduled)的方式**

启动类中用注解***@EnableScheduling***进行标注，表明此类 存在定时任务。在定时执行的方法之上添加注解

```html
@Scheduled(cron = "0 0 0 * * ?") // 每天午夜执行
```

启动类需要能扫描到定时任务类，否则定时任务启动不起来。不仅需要@Component注解，也需要将启动类位置位于定时任务类之上。

## 注：小知识点

#### 1.ctrl+ F 快速查找命令

#### 2.打开服务services.msc

#### 3.ctrl+ R全文替换单词

#### 4.Java基础中的集合

#### 5.spring中的依赖注入

#### 6.springmvc中的modelAndView

#### 7.netstat -ano |findstr "80"查看对应的端口号

#### 8.taskkill /t /f /im ”TCP号“

#### 9.nohup java -jar your-application.jar > output.log &