springboot maven 项目集成liquibase

官网文档： https://docs.liquibase.com/tools-integrations/springboot/using-springboot-with-maven.html

1、导入项目依赖
<dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
         <groupId>org.liquibase</groupId>
         <artifactId>liquibase-core</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.16</version>
</dependency>

这三个是主要依赖

2、配置liquibase
①通过maven插件配置
 <plugin>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-maven-plugin</artifactId>
    <configuration>
        <propertyFile>liquibase.properties</propertyFile>
        <changeLogFile>${project.basedir}/src/main/resources/config/liquibase/master.xml</changeLogFile>
        <!--          指定和谁进行比较          -->
        <referenceUrl>
            hibernate:spring:org.qingshan.domain?dialect=org.hibernate.dialect.MySQL8Dialect&amp;hibernate.physical_naming_strategy=org.springframework.boot.orm.jpa.hibernate.SpringPhysicalNamingStrategy&amp;hibernate.implicit_naming_strategy=org.springframework.boot.orm.jpa.hibernate.SpringImplicitNamingStrategy
        </referenceUrl>
        <!--设置输出目录-->
<!--                    <changeLogFile>${project.basedir}/src/main/resources/config/liquibase/master.xml</changeLogFile> 这个设置了没啥用-->
        <diffChangeLogFile>
            ${project.basedir}/src/main/resources/config/liquibase/changelog/${maven.build.timestamp}_changelog.xml
        </diffChangeLogFile>
        <outputChangeLogFile>
            ${project.basedir}/src/main/resources/config/liquibase/changelog/${maven.build.timestamp}_changelog.xml
        </outputChangeLogFile>
    </configuration>
    <dependencies>
        <!--                不添加这个依赖：Driver class was not specified and could not be determined from the url （应该是数据库连接依赖）   -->
        <dependency>
            <groupId>org.liquibase.ext</groupId>
            <artifactId>liquibase-hibernate5</artifactId>
            <version>4.3.5</version>
        </dependency>
        <!--Cannot invoke "java.sql.ResultSet.next()" because "catalogs" is null(如果不加这个依赖)                -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
            <version>2.6.2</version>
        </dependency>
    </dependencies>
</plugin>


②在项目根目录下新建文件 liquibase.properties
#要连接库配置信息
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/liquibase?characterEncoding=utf-8&useSSL=false&allowPublicKeyRetrieval=true&useLegacyDatetimeCode=false
username=root
password=softgrid

测试
3、添加user实体类，并标注是一个数据库表，@entity、@table和id注解一定要加

4、运行命令：
mvn clean compile liquibase:diff
将会生成一个changlog

5、创建master.xml文件
将第四步生成的changlog目录放入master.xml

6、配置liquibase 设置加载changlog 目录

import javax.sql.DataSource;

@Configuration
public class LiquibaseConfig {

    @Bean
    public SpringLiquibase liquibase(DataSource dataSource) {
        SpringLiquibase springLiquibase = new SpringLiquibase();
        springLiquibase.setChangeLog("classpath:/config/liquibase/master.xml");
        springLiquibase.setDataSource(dataSource);
        return springLiquibase;
    }
}

6、启动项目。数据库将会更新

注：项目结构



