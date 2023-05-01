

# 前后端分离 数据交换格式



[前后端数据交互的基本格式](https://blog.csdn.net/qq_34418607/article/details/84972966?ydreferer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS5oay8%3D)



##### 前后端分离开发的JSON数据交换格式

> 前后端分离开发中，JSON（JavaScript Object Notation）是一种常用的数据交换格式，它是一种轻量级的数据交换格式，易于读写，也易于机器解析和生成。在前后端分离的开发中，前端通过HTTP请求后端接口获取JSON格式的数据，然后将这些数据进行处理后，呈现在前端页面上，从而实现了前后端分离的开发模式。通常，在前端和后端之间传递的JSON数据会使用RESTful API进行交互。



##### SpringBoot后端接口返回的JSON数据是写在哪里返回的？

+ 我们写在controller中的返回的数据会被@RestController直接序列化成一个JSON数据，而不用我们显式的去序列化

> Spring Boot 后端接口返回的 JSON 数据一般是写在控制器（Controller）中的某个方法中进行返回的。
>
> 在 Spring Boot 中，可以使用注解 `@RestController` 或 `@Controller` 来标识一个类为控制器，其中 `@RestController` 注解相当于 `@Controller` 和 `@ResponseBody` 的组合，表示该控制器返回的结果是直接序列化为 JSON 数据，而不是返回一个视图。在控制器的方法中，可以使用 `@GetMapping`、`@PostMapping` 等注解来标识该方法支持的 HTTP 请求方式，同时在方法上添加 `@ResponseBody` 注解来表示该方法返回的结果也是直接序列化为 JSON 数据。
>
> 具体来说，在控制器的方法中可以使用如下代码来返回 JSON 数据：
>
> ```java
> @ResponseBody
> @GetMapping("/example")
> public Result example() {
>     // 模拟一个返回结果
>     Map<String, Object> data = new HashMap<>();
>     data.put("id", 1);
>     data.put("name", "example");
>     return Result.success(data);
> }
> ```
>
> 其中，`Result` 是一个封装了统一格式的 JSON 结果的类，`Result.success()` 方法返回一个表示成功的 JSON 结果，`data` 参数是一个封装了实际数据的 Map。在方法中使用 `@ResponseBody` 注解来表示返回的结果是序列化为 JSON 数据。

##### 这个JSON数据最终被封装到哪里

> Spring Boot后端接口返回的JSON数据最终被封装到HTTP响应体中返回给前端。具体地，Spring Boot框架内部使用了Jackson库将Java对象序列化为JSON格式的字符串，然后将这个字符串写入到HTTP响应体中，完成了向前端返回JSON数据的过程。在前端，可以通过AJAX等技术来获取并处理这个JSON数据。

##### Spring Boot框架内部使用了Jackson库将Java对象序列化为JSON格式的字符串，然后将这个字符串写入到HTTP响应体中。这个源代码是什么样子的

> Spring Boot使用了Jackson库的ObjectMapper类将Java对象序列化为JSON格式字符串，然后将字符串写入到HTTP响应体中。具体实现可以参考以下代码：
>
> ```
> @RestController
> @RequestMapping("/api")
> public class ApiController {
> 
>     @GetMapping("/user")
>     public ResponseEntity<User> getUser() {
>         User user = new User("John", "Doe");
>         ObjectMapper objectMapper = new ObjectMapper();
>         String json = "";
>         try {
>             json = objectMapper.writeValueAsString(user);
>         } catch (JsonProcessingException e) {
>             e.printStackTrace();
>         }
>         HttpHeaders headers = new HttpHeaders();
>         headers.setContentType(MediaType.APPLICATION_JSON);
>         return new ResponseEntity<>(json, headers, HttpStatus.OK);
>     }
> }
> ```
>
> 在这个例子中，我们定义了一个返回User对象的接口。我们首先创建一个User对象，然后使用ObjectMapper类将其序列化为JSON格式字符串。接下来，我们创建了一个HttpHeaders对象，设置Content-Type为application/json。最后，我们使用ResponseEntity类将JSON字符串和HttpHeaders对象封装为一个响应实体，然后返回该响应实体。在这个过程中，Spring Boot会自动将响应实体的内容写入到HTTP响应体中。

##### 那么写入HTTP响应体这个过程怎么实现

> 在Spring Boot框架内部，将Java对象序列化为JSON格式的字符串，并将这个字符串写入到HTTP响应体中的过程，是由响应体的输出流实现的。一般来说，这个过程可以通过以下几个步骤实现：
>
> 1. 通过`HttpServletResponse`对象获取响应的输出流，代码如下：
>
>    ```java
>    OutputStream outputStream = response.getOutputStream();
>    ```
>
> 2. 将Java对象序列化为JSON格式的字符串，代码如下：
>
>    ```java
>    ObjectMapper objectMapper = new ObjectMapper();
>    String jsonString = objectMapper.writeValueAsString(object);
>    ```
>
> 3. 将JSON格式的字符串写入到输出流中，代码如下：
>
>    ```java
>    outputStream.write(jsonString.getBytes(Charset.forName("UTF-8")));
>    ```
>
> 4. 关闭输出流，代码如下：
>
>    ```java
>    outputStream.flush();
>    outputStream.close();
>    ```
>
> 在Spring Boot框架中，这个过程通常会由框架内部的JSON序列化组件和响应输出组件自动完成，开发者只需要在Controller中返回一个Java对象即可，框架会自动将其转化为JSON字符串并写入到HTTP响应体中。例如，下面是一个使用Spring Boot框架返回JSON响应的示例：
>
> ```java
> @GetMapping("/user")
> @ResponseBody
> public User getUser() {
>     User user = new User();
>     user.setId(1L);
>     user.setName("Tom");
>     user.setAge(20);
>     return user;
> }
> ```
>
> 在这个示例中，返回的`User`对象会被自动转化为JSON格式的字符串，并通过HTTP响应体返回给客户端。





##### 目前理解：

> 其实前后端分离项目的交互方式也是通过http协议的，只是在http协议的body部分约定了其中的数据格式为application/json因为json格式轻量级，解析方便，即使我们在controller层返回的可能只是一个java实体或者字符串，但是 spring boot 底层可以自动进行解析，把实体解析为json对象放回到response对象的body中（通过获取到ServletRequest对象的输出流进行写入JSON）。所以在一些处理中我们可以直接向response对象输出流中中写入数据进行返回