# file

*spring boot项目中的前后端分离中的文件处理内容*

#### 源代码

> ```java
> @PostMapping("/uploadFile")
> public ResponseBean upload(@RequestParam("file") MultipartFile file) throws UnsupportedEncodingException {
>     if (file.isEmpty()) {
>         return new ResponseBean(500,"上传失败，请选择文件",null);
>     }
> 
>     File dest = new File(path + IdGeneratorUtil.uuid()+file.getOriginalFilename());
> 
>     //判断文件父目录是否存在
>     if (!dest.getParentFile().exists()) {
>         dest.getParentFile().mkdirs();
>     }
>     try {
>         file.transferTo(dest);
>         InetAddress inetAddress = InetAddress.getLocalHost();
>         String host = inetAddress.getHostAddress();
>         String path = "http://"+host+":"+port+"/file/"+ URLEncoder.encode(dest.getName(), String.valueOf(StandardCharsets.UTF_8));
>         return new ResponseBean(200,"上传成功",path);
>     } catch (IOException e) {
>         return new ResponseBean(400,"上传失败",null);
>     }
> }
> ```

##### MultipartFile是什么类

> `MultipartFile`是一个接口，它是在Java Web开发中用于处理上传文件的一种数据类型。它属于Spring Framework中的`org.springframework.web.multipart`包。
>
> `MultipartFile`接口提供了一种方便的方式来访问上传的文件的内容和元数据。它定义了许多方法来获取文件的各种属性和操作文件的内容。
>
> 以下是一些常用的`MultipartFile`接口方法：
>
> - `String getName()`: 获取文件表单字段的名称。
> - `String getOriginalFilename()`: 获取上传的原始文件名。
> - `String getContentType()`: 获取文件的内容类型。
> - `boolean isEmpty()`: 检查文件是否为空。
> - `long getSize()`: 获取文件的大小（以字节为单位）。
> - `byte[] getBytes()`: 将文件内容作为字节数组返回。
> - `InputStream getInputStream()`: 获取文件内容的输入流。
> - `void transferTo(File dest)`: 将文件内容保存到指定的目标文件。
>
> 在Spring Web应用程序中，当通过HTTP请求上传文件时，通常会使用`MultipartFile`作为处理文件上传的参数类型。可以通过注解（例如`@RequestParam`或`@RequestPart`）将`MultipartFile`对象绑定到对应的控制器方法参数上。
>
> 需要注意的是，`MultipartFile`是Spring框架提供的特定接口，它简化了文件上传的处理过程，封装了与上传文件相关的操作和属性。

##### 我在控制器中直接接收MultipartFile类型参数，这个参数中都会包含哪些信息

> 当你在控制器中直接接收`MultipartFile`类型参数时，该参数中包含了上传文件的相关信息和内容。以下是`MultipartFile`接口中常用的属性和方法，通过这些属性和方法你可以获取文件的各种信息：
>
> - `String getName()`: 获取文件表单字段的名称。
> - `String getOriginalFilename()`: 获取上传的原始文件名。
> - `String getContentType()`: 获取文件的内容类型。
> - `boolean isEmpty()`: 检查文件是否为空。
> - `long getSize()`: 获取文件的大小（以字节为单位）。
> - `byte[] getBytes()`: 将文件内容作为字节数组返回。
> - `InputStream getInputStream()`: 获取文件内容的输入流。
>
> 通过上述方法，你可以获取文件的名称、文件类型、文件大小等元数据信息。同时，你还可以通过`getBytes()`方法获取文件内容的字节数组，或者通过`getInputStream()`方法获取文件内容的输入流，进而进行文件的读取、存储或处理操作。
>
> 在控制器方法中，Spring框架会自动将上传的文件转换为`MultipartFile`对象，并将其绑定到对应的方法参数上。你可以直接在控制器方法中使用`MultipartFile`参数，然后通过调用该参数的方法来获取文件的各种信息。
>
> 以下是一个示例控制器方法的代码片段，演示了如何使用`MultipartFile`参数和获取文件信息：
>
> ```java
> @PostMapping("/upload")
> public String handleFileUpload(@RequestParam("file") MultipartFile file) {
>     if (!file.isEmpty()) {
>         String originalFilename = file.getOriginalFilename();
>         String contentType = file.getContentType();
>         long fileSize = file.getSize();
>         
>         // 其他文件处理操作，例如保存文件到磁盘、读取文件内容等
>         
>         return "File uploaded successfully.";
>     } else {
>         return "No file uploaded.";
>     }
> }
> ```
>
> 在上述示例中，`handleFileUpload`方法接收一个`MultipartFile`类型的参数`file`，然后通过调用`file`的方法获取文件的原始名称、内容类型和大小。你可以在方法中进行进一步的文件处理操作，根据需要保存文件、读取文件内容或执行其他操作。
>
> 希望以上信息对你有帮助！如果你有任何其他问题，请随时提问。

