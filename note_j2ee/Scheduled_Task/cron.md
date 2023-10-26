# cron



![image-20231018192803581](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310181928651.png)

一个标准的CRON表达式由六个至七个部分组成，分别为：秒（second）、分钟（minute）、小时（hour）、日（day-of-month）、月（month）、周（week）和年（year）。其中，年并不是必需的部分，只有在需要特定年份的任务时才使用。



CRON表达式基本格式为：`* * * * *`，每个星号代表一个字段的取值范围。CRON表达式中的每个字段都可以使用以下符号和数字设置特定的任务执行时间：



+ ？：不生效，日或者周只能指定一个，另一个不生效就是 ？



![image-20231018192832002](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310181928112.png)







### 参考

[Cron表达式详细讲解，平常看一看就记住了](https://juejin.cn/post/7226724826470858810)

[**SpringBoot--定时任务--选型/对比/框架**](https://blog.51cto.com/knifeedge/5268663)