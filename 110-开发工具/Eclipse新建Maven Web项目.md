# Eclipse新建Maven Web项目

1. 新建maven，勾选`Create a simple project(skip archetype selection)`，下一步输入`groupId`、`artifactId`，`Packaging`选择`war`，完成

2. 项目报错，`web.xml is missing and <failOnMissingWebXml> is set to true`，提示缺少web.xml文件，增加插件，然后右键，maven update即可

   ```xml
   <build>
       <plugins>
           <!-- 解决web.xml is missing and <failOnMissingWebXml> is set to true -->
           <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-war-plugin</artifactId>
               <version>3.2.3</version>
               <configuration>
                   <failOnMissingWebXml>false</failOnMissingWebXml>
               </configuration>
           </plugin>
   
       </plugins>
   </build>
   ```

3. 导入servlet api依赖，version 3.0以上，可以没有web.xml文件

   ```xml
   <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>javax.servlet-api</artifactId>
       <version>3.1.0</version>
       <!-- provided 目标项目由这个jar，打包的时候不要放进去 -->
       <scope>provided</scope>
   </dependency>
   ```

   