##### URLEncoder.encode(dest.getName(), String.valueOf(StandardCharsets.UTF_8));spring boot 项目中这段代码有什么用

> 在Spring Boot项目中，`URLEncoder.encode(dest.getName(), String.valueOf(StandardCharsets.UTF_8))`这段代码用于对文件名进行URL编码。
>
> `URLEncoder.encode()`方法是Java中的一个用于URL编码的工具方法。它将给定的字符串进行URL编码，将字符串中的特殊字符转换为URL安全的形式，以便在URL中传输或使用。
>
> 在这个特定的代码片段中，`dest.getName()`获取了文件名（可能包含特殊字符），然后通过`URLEncoder.encode()`方法对文件名进行URL编码。编码后的文件名将在URL中使用，确保URL的有效性和安全性，以避免特殊字符引起的问题，例如空格、特殊符号等。
>
> `StandardCharsets.UTF_8`是指定编码字符集为UTF-8，这是一种常用的字符编码格式，支持大多数语言的字符表示。`String.valueOf()`方法用于将`StandardCharsets.UTF_8`转换为字符串类型，以符合`URLEncoder.encode()`方法的参数要求。
>
> 总的来说，该代码的作用是对文件名进行URL编码，以确保在使用文件名作为URL的一部分时不会引起问题。







#### 源代码

```java
@Configuration
public class ResoureConfig implements WebMvcConfigurer {

    @Value("${file.path}")
    public String path;

    @Value("${file.pathOld}")
    public String pathOld;

    @Value("${file.pathNew}")
    public String pathNew;

    @Value("${file.pathMaterial}")
    public String pathMaterial;
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry){
        System.out.println("addResourceHandlers...");
        registry.addResourceHandler("/file/**").addResourceLocations("file:" + path);
        registry.addResourceHandler("/ofile/**").addResourceLocations("file:" + pathOld);
        registry.addResourceHandler("/nfile/**").addResourceLocations("file:" + pathNew);
        registry.addResourceHandler("/materfile/**").addResourceLocations("file:" + pathMaterial);
    }

}
```



##### WebMvcConfigurer这个是什么类

