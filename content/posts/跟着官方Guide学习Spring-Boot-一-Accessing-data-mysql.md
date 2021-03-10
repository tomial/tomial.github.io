---
title: '跟着官方Guide学习Spring Boot(一):Accessing-data-mysql'
date: 2019-10-20 20:18:22

tags: 
- Web

categories : 
- Spring
---

官网地址：[Accessing data with MySQL](https://spring.io/guides/gs/accessing-data-mysql/)

本期Guide需要：

- MySQL5.6或以上版本
- 一个文本编辑器/IDE
- JDK1.8或以上版本
- Gradle 4+或 Maven 3.2+

## 使用Spring Initializr开始

在Spring Initializr中选择以下依赖项：

-  Spring Web
-  Spring Data JPA
-  MySQL Driver

![](https://spring.io/guides/gs/accessing-data-mysql/images/initializr.png)

然后在终端里用root账号登录mysql：

`sudo mysql --password` 或 `sudo mysql -u root -p`

> `root`用户能访问数据库里的所有信息，**不推荐**在生产环境中使用使用root来连接到数据库
>
> This connects to MySQL as `root` and allows access to the user from all hosts. This is **not the recommended way** for a production server.

通过以下命令创建所需要的数据库：

```mysql
mysql> create database db_example; -- Creates the new database
mysql> create user 'springuser'@'%' identified by 'ThePassword'; -- Creates the user
mysql> grant all on db_example.* to 'springuser'@'%'; -- Gives all privileges to the new user on the newly created database
```



## 创建 `application.properties`文件

Spring Boot 会给你设置很多默认选项，比如它默认的数据库是H2，所以如果你想要使用其他的数据库，你需要在 `application.properties`里面定义连接属性

定义一个`src/main/resources/application.properties`文件，内容为

```java
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://${MYSQL_HOST:localhost}:3306/db_example
spring.datasource.username=springuser
spring.datasource.password=ThePassword
```

这里的spring.jpa.hibernate.dll-auto可以是`none` , `update`, `create` 或者 `create-drop`.具体请去查看Hibernate的文档

- `none`: `MySQL`的默认项，数据库结构不会被改变

- `update`: Hibernate根据入口的结构改变数据库的结构

- `create`: 每次都创建该数据库但在关闭的时候不删除(drop)它

- `create-drop`: 创建数据库并在`SessionFactory`关闭时删除它 

  一开始一定是使用`create` 或 `update`，因为一开始还没有数据库结构。第一次运行后可以根据需要将这个参数改为`update` 或者 `none`，如果你想要改变数据库结构时请使用`create`.

  对于`H2`和其他嵌入式数据库，默认为`create-drop`，而其他数据库，如`MySQL`，默认则为`none`.

> 一种良好的做法是，当你的数据库进入了生产状态，这个应该被设置为 none，revoke 连接到 Spring应用的MySQL用户的所有权限，然后给予用户 SELECT, UPDATE, INSERT 和 DELETE的权限，更多相关知识请阅读指南的结尾部分
>
> It is good security practice to, after your database is in production state, set this to `none`, revoke all privileges from the MySQL user connected to the Spring application, and give the MySQL user only `SELECT`, `UPDATE`, `INSERT`, and `DELETE`. You can read more about this at the end of this guide.

### 创建`Entity`模型

创建一个`User`类：

```java
package com.example.accessingdatamysql;


import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

// Hibernate automatically translates the entity into a table.

@Entity //这个注解告诉Hibernate用这个类生成一个表
		// This tells Hibernate to make a table out of this class
public class User {
  @Id
  @GeneratedValue(strategy= GenerationType.AUTO)
  private Integer id;
  private String name;
  private String email;

  public Integer getId() {
    return id;
  }

  public void setId(Integer id) {
    this.id = id;
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public String getEmail() {
    return email;
  }

  public void setEmail(String email) {
    this.email = email;
  }
}

```

## 创建存储库

你需要一个存储库来保存用户记录，创建一个`UserRepository`类：

```java
package com.example.accessingdatamysql;

import org.springframework.data.repository.CrudRepository;

import com.example.accessingdatamysql.User;

// 这个类会被Spring自动实现为一个叫userRepository的类
// CURD 指 Create, Read, Update, Delete
// This will be AUTO IMPLEMENTED by Spring into a Bean called userRepository
// CRUD refers Create, Read, Update, Delete

public interface UserRepository extends CrudRepository<User, Integer> {
}
```

Spring在具有相同名称的Bean中自动实现此存储库接口(大小写有所改变，它叫`userRepository`)

### 创建一个控制器

你需要创建一个控制器来处理发送到这个应用的HTTP请求，如下面的`MainControl.java`所示：

```java
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller //这个注解指出这个类是一个控制器
@RequestMapping(path="/demo") //这个注解表示这个类接收/demo开头的请求(/demo在Application的路径后)
public class MainController {
  @Autowired  //这个注解指出这里需要获得一个叫userRepository的bean
              //这个bean是刚才被Spring自动生成的，它被用来处理数据
  private UserRepository userRepository;

  @PostMapping(path="/add") //发送POST请求到路径/demo/add时由这个方法处理
  public @ResponseBody String addNewUser (@RequestParam String name
                  , @RequestParam String email) {
    /**
     * @ResponseBody 意思是这个方法返回的 String 内容就是服务器响应的具体内容，
     *  ,而不是视图名称
     * @RequestParam 意思是将该参数和发送到服务器的GET/POST请求的参数 对应 起来
     */

          User n = new User();
          n.setName(name);
          n.setEmail(email);
          userRepository.save(n);
          return "Saved";
  }

  @GetMapping(path="/all")  //发送GET请求到路径/demo/all时由这个方法处理
  public @ResponseBody Iterable<User> getAllUsers() {
    // 返回包含所有用户的JSON或XML
    return userRepository.findAll();
  }
}
```

>  上述例子特别地将`GET`和`POST`请求映射到了`/demo/add`路径，一般来说，@RequestMapping默认将所有类型请求的路径都映射到其参数
>
> The preceding example explicitly specifies `POST` and `GET` for the two endpoints. By default, `@RequestMapping` maps all HTTP operations.

## 创建一个Application类

```java
package com.example.accessingdatamysql;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class AccessingDataMysqlApplication {

	public static void main(String[] args) {
		SpringApplication.run(AccessingDataMysqlApplication.class, args);
	}

}
```

Spring Initializr自动创建了一个简单的Application类，这个例子中你不需要去修改这个类

`@SpringApplication`这个注解相当于添加了下面三个注解：

- `@Configuration`: 
- `@EnableAutoConfiguration`: 告诉Spring Boot根据添加的依赖项jar来"猜测"你想要怎么配置Spring。比如，如果`spring-webmvc`在依赖项里面，这个注解就会自动将这个应用标记为一个web应用程序并且激活其关键行为，比如设置一个`DispatcherServlet`
- `@ComponentScan`: 告诉Spring在`com/example`包中寻找其他的组件、配置和服务，让它找到控制器。

main方法使用Spring Boot的`SpringApplcation.run()`方法来运行该应用

## 测试

应用运行后，你可以用`curl`或者一些类似的工具来测试它

以下的curl 命令会添加一个用户：

`$ curl localhost:8080/demo/add -d name=First -d email=someemail@someemailprovider.com`

返回的内容应该为：

`Saved`

下面的curl 命令会返回所有用户：

`$ curl 'localhost:8080/demo/all'`

返回的内容为：

`[{"id":1,"name":"First","email":"someemail@someemailprovider.com"}]`

## 安全问题

当你的应用处于生产环境时，你可能会面临**SQL注入**攻击。一个攻击者可能会注入`DROP TABLE`或者任何毁灭性的SQL 命令。所以，一个安全的做法是，在投入生产环境给用户使用前，你应该修改一下你的数据库。

下面的命令撤销所有跟该应用有关的用户的权限：

`mysql> revoke all on db_example.* from 'springuser'@'%';`

现在这个应用无法在数据库中做任何事情了。

但用户必须有一些权限来使用你的应用，下面的命令给予用户一些基本的权限：

`mysql> grant select, insert, delete, update on db_example.* to 'springuser'@'localhost';`

删除所有的权限再给予需要的权限可以是的你的应用具有更改数据库数据而不更改数据库结构的能力。

当你下次想要更改数据库结构的时候：

1. 重新获得权限
2. 更改`spring.jpa.hibernate.ddl-auto`为`update`
3. 重新运行你的应用 

然后重复上面的两处操作，以确保你的应用是安全的。更好的做法是，使用一个专门的迁移工具，比如Flyway或者Liquibase

## 总结

恭喜！你成功开发了你的第一个绑定MySQL数据库的Spring应用程序

## See Also

可以继续学习下列Guide:

- [Accessing Data with JPA](https://spring.io/guides/gs/accessing-data-jpa/)
- [Accessing Data with MongoDB](https://spring.io/guides/gs/accessing-data-mongodb/)
- [Accessing data with Gemfire](https://spring.io/guides/gs/accessing-data-gemfire/)