# littlebird-mybatis-generator-maven-plugin文档

1. 修改源码, 让逆向生成代码更加优雅
2. 省略多余的注释
3. 自动添加数据库中的注释

## 配置插件

在所在项目的`pom.xml`新增插件

```
<!-- MyBatis generator代码生成插件 配置 -->
    <plugin>
        <groupId>net.arccode</groupId>
        <artifactId>littlebird-mybatis-generator-maven-plugin</artifactId>
        <version>0.0.1</version>
        <configuration>
            <configurationFile>${basedir}/src/test/resources/generator-config.xml</configurationFile>
            <overwrite>true</overwrite>
            <verbose>true</verbose>
        </configuration>
    </plugin>
```

相关配置文件和配置(properties)见项目: littlebird

## 运行插件Goal

mvn littlebird-mybatis-generator:generate -Dmybatis.generator.overwrite=true

## db.table -> domain

注: 所有内容均为自动生成, 无人工修改; 看到中文注释是否觉得很优雅呢.
 
### 数据库`little_bird_master`中的表`user`结构

```
CREATE TABLE `user` (
`id`  bigint(20) NOT NULL AUTO_INCREMENT COMMENT '主键ID' ,
`username`  varchar(40) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '用户名' ,
`password`  varchar(40) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '密码' ,
`age`  int(11) NULL DEFAULT NULL COMMENT '年龄' ,
`sex`  int(11) NULL DEFAULT NULL COMMENT '性别, 1-男, 2-女, 3-未知' ,
`mobile_phone`  varchar(20) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '手机号' ,
`created_at`  timestamp NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建日期' ,
`updated_at`  timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新日期' ,
PRIMARY KEY (`id`)
)
ENGINE=InnoDB
DEFAULT CHARACTER SET=utf8 COLLATE=utf8_general_ci
AUTO_INCREMENT=1
ROW_FORMAT=DYNAMIC
;
```

### 通过该插件生成的领域模型

```
public class User {
    /**
     * 主键ID,所属表字段为user.id
     */
    private Long id;

    /**
     * 用户名,所属表字段为user.username
     */
    private String username;

    /**
     * 密码,所属表字段为user.password
     */
    private String password;

    /**
     * 年龄,所属表字段为user.age
     */
    private Integer age;

    /**
     * 性别, 1-男, 2-女, 3-未知,所属表字段为user.sex
     */
    private Integer sex;

    /**
     * 手机号,所属表字段为user.mobile_phone
     */
    private String mobilePhone;

    /**
     * 创建日期,所属表字段为user.created_at
     */
    private Date createdAt;

    /**
     * 更新日期,所属表字段为user.updated_at
     */
    private Date updatedAt;

	/** 以下为getter, setter方法 **/
	// ...省略
}
```

### 通过该插件生成的DAO

```
public interface UserDao {
    /**
     *  根据主键删除数据库的记录,user
     *
     * @param id
     */
    int deleteByPrimaryKey(Long id);

    /**
     *  动态字段,写入数据库记录,user
     *
     * @param record
     */
    int insertSelective(User record);

    /**
     *  根据指定主键获取一条数据库记录,user
     *
     * @param id
     */
    User selectByPrimaryKey(Long id);

    /**
     *  动态字段,根据主键来更新符合条件的数据库记录,user
     *
     * @param record
     */
    int updateByPrimaryKeySelective(User record);
}
```

### 生成的结构如下

```
 ├─dao
 │  ├─impl
 │  └─mapper
 ├─domain
```


 