> `WebMvcConfigurer`是Spring MVC框架中的一个接口，它提供了一种扩展和自定义Web MVC配置的方式。
>
> 在Spring MVC中，`WebMvcConfigurer`接口允许你通过实现该接口并重写其中的方法来自定义和配置Web MVC的行为。你可以使用`WebMvcConfigurer`来注册拦截器、添加资源处理器、配置视图解析器等。
>
> 以下是一些`WebMvcConfigurer`接口中常用的方法：
>
> - `void addInterceptors(InterceptorRegistry registry)`: 注册拦截器，可以在请求处理之前或之后进行拦截操作。
> - `void addResourceHandlers(ResourceHandlerRegistry registry)`: 添加自定义的静态资源处理器，用于映射和处理静态资源文件。
> - `void addViewControllers(ViewControllerRegistry registry)`: 添加视图控制器，用于简化某些页面跳转的配置。
> - `void configureViewResolvers(ViewResolverRegistry registry)`: 配置视图解析器，用于解析视图名称并渲染对应的视图。
> - `void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer)`: 配置默认的Servlet处理，用于处理静态资源的请求。
> - `void configureContentNegotiation(ContentNegotiationConfigurer configurer)`: 配置内容协商策略，用于确定响应的内容类型。
>
> 通过实现`WebMvcConfigurer`接口并重写上述方法，你可以根据自己的需求来自定义和配置Web MVC的行为。在Spring Boot中，你可以创建一个配置类，并使用`@EnableWebMvc`注解来启用Web MVC并将配置类注册为`WebMvcConfigurer`的实现。
>
> 以下是一个简单的示例，展示了如何使用`WebMvcConfigurer`进行自定义配置：
>
> ```java
> @Configuration
> @EnableWebMvc
> public class MyWebMvcConfig implements WebMvcConfigurer {
> 
>     @Override
>     public void addInterceptors(InterceptorRegistry registry) {
>         // 注册拦截器
>         registry.addInterceptor(new MyInterceptor()).addPathPatterns("/**");
>     }
> 
>     @Override
>     public void addResourceHandlers(ResourceHandlerRegistry registry) {
>         // 添加自定义的静态资源处理器
>         registry.addResourceHandler("/static/**").addResourceLocations("classpath:/static/");
>     }
> 
>     @Override
>     public void addViewControllers(ViewControllerRegistry registry) {
>         // 添加视图控制器
>         registry.addViewController("/home").setViewName("home");
>     }
> 
>     // 其他方法...
> 
> }
> ```
>
> 在上述示例中，`MyWebMvcConfig`是一个配置类，通过实现`WebMvcConfigurer`接口并重写相关方法，对Web MVC进行自定义配置。例如，注册了一个拦截器、添加了一个自定义的静态资源处理器，并添加了一个视图控制器。
>
> 需要注意的是，`WebMvcConfigurer`接口提供了许多其他的配置方法，你可以根据需要选择性地重写这些方法来进行自定义配置。
>
> 希望以上信息能帮助到你！如有更多疑问，请随时提问。

