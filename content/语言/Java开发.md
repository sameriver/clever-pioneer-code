
# 基础性开发

## 文件读取

- 非数据库形式的读取 读取resource目录下的配置属性文件

### 获取路径

FileInputStream（文件字节输入流）

1. InputStream （常用子类）
    1. FileInputStream （文件字节输入流）
    2. BufferedInputStream

> 由于resource目录下的文件 在本地开发时可以通过代码获取，但是打包为jar包后 文件没有实际路径 只有相对路径

- getResourceAsStream

```java
InputStream in = this.getClass().getClassLoader().getResourceAsStream("resource/file.txt");
```

- ClassPathResouce

> 由于this指针无法在static中获取，此方法适用于小型的main方法程序

```java
ClassPathResource resource = new ClassPathResource("resource/file.txt");
InputStream in = resource.getInputStream();
```

### 读取加载

```java
Properties properties = new Properties();
FileInputStream input = new FileInputStream("src/main/resources/application.properties");
properties.load(input);
String property = properties.getProperty("post.url");
```

### Properties文件读取属性

> properties简介 采用键对值形式存储 key=value形式 采用#或者!进行注释 如果一个值需要跨多行 采用反斜杠\表示延续

## 请求and响应参数

### 请求参数

- URL路径参数：嵌入URL路径中的动态值 如：`GET /api/users/123`
- 查询参数：附加在URL后的键对值 作为条件 如：`GET /api/users?name=Mike&age=25`
- 请求头参数 描述请求的元数据
    - Content-Type：请求体格式 如：`Content-Type: application/json`
    - Accept：期待响应格式 如：`Accept: application/json`
    - Authorization：认证信息 如：`Authorization: Bearer 123`
    - User-Agent：客户端类型（浏览器/设备类型） 如：`User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36`
- 请求体参数：请求体中发送的数据 表单/文件上传 如 JSON、Form Data、Multipart(文件上传)
- Cookie参数: 客户端自动附加的会话标识 如：`Cookie: session=123`

### 响应参数

- 状态码：表示请求结果的三位数字代码
- 响应头：描述响应的元数据
    - Content-Type：响应体格式 如：`Content-Type: application/json` `Content-Type: text/html` `Content-Type: image/jpeg` `Content-Type: excel/xls` `Content-Type: application/octet-stream`
    - Content-Length：响应体长度 如：`Content-Length: 1024`
    - Set-Cookie：设置Cookie 如：`Set-Cookie: session=123`
    - Location：重定向地址
    - Cache-Control：缓存控制策略 如：`Cache-Control: max-age=3600`
    - Content-Disposition：控制客户端对响应内容的处理方式 inline：表示直接显示 attachment：表示作为附件下载
- 响应体：响应体中返回的数据 如 JSON、HTML、XML、图片、视频、音频、文件等

## Excel导出

- 使用EasyExcel

> 后端需要返回给前端通过response输出流，前端通过流下载文件

```java
response.setContentType("application/octet-stream");
response.setHeader("Content-Disposition", "attachment;filename=" + URLEncoder.encode("文件名.xlsx", "UTF-8"));
```

## 注解开发

### 注解校验格式

- @NotNull->基本类型/@NotEmpty->String/集合/数组
    - NotEmpty：用于校验String 集合 数组等类型 确保不为空
    - NotNull： 用于校验基本类型 确保不为NUll
- @Max/@Min->数值类型 用于校验最大值和最小值 `@Max/Min(value= 目标值，message= 错误提示)`
- @NotBlank->string 用于校验字符串不为空 并且最少包含一个非空格字符
- @Size->String/集合/数组 用于校验值的长度范围 `@Size(min= 最小长度，max= 最大长度，message= 错误提示)`
- @Email->String 用于校验邮箱格式 `@Email(message= 错误提示)`

## 正则表达式

### 匹配规则

|字符|说明|
|---|---|
|`.`|匹配任意字符|
|`*`|匹配零个或多个前面的字符|
|`+`|匹配一个或多个前面的字符|
|`?`|匹配零个或一个前面的字符|
|`{n}`|匹配n个前面的字符|
|`{n,}`|匹配n个或更多前面的字符|
|`{n,m}`|匹配n到m个前面的字符|
|`[abc]`|匹配a、b、c中的任意一个字符|
|`[^abc]`|匹配除了a、b、c之外的任意字符|
|`[a-z]`|匹配a到z中的任意一个字符|
|`[^a-z]`|匹配除了a到z之外的任意字符|
|`\\`|转义字符|
|`()`|匹配括号内的字符|

