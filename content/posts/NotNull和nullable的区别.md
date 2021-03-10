---
title: '@NotNull和nullable的区别'
date: 2020-06-07 17:02:34

tags:
- JPA

categories : 
- 数据库相关
---

## @NotNull
@NotNull是Bean Validation规范里面定义的一个注解，不仅可以用在实体(Entity)类中，还可以用在别的bean验证Null值。

有一个Item实体类：
```java
@Entity

public class Item {

  @Id
  @GeneratedValue
  private Long id;

  private BigDecimal price;

}
```
 假如在price上添加一个@NotNull注解，并且尝试往服务器里存储一个null值：
```java
@Entity

public class Item {

  @Id
  @GeneratedValue
  private Long id;

  @NotNull
  private BigDecimal price;

}
```

```java
@SpringBootTest
public class ItemIntegrationTest {
  @Autowired
  private ItemRepository itemRepository;

  @Test
  public void shouldNotAllowToPersistNullItemsPrice() {

    itemRepository.save(new Item());
  }
}
```

会看到输出：
```
ConstraintViolationImpl{interpolatedMessage='must not be null', propertyPath=price, rootBeanClass=com.baeldung.h2db.springboot.models.Item,
messageTemplate= {javax.validation.constraints.NotNull.message}'}]]
```

系统会抛出`javax.validation.ConstraintViolationException`异常，而很重要的一点是：**这时候Hibernate还没有尝试往数据库里面发送存储数据的SQL语句**。

在application.properties里面设置`spring.jpa.show-sql=true`后，可以看到控制台输出hibernate执行的语句：
```sql
create table item (

  id bigint not null,
  price decimal (19,2) not null,
  primary key (id)

)
```
会发现@NotNull注解的信息也被Hibernate读取并应用在生成数据库的语句里了，这是个很方便的特性。如果想要禁用这个特性，可以设置
`spring.jpa.properties.hibernate.validator.apply_to_ddl=false`（这个属性属于spring.jpa.properties.*，IDE没有自动补全，属于hibernate原生的属性）,以下是一些spring文档的说明：
>You need to ensure that names defined under `spring.jpa.properties.*` exactly match those expected by your JPA provider. **Spring Boot will not attempt any kind of relaxed binding for these entries.**
>For example, if you want to configure Hibernate’s batch size you must use `spring.jpa.properties.hibernate.jdbc.batch_size`. If you use other forms, such as `batchSize` or `batch-size`, Hibernate will not apply the setting.
>Additional native properties to set on the JPA provider.

## @Column(nullable = false)

这个注解由JPA定义，这意味着Hibernate也会读取这个注解并应用到生成的数据库模式中。

在price加上这个注解：
```java
@Entity

public class Item {

  @Id
  @GeneratedValue
  private Long id;

  @Column(nullable = true)
  private BigDecimal price;

}
```

如果尝试相同的存储操作会获得以下错误：
```
2019-11-14 13:23:03.000 WARN 14580 --- [main] o.h.engine.jdbc.spi.SqlExceptionHelper : SQL Error: 23502, SQLState: 23502

2019-11-14 13:23:03.000 ERROR 14580 --- [main] o.h.engine.jdbc.spi.SqlExceptionHelper : NULL not allowed for column "PRICE"
```

在生成的SQL语句可以看到，列值是有not null约束的：
```sql
Hibernate:

create table item (

  id bigint not null,
  price decimal(19,2) not null,
  primary key (id)

)
```
在上面的错误提示中可以看到这是数据库发生的错误，**说明Hibernate已经往数据库发送了插入语句并且执行了，这个null值并没有在应用层面被发现并抛出异常。**也就是说`@Column(nullable = true)`这个注解**默认**仅仅是用于**约束数据库的列值，而并不会验证插入的数据是否满足约束的条件**。

当然，Hibernate是像@NotNull那样将这个约束作为验证条件的，只需要添加这个设置项：
`spring.jpa.properties.hibernate.check_nullability=true`
，这样Hibernate就会验证插入的数据为null值并抛出异常了：

```
org.springframework.dao.DataIntegrityViolationException:
not-null property references a null or transient value : com.baeldung.h2db.springboot.models.Item.price;

nested exception is org.hibernate.PropertyValueException:
not-null property references a null or transient value : com.baeldung.h2db.springboot.models.Item.price
```

注意这里Hibernate并没有往数据库发送执行语句，数据库就不会产生错误。

## 结论
上面的例子可以看到，@NotNull不仅可以在应用层面就对约束的值做检查，而且Hibernate可以读取这个注解的信息并将其作为自动生成的数据库表的约束。

让数据库处理null值并不是个好的做法，应该在应用层面就将null值过滤掉，而@Column(nullable = true)默认并没有这个功能，它仅仅是用来约束表的模式。

所以应该尽量使用@NotNull这个注解来处理null值。