[SpringBoot---WebMvcConfigurer详解](https://blog.csdn.net/zhangpower1993/article/details/89016503)

##### ResourceHandlerRegistry这是个什么类

> `ResourceHandlerRegistry`是Spring MVC框架中的一个类，用于注册和配置静态资源的处理器。
>
> 在Spring MVC中，`ResourceHandlerRegistry`类允许你注册自定义的静态资源处理器，用于处理静态资源文件（例如CSS、JavaScript、图片等）。通过注册资源处理器，你可以定义URL路径与实际静态资源文件的映射关系，并配置一些属性来控制资源的缓存、版本管理等行为。
>
> 以下是一些`ResourceHandlerRegistry`类常用的方法：
>
> - `void addResourceHandler(String... pathPatterns)`: 添加静态资源的URL路径模式，用于匹配需要处理的静态资源请求。
> - `ResourceHandlerRegistration addResourceLocations(String... resourceLocations)`: 指定静态资源的存放目录或位置。
> - `ResourceHandlerRegistration setCachePeriod(Integer cachePeriod)`: 设置静态资源的缓存期限，单位为秒。
> - `ResourceHandlerRegistration setCacheControl(CacheControl cacheControl)`: 设置静态资源的缓存控制策略。
> - `ResourceHandlerRegistration resourceChain(Boolean resourceChain)`: 配置是否开启资源链的支持。
> - `ResourceHandlerRegistration addTransformer(ResourceTransformer... transformers)`: 添加资源转换器，用于对静态资源进行转换或处理。
>
> 通过创建一个配置类，并在其中使用`ResourceHandlerRegistry`的方法，你可以自定义和配置静态资源的处理器。该处理器将根据你的配置来处理对应的静态资源请求，并返回相应的资源文件。
>
> 以下是一个简单的示例，展示了如何使用`ResourceHandlerRegistry`注册和配置静态资源的处理器：
>
> ```java
> @Configuration
> public class WebMvcConfig implements WebMvcConfigurer {
> 
>     @Override
>     public void addResourceHandlers(ResourceHandlerRegistry registry) {
>         registry
>             .addResourceHandler("/static/**") // 定义静态资源的URL路径模式
>             .addResourceLocations("classpath:/static/") // 指定静态资源的存放目录
>             .setCachePeriod(3600) // 设置静态资源的缓存期限（单位：秒）
>             .resourceChain(true); // 开启资源链的支持
>     }
> 
>     // 其他配置方法...
> 
> }
> ```
>
> 在上述示例中，`WebMvcConfig`是一个配置类，通过实现`WebMvcConfigurer`接口并重写`addResourceHandlers`方法，来注册和配置静态资源的处理器。静态资源的URL路径模式是`/static/**`，对应的静态资源存放在`classpath:/static/`目录下，设置了缓存期限为3600秒，并开启了资源链的支持。
>
> 需要注意的是，`ResourceHandlerRegistry`类提供了许多其他的配置方法，你可以根据需要选择性地调用这些方法来进行自定义配置。
>
> 希望以上信息对你有所帮助！如果还有其他问题，请随时提问。



##### addResourceHandlers这个方法能详细解释一下吗

> 当使用Spring MVC框架处理Web请求时，`addResourceHandlers`方法用于注册和配置静态资源的处理器，用于处理静态资源文件（例如CSS、JavaScript、图片等）的请求。
>
> 该方法属于`ResourceHandlerRegistry`类，通过调用该方法可以指定静态资源的URL路径模式、资源文件的存放位置和其他相关属性。下面是对`addResourceHandlers`方法的详细解释：
>
> ```java
> void addResourceHandlers(ResourceHandlerRegistry registry)
> ```
>
> - `ResourceHandlerRegistry`: 资源处理器注册器。它是Spring MVC框架中用于注册和配置资源处理器的类。
>
> 该方法允许你通过`ResourceHandlerRegistry`对象来注册静态资源处理器，并定义如何处理静态资源请求。
>
> 使用`addResourceHandlers`方法的一般步骤如下：
>
> 1. 在配置类中实现`WebMvcConfigurer`接口，并重写`addResourceHandlers`方法。
>
> 2. 在`addResourceHandlers`方法中，通过调用`registry.addResourceHandler`方法来指定静态资源的URL路径模式。该模式是一个Ant风格的路径模式，可以使用`*`、`**`等通配符。例如，`/static/**`可以匹配所有以`/static/`开头的URL路径。
>
> 3. 使用`addResourceLocations`方法指定静态资源文件的存放目录或位置。可以使用`classpath:`前缀指定类路径下的资源，也可以使用绝对路径指定文件系统中的资源。例如，`classpath:/static/`表示静态资源文件存放在类路径下的`/static/`目录。
>
> 4. 可选地，使用其他方法设置一些属性，如设置缓存期限、配置缓存控制策略、开启资源链等。这些属性可以根据你的需求来定制。
>
> 下面是一个示例，展示了如何使用`addResourceHandlers`方法注册和配置静态资源处理器：
>
> ```java
> @Configuration
> public class WebMvcConfig implements WebMvcConfigurer {
> 
>     @Override
>     public void addResourceHandlers(ResourceHandlerRegistry registry) {
>         registry
>             .addResourceHandler("/static/**")
>             .addResourceLocations("classpath:/static/")
>             .setCachePeriod(3600)
>             .resourceChain(true);
>     }
> 
>     // 其他配置方法...
> 
> }
> ```
>
> 在上述示例中，我们注册了一个处理器，用于处理以`/static/`开头的URL路径，并将对应的静态资源文件存放在类路径下的`/static/`目录。设置了缓存期限为3600秒，并开启了资源链的支持。
>
> 通过使用`addResourceHandlers`方法，你可以轻松地注册和配置静态资源处理器，使得Spring MVC框架能够正确处理静态资源请求并返回相应的资源文件。
>
> 希望这样能更清楚地解释了`addResourceHandlers`方法的作用和用法。如有其他问题，请随时提问。

##### @Override    public void addResourceHandlers(ResourceHandlerRegistry registry){        registry.addResourceHandler("/file/**").addResourceLocations("file:" + path);       }；这段代码什么啥意思

> 这段代码是在Spring MVC的配置类中使用`addResourceHandlers`方法注册了一个静态资源处理器。
>
> 具体解释如下：
>
> 1. `@Override`：这是一个注解，表示方法重写了父类或接口的方法。
>
> 2. `public void addResourceHandlers(ResourceHandlerRegistry registry)`：这是`WebMvcConfigurer`接口的方法，用于注册和配置静态资源处理器。
>
> 3. `registry.addResourceHandler("/file/**")`：通过调用`addResourceHandler`方法，指定了静态资源的URL路径模式。在这里，URL路径模式是"/file/**"，表示匹配以"/file/"开头的URL路径。
>
> 4. `.addResourceLocations("file:" + path)`：通过调用`addResourceLocations`方法，指定了静态资源文件的存放位置。这里使用了"file:"前缀，表示资源文件存放在文件系统中。`path`是一个变量，代表了文件系统中的具体路径。这样配置后，当请求匹配到"/file/**"的URL路径时，将会在指定的文件系统路径下查找对应的资源文件。
>
> 综合起来，以上代码的作用是将"/file/**"的URL路径与指定的文件系统路径下的静态资源文件进行映射关联。当请求匹配到"/file/**"的URL路径时，将会在指定的文件系统路径下查找对应的资源文件并返回给客户端。
>
> 请注意，`path`是一个变量，你需要将其替换为实际的文件系统路径，以确保正确的文件路径映射。
>
> 希望这样解释能够帮助到你。如果还有其他问题，请随时提问。

##### addResourceLocations方法参数有哪些

> `addResourceLocations`方法用于指定静态资源文件的存放位置，其参数可以接受多个字符串参数。
>
> 在`ResourceHandlerRegistry`类中，`addResourceLocations`方法的参数类型为`String... resourceLocations`，表示可变长度的字符串数组，即可以传递一个或多个字符串作为参数。
>
> 每个字符串参数都代表一个静态资源文件的存放位置，可以是文件系统路径、类路径、URL等。以下是`addResourceLocations`方法的示例用法：
>
> ```java
> .addResourceLocations("file:/path/to/files", "classpath:/static/", "https://example.com/assets/")
> ```
>
> 在上述示例中，我们通过多个参数传递了三个静态资源文件的存放位置：
>
> 1. `"file:/path/to/files"`：表示静态资源文件存放在文件系统中的`/path/to/files`路径下。
>
> 2. `"classpath:/static/"`：表示静态资源文件存放在类路径中的`/static/`目录下。
>
> 3. `"https://example.com/assets/"`：表示静态资源文件存放在远程服务器上的`https://example.com/assets/`路径下。
>
> 你可以根据实际情况传递一个或多个参数来指定静态资源文件的存放位置。多个参数将被依次处理，即按照参数的顺序进行资源查找。
>
> 希望这个回答对你有帮助。如果还有其他问题，请随时提问。

##### 这个文件系统指什么

> 在计算机领域中，"文件系统"（File System）是指用于组织和管理计算机上文件和目录的一种方式或结构。
>
> 文件系统定义了文件和目录的命名规则、存储方式、访问权限以及文件和目录之间的关系等。它提供了一种逻辑上的抽象，使得我们可以通过统一的接口（例如操作系统提供的文件系统 API）来操作和管理文件和目录，而不需要关心底层存储设备的具体细节。
>
> 在上下文中，当提到"文件系统"时，通常指的是计算机操作系统中的文件系统，用于管理计算机上的文件和目录。具体的文件系统可以有多种实现，例如：
>
> 1. Windows 文件系统（例如 NTFS、FAT32 等）：用于 Windows 操作系统。
> 2. macOS 文件系统（例如 HFS+、APFS 等）：用于 macOS 操作系统。
> 3. Linux 文件系统（例如 ext4、XFS、Btrfs 等）：用于 Linux 操作系统。
> 4. Unix 文件系统（例如 UFS、ZFS 等）：用于 Unix 类操作系统。
>
> 当在代码中使用 "文件系统" 一词时，通常指的是在操作系统中，使用文件路径和文件系统 API（如Java中的`java.io.File`类）来访问和操作文件和目录。这样的文件系统可以是本地文件系统（例如硬盘、固态硬盘等），也可以是网络文件系统（例如 NFS、SMB/CIFS 等）。
>
> 所以，当在代码中使用"文件系统"时，通常是指访问和操作计算机上的文件和目录的能力和机制。
>
> 希望这个解释能够回答你的问题。如果还有其他疑问，请随时提问。