`^` | 匹配字符串的开始 |`|` | | 匹配或的条件 |`$` | 匹配字符串的结束 |

## Json数据语法

### 语法

1. 对象：`{...}`
2. 数组：`[...]`
3. 字符串：`"`
4. 数字：`123`
5. 布尔值：`true`或`false`
6. 键值对：`key:value`
7. 数组对象：`{...}`
8. 注释：`//`或`/**...*/`

### 示例

```json
{
    //键对值表示数据
    //字符串
    "name": "Mike",
    //数字
    "age": 25,    
    "localTime":"2021-12-12 12:12:12",
    "localDate":"2021-12-12", 
    //对象
    "address": {
        "city": "Beijing",
        "street": "Chaoyang"
    },
    //数组
    "hobbies": [
        "reading",
        "swimming",
        "running"
    ],
    //布尔值
    "married": true,
    //数组对象
    "breakfast":[      
        { "name": "Project X", "status": "completed" },
        { "name": "Project Y", "status": "ongoing" }
    ]
}
```

- [ ] 序列化

## Swagger开发

> 包含如下信息

- 组
- 基本信息
- 接口
- 实体类

### 引入依赖

```xml
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
```

### 配置Swagger

### 标准配置

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket docket() {
        // 创建一个 swagger 的 bean 实例
        return new Docket(DocumentationType.SWAGGER_2)
                // 配置接口信息
                .select() // 设置扫描接口
                // 配置如何扫描接口
                .apis(RequestHandlerSelectors
                        //.any() // 扫描全部的接口，默认
                        //.none() // 全部不扫描
                        .basePackage("com.duojiala.mikeboot.controller") // 扫描指定包下的接口，最为常用
                        //.withClassAnnotation(RestController.class) // 扫描带有指定注解的类下所有接口
                        //.withMethodAnnotation(PostMapping.class) // 扫描带有只当注解的方法接口
                )
                .paths(PathSelectors
                        .any() // 满足条件的路径，该断言总为true
                        //.none() // 不满足条件的路径，该断言总为false（可用于生成环境屏蔽 swagger）
                        //.ant("/user/**") // 满足字符串表达式路径
                        //.regex("") // 符合正则的路径
                )
                .build();
    }
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Spring Boot中使用Swagger2构建RESTful API")
                .description("RESTful API")
                .termsOfServiceUrl("<http://localhost:8080/>")
                .version("1.0")
                .build();
    }
}
```

### 配置分组

```java
    @Bean
    public Docket docket1() {
        // 创建一个 swagger 的 bean 实例
        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("mike") // 修改组名为 "mike"
                ;
    }
    @Bean
    public Docket docket2() {
        // 创建一个 swagger 的 bean 实例
        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("tim") // 修改组名为 "tim"
                ;
    }
```

### 配置环境

```java
 @Bean
    public Docket docket(Environment environment) {

        // 设置环境范围
        Profiles profiles = Profiles.of("dev","test");
        // 如果在该环境返回内则返回：true，反之返回 false
        boolean flag = environment.acceptsProfiles(profiles);

        // 创建一个 swagger 的 bean 实例
        return new Docket(DocumentationType.SWAGGER_2)
                .enable(flag) // 是否开启 swagger：true -> 开启，false -> 关闭
                ;
    }
