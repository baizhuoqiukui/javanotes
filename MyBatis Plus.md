# MyBatis Plus

## 代码生成

导入依赖

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.2.0</version>
</dependency>
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.3</version>
</dependency>
```

```java
public static void main(String[] args) {

    // 代码生成器
    AutoGenerator mpg = new AutoGenerator();


    GlobalConfig globalConfig = new GlobalConfig();
    globalConfig.setOutputDir(System.getProperty("user.dir") + "/src/main/java");
    globalConfig.setAuthor("nyq");
    globalConfig.setOpen(false);


    // 数据源配置
    DataSourceConfig dataSourceConfig = new DataSourceConfig();
    dataSourceConfig.setUrl("jdbc:mysql://localhost:3306/blogs?useSSL=false&characterEncoding=utf8");
    dataSourceConfig.setDriverName("com.mysql.jdbc.Driver");
    dataSourceConfig.setUsername("root");
    dataSourceConfig.setPassword("root");

    // 包配置
    PackageConfig pc = new PackageConfig();
    pc.setModuleName("user");
    pc.setParent("com.baizhuoqiukui.blogs");
    mpg.setPackageInfo(pc);

    // 策略配置
    StrategyConfig strategy = new StrategyConfig();
    // 下划线转驼峰
    strategy.setNaming(NamingStrategy.underline_to_camel);
    strategy.setColumnNaming(NamingStrategy.underline_to_camel);
    //strategy.setSuperEntityClass("你自己的父类实体,没有就不用设置!");
    strategy.setEntityLombokModel(true);
    strategy.setRestControllerStyle(true);
    // 公共父类
    //strategy.setSuperControllerClass("你自己的父类控制器,没有就不用设置!");
    // 写于父类中的公共字段
    strategy.setSuperEntityColumns("id");
    strategy.setInclude("blogs_auth_user");
    strategy.setControllerMappingHyphenStyle(true);
    //去除表前缀
    strategy.setTablePrefix("blogs_");

    mpg.setStrategy(strategy);
    mpg.setDataSource(dataSourceConfig);
    mpg.setGlobalConfig(globalConfig);
    mpg.execute();


}
```