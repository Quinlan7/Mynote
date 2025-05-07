# Spring MVC 小记

## 一、后缀模式匹配

Spring MVC 默认采用“.\*”的后缀模式匹配来进行路径匹配，因此，一个映射到/person路径的控制器也会隐式地映射到/person.*

所以会出现 请求路径 是 http://cmc2soa.jd.com/v2/api/couponSearch/queryGuideSelect.action ，但是代码路径是 /queryGuideSelect 的情况。

##### 参考

[后缀模式匹配](https://hongxingwz.gitbooks.io/spring-mvc/content/hou-zhui-mo-shi-pi-pei.html)