```

### 编写接口文档

1. 实体类 `@ApiModel(value = "用户实体类", description = "用户实体类")` value 实体类名/不指定则为类名 description 实体类描述 parent 父类
2. 属性模块数据 `@ApiModelProperty(value = "用户id", required = true, example = "1")` value 属性描述 required 是否必填 example 示例值
3. 编写接口类 `@ApiOperation(value = "获取用户列表", notes = "获取用户列表")` value 接口描述 notes 接口详细描述 response 响应实体类 request 请求实体类
4. 方法和参数注解 `@ApiParam(value = "用户id", required = true, example = "1")` value 参数描述 required 是否必填 example 示例值 defaultValue 默认值

## 芋道开发

### 拉取芋道源码

[芋道项目地址](https://github.com/YunaiV/ruoyi-vue-pro)

[swagger文档地址](%5B**%3Chttp://127.0.0.1:48080/doc.html**%3E%5D\(%3Chttp://127.0.0.1:48080/doc.html%3E\))

### 一键改包

1. 打开projectReactor类
2. 填写如下信息： **groupIdNewartifactIdNewtitleNew**：项目名称 **packageNameNew**：包名 **projectBaseDirNew**：新项目路径
3. 执行prejectReactor.main()方法
4. Idea**打开新目录后** **执行SQL导入**

### 新建模块

1. 新建模块→命名为yudao-module-demo（**以yudao为父项目**）
    
    - pom文件添加配置
        
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <project xmlns="<http://maven.apache.org/POM/4.0.0>"
                 xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>"
                 xsi:schemaLocation="<http://maven.apache.org/POM/4.0.0> <http://maven.apache.org/xsd/maven-4.0.0.xsd>">
            <parent>
                <artifactId>yudao</artifactId>
                <groupId>cn.iocoder.boot</groupId>
                <version>${revision}</version> <!-- 1. 修改 version 为 ${revision} -->
            </parent>
            <modelVersion>4.0.0</modelVersion>
        
            <artifactId>yudao-module-demo</artifactId>
            <packaging>pom</packaging> <!-- 2. 新增 packaging 为 pom -->
        
            <name>${project.artifactId}</name> <!-- 3. 新增 name 为 ${project.artifactId} -->
            <description> <!-- 4. 新增 description 为该模块的描述 -->
                demo 模块，主要实现 XXX、YYY、ZZZ 等功能。
            </description>
        
        </project>
        ```
        
    
    删除src目录
    
2. 新建子模块→命名为yudao-module-demo-api（**以yudao-module-demo为父项目**）
    
    - pom文件中添加配置
        
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <project xmlns="<http://maven.apache.org/POM/4.0.0>"
                 xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>"
                 xsi:schemaLocation="<http://maven.apache.org/POM/4.0.0> <http://maven.apache.org/xsd/maven-4.0.0.xsd>">
            <parent>
                <artifactId>yudao-module-demo</artifactId>
                <groupId>cn.iocoder.boot</groupId>
                <version>${revision}</version> <!-- 1. 修改 version 为 ${revision} -->
            </parent>
            <modelVersion>4.0.0</modelVersion>
            <packaging>jar</packaging> <!-- 2. 新增 packaging 为 jar -->
        
            <artifactId>yudao-module-demo-biz</artifactId>
        
            <name>${project.artifactId}</name> <!-- 3. 新增 name 为 ${project.artifactId} -->
            <description> <!-- 4. 新增 description 为该模块的描述 -->
                demo 模块，主要实现 XXX、YYY、ZZZ 等功能。
            </description>
        
            <dependencies>  <!-- 5. 新增依赖，这里引入的都是比较常用的业务组件、技术组件 -->
                <dependency>
                    <groupId>cn.iocoder.boot</groupId>
                    <artifactId>yudao-module-demo-api</artifactId>
                    <version>${revision}</version>
                </dependency>
        
                <!-- Web 相关 -->
                <dependency>
                    <groupId>cn.iocoder.boot</groupId>
                    <artifactId>yudao-spring-boot-starter-web</artifactId>
                </dependency>
        
                <dependency>
                    <groupId>cn.iocoder.boot</groupId>
                    <artifactId>yudao-spring-boot-starter-security</artifactId>
                </dependency>
        
                <!-- DB 相关 -->
                <dependency>
                    <groupId>cn.iocoder.boot</groupId>
                    <artifactId>yudao-spring-boot-starter-mybatis</artifactId>
                </dependency>
        
                <!-- Test 测试相关 -->
                <dependency>
                    <groupId>cn.iocoder.boot</groupId>
                    <artifactId>yudao-spring-boot-starter-test</artifactId>
                </dependency>
            </dependencies>
            
        </project>
        ```
        
    
    _**可选**_：新建 `cn.iocoder.yudao.module.demo` **基础**包，其中 `demo` 为模块名。之后，新建 `api` 和 `enums` 包
    
3. 新建子模块→命名为yudao-module-demo-biz
    
    - pom文件中添加配置
        
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <project xmlns="<http://maven.apache.org/POM/4.0.0>"
                 xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>"
                 xsi:schemaLocation="<http://maven.apache.org/POM/4.0.0> <http://maven.apache.org/xsd/maven-4.0.0.xsd>">
            <parent>
                <artifactId>yudao-module-demo</artifactId>
                <groupId>cn.iocoder.boot</groupId>
                <version>${revision}</version> <!-- 1. 修改 version 为 ${revision} -->
            </parent>
            <modelVersion>4.0.0</modelVersion>
            <packaging>jar</packaging> <!-- 2. 新增 packaging 为 jar -->
        
            <artifactId>yudao-module-demo-biz</artifactId>
        
            <name>${project.artifactId}</name> <!-- 3. 新增 name 为 ${project.artifactId} -->
            <description> <!-- 4. 新增 description 为该模块的描述 -->
                demo 模块，主要实现 XXX、YYY、ZZZ 等功能。
            </description>
        
            <dependencies>  <!-- 5. 新增依赖，这里引入的都是比较常用的业务组件、技术组件 -->
                <dependency>
                    <groupId>cn.iocoder.boot</groupId>
                    <artifactId>yudao-module-demo-api</artifactId>
                    <version>${revision}</version>
                </dependency>
        
                <!-- Web 相关 -->
                <dependency>
                    <groupId>cn.iocoder.boot</groupId>
                    <artifactId>yudao-spring-boot-starter-web</artifactId>
                </dependency>
        
                <dependency>
                    <groupId>cn.iocoder.boot</groupId>
                    <artifactId>yudao-spring-boot-starter-security</artifactId>
                </dependency>
        
                <!-- DB 相关 -->
                <dependency>
                    <groupId>cn.iocoder.boot</groupId>
                    <artifactId>yudao-spring-boot-starter-mybatis</artifactId>
                </dependency>
        
                <!-- Test 测试相关 -->
                <dependency>
                    <groupId>cn.iocoder.boot</groupId>
                    <artifactId>yudao-spring-boot-starter-test</artifactId>
                </dependency>
            </dependencies>
            
        </project>
        ```
        
    
    ***必选：***新建 `cn.iocoder.yudao.module.demo` **基础**包，其中 `demo` 为模块名。之后，新建 `controller.admin` 和 `controller.user` 等包
    
