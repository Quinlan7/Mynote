## ant风格





##### AntPathRequestMatcher是什么

> AntPathRequestMatcher是Spring框架中的一个类，用于匹配HTTP请求的URL路径。它可以用来定义需要保护的URL路径，以及匹配这些路径的请求方法（GET、POST、PUT、DELETE等）。在Spring Security中，AntPathRequestMatcher通常用于配置请求授权规则，以确定哪些请求需要进行身份验证和授权。
>
> AntPathRequestMatcher使用Ant风格的路径模式进行匹配，可以使用“？”匹配一个字符，“*”匹配任意数量的字符（包括0个），以及“**”匹配任意数量的目录（包括0个）。例如，"/user/*"可以匹配所有以"/user/"开头的URL路径，而"/user/**"可以匹配所有以"/user/"开头的URL路径以及其后的任意子路径。
>
> AntPathRequestMatcher是一个强大的工具，可以帮助我们轻松地定义和配置请求授权规则，以确保我们的应用程序在安全方面得到充分的保护。





##### ant风格是什么意思

> Ant风格指的是一种通配符表达式语言，它在Java领域广泛应用于路径匹配、文件过滤等场景。这种风格的表达式通常使用特定的通配符来匹配文件或路径中的部分内容，从而实现模式匹配和筛选的功能。
>
> 在Ant风格中，常用的通配符包括：
>
> - “?”：表示匹配一个任意字符；
> - “*”：表示匹配零个或多个字符；
> - “**”：表示匹配任意数量的目录和子目录。
>
> 例如，使用Ant风格的表达式"/user/*"可以匹配所有以"/user/"开头的路径，而"/user/**"可以匹配所有以"/user/"开头的路径及其后的任意子路径。
>
> Ant风格的表达式语言是Spring框架中很多模块中常用的表达式，如Spring MVC的@RequestMapping、Spring Security的AntPathRequestMatcher等。通过使用Ant风格的表达式，可以更加灵活和方便地完成路径匹配和筛选的任务。





##### 为什么叫ant风格

> Ant风格得名自Apache Ant项目，它是一个Java构建工具，用于自动化构建Java项目。Ant提供了一种基于XML文件的构建脚本语言，用于描述项目的构建过程。在Ant的构建脚本中，使用类似于文件路径的通配符来指定要包含或排除的文件，例如“**/*.java”表示匹配任意目录下的所有.java文件。这种通配符表达式语言后来被称为Ant风格的语法。
>
> Ant风格的语法因其简洁、易读和方便的特点，逐渐被引入到其他领域中，如路径匹配、文件过滤等。在Java领域中，Spring框架是使用Ant风格语法的典型例子，例如使用Ant风格的表达式来匹配URL路径、过滤器、扫描类等。因此，Ant风格的语法已经成为Java领域中的一种通用惯例，被广泛使用。







##### 参考

[ant风格](https://www.cnblogs.com/Eddyer/p/5045523.html)