4. 新建RESTful API接口
    
    - Controller接口模板
        
        ```java
        @Tag(name = "管理后台 - Test")
        @RestController
        @RequestMapping("/demo/test")
        @Validated
        public class DemoTestController {
        
            @GetMapping("/get")
            @Operation(summary = "获取 test 信息")
            public CommonResult<String> get() {
                return success("true");
            }
        
        }
        ```
        
    
    admin包下默认添加admin-api路径→/admin-api/demo/test/get
    
    app包下默认添加app-api路径→/app-api/demo/test/get
    
5. 引入demo模块
    
    进入yudao-server模块下pom.xml文件下引入新增模块
    
    ![image.png](attachment:71cd0d56-1a6a-482b-bccc-4f12d2a28b58:image.png)
    
6. 添加swagger分组
    
    需要添加swagger→GroupOpenApiBean
    
    ![image.png](attachment:d22eb7a5-8cda-480a-a46f-ff3057540b9b:image.png)
    

### 代码生成

1. 执行SQL文件
    - SQL文件
        
        ```sql
        CREATE TABLE `system_group` (
            `id` bigint NOT NULL AUTO_INCREMENT COMMENT '编号',
            `name` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '名字',
            `description` varchar(512) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '描述',
            `status` tinyint NOT NULL COMMENT '状态',
            `creator` varchar(64) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT '' COMMENT '创建者',
            `create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
            `updater` varchar(64) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT '' COMMENT '更新者',
            `update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
            `deleted` bit(1) NOT NULL DEFAULT b'0' COMMENT '是否删除',
            `tenant_id` bigint NOT NULL DEFAULT '0' COMMENT '租户编号',
            PRIMARY KEY (`id`) USING BTREE
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci COMMENT ='用户组';
        ```
        

- 注意事项：
    - 表名前缀需要与对应模块保持一致→用户组在 `yudao-module-system` 模块，所以表名的前缀是 `system_`
    - 设置主键**Id为bigint 并设置自增**
    - 正确设置**字段是否非空** 将自动生成非空校验
    - 注释信息→自动生成字段名和提示信息
    - 添加 `creator`、`create_time`、`updater`、`update_time`、`deleted` 是必须设置的系统字段；如果开启多租户的功能，并且该表需要多租户的隔离，则需要添加 `tenant_id` 字段

1. 导入表
    - 基础设施→代码生成→基于DB导入
    - 编辑配置
        - 将 `status` 字段的显示类型为【下拉框】，字典类型为【系统状态】。
        - 将 `description` 字段的【查询】取消。
        - 将 `id`、`name`、`description`、`status` 字段的【示例】填写上
2. 配置
    - 下载压缩包后导入模块
    - 复制**ErrorCodeConstants.java_手动操作**的错误码到**ErrorCodeConstants**
    - 将 `h2.sql` 的 CREATE 语句复制到该模块的 `create_tables.sql` 文件，DELETE 语句复制到该模块的 `clean.sql`

### 返回处理

1. 统一响应
    - 成功：状态码+数据
    - 失败：状态码+错误信息

- CommonResult类

![image.png](attachment:5655477a-9029-4cb1-8909-65100af51ce9:image.png)

```json
{
    code: 0,
    data: {
        id: 1,
        username: "yudaoyuanma"
    }
}

// 失败响应
{
    code: 233666,
    message: "徐妈太丑了"
}
```

1. 异常处理

### 日志记录功能

- 开启日志→application.yaml→logging:level添加对应debug

![image.png](attachment:d5e0f230-481e-4051-923a-bb04e36a0969:image.png)

- 操作日志→记录增删改查数据→[**`system_operate_log`**](https://github.com/YunaiV/ruoyi-vue-pro/blob/master/yudao-module-system/yudao-module-system-biz/src/main/java/cn/iocoder/yudao/module/system/dal/dataobject/logger/OperateLogDO.java)

对应的_Service方法_上添加***@LogRecord注解***

- @LogRecord注解
    
    ## 属性
    
    |success|操作内容|string|
    |---|---|---|
    |bizNo|业务编号|integer|
    |type|操作模块|string|
    |subtype|操作名|string|
    |fail|||
    |operator|||
    |extra|||
    |condition|||
    |successCondition|||
    ||||
    
    ## 语法
    
    ```cpp
    //使用花括号包围+#表明特殊使用
    {{#user}}
    //对象的属性 使用 object.field的方式获取
    {{#user.id}}
    //更新操作->使用_DIFF函数
    {_DIFF{user}}
    //同时对应类的字段上添加@DiffLogField注解
    @DiffLogField(name = "字段描述")
    
    ```
    
- 登录日志→记录登录信息→[**`system_login_log`**](https://github.com/YunaiV/ruoyi-vue-pro/blob/master/yudao-module-system/yudao-module-system-biz/src/main/java/cn/iocoder/yudao/module/system/dal/dataobject/logger/LoginLogDO.java)
    
    通过 [**LoginLogTypeEnum](https://github.com/YunaiV/ruoyi-vue-pro/blob/master/yudao-module-system/yudao-module-system-api/src/main/java/cn/iocoder/yudao/module/system/enums/logger/LoginLogTypeEnum.java)枚举类定义登录类型**
    
- 访问日志→记录查询信息→[**`infra_api_access_log`**](https://github.com/YunaiV/ruoyi-vue-pro/blob/master/yudao-module-infra/yudao-module-infra-biz/src/main/java/cn/iocoder/yudao/module/infra/dal/dataobject/logger/ApiAccessLogDO.java)
    
    - @ApiAccessLog注解
        
        ## 属性
        
        |字段|功能|default|
        |---|---|---|
        |enable|是否记录访问参数|true|
        |requestEnable|是否记录请求参数|true|
        |responseEnable|是否记录响应结果|false|
        |sanitizeKeys|敏感参数数组/string|空|
        |operateMdoule|操作模块|@Tag→name|
        |operateName|操作名|@Operation→summary|
        |operateType|操作类型|空/可用OperateTypeEnum|
        
- 错误日志→记录错误信息→[**`infra_api_error_log`**](https://github.com/YunaiV/ruoyi-vue-pro/blob/master/yudao-module-infra/yudao-module-infra-biz/src/main/java/cn/iocoder/yudao/module/infra/dal/dataobject/logger/ApiErrorLogDO.java)
    
    - @ExceptionHandler（value = Exception.class）→添加在异常拦截器上方

## HuTool工具开发

### 导入依赖

```xml
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.8.11</version> <!-- 请使用最新版本 -->       
        </dependency>
```

### MailUtil

- 配置

> 添加与resource/config目录下->mail.setting 文件

```
# 邮件服务器的SMTP地址，可选，默认为smtp.<发件人邮箱后缀>
host = smtp.qq.com
# 邮件服务器的SMTP端口，可选，默认25
port = 465
# 发件人（必须正确，否则发送失败）
from = 打酱油的奥特曼<1420978691@qq.com>
# 用户名，默认为发件人邮箱前缀
user = 1420978691
# 密码（注意，某些邮箱需要为SMTP服务单独设置授权码，详情查看相关帮助）
pass = qsslnxuleorvfide
sslEnable = true
startttlEnable = true
```

- 启用SSL

```java
MailUtil.setSslEnable(true);
```

- 发送邮件

```java
 MailUtil.send("接收者", "主题", "内容","false" (是否启用html));
```

### CronUtil

- 设置定时任务

```java
CronUtil.schedule("0/10 * * * * ? ", new Task() {            @Override            public void execute() {                // 任务内容            }        });
```

- 设置秒级定时任务

```java
        CronUtil.setMatchSecond(true);
```

- 开启定时任务

```java
        CronUtil.start();
```

- 停止定时任务

```java
        CronUtil.stop();
```

### StrUtil

- 字符串判断与处理
    
- 判断操作
    
    |函数|说明|
    |---|---|
    |isEmpty/isNotEmpty|是否为空/不为空|
    |isBlank/isNotBlank|是否为空/不为空|
    |equals/equalsIgnoreCase|是否相等/相等（忽略大小写）|
    |contains|是否包含子串|
    |startsWith/endWith|判断前缀/后缀|
    |isNumberic|是否为数字|
    |isInteger/isFloat|是否为整数/浮点数|
    |isChinese|是否为中文|
    |isEmail/isUrl/isIP/isMobile|判断邮箱/url/ip地址/联系方式|
    |isIdCard/isBirthday|判断身份证号/生日|
    |||
    
- 处理操作
    
    |函数|说明|
    |---|---|
    |trim|去除两端空格|
    |clean|清楚不可见字符|
    

### CollectionUtil

- 集合判断处理
    
- 判断操作
    
    |函数|说明|
    |---|---|
    |isEmpty/isNotEmpty|是否为空/不为空|
    |isAllMatch/isAllNotMatch|是否所有/没有满足条件|
    |isAnyMatch/isAnyNotMatch|是否有/没有满足条件|
    
- 处理
    
    |||
    |---|---|
    |||
    |||
    

## 定时任务实现

### 基于Timer实现

```java
import java.util.Timer;
import java.util.TimerTask;    

public class TimerTest {
    public static void main(String[] args) {
        Timer timer = new Timer();
        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                // 定时任务执行内容
                System.out.println("定时任务执行");
            }
        }, 0, 1000 * 60 * 10); // 延迟10分钟执行，每隔10分钟执行一次
    }
}
```

### 基于ScheduledExecutorService实现
```java
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

public class ScheduledExecutorServiceExample {
    private static boolean requestReceived = false; // 假设这是接收请求的状态标志

    public static void main(String[] args) {
        ScheduledExecutorService scheduler = Executors.newSingleThreadScheduledExecutor();

        // 安排一个任务，每隔 15 秒检查一次是否有请求到达
        scheduler.scheduleAtFixedRate(() -> {
            if (!requestReceived) {
                System.out.println("No request received, performing other operations...");
                performOtherOperations(); // 调用其他操作方法
            } else {
                System.out.println("Request received, resetting state.");
                requestReceived = false; // 重置状态以便下次检测
            }
        }, 0, 15, TimeUnit.SECONDS);

        // 关闭调度服务的钩子函数
        Runtime.getRuntime().addShutdownHook(new Thread(scheduler::shutdown));
    }

    private static void performOtherOperations() {
        // 这里放置要执行的操作逻辑
        System.out.println("Executing additional tasks...");
    }

    // 模拟外部请求到来的方法
    public static void simulateRequestArrival() {
        requestReceived = true;
    }
}
```