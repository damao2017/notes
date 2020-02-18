# maven指向nexus

修改maven配置文件settings.xml，eclipse默认不存在，默认位置为C:\Users\hp\.m2\settings.xml，需要拷贝过去。

```xml
<profiles>
		<!-- 配置一个profile -->
		<profile>
			<id>NexusProfile</id>
			
			<!-- 配置一个repository -->
			<repositories>
				<repository>
					<id>nexus</id>
					<name>Nexus Repositories</name>
					<url>http://132.233.33.153:18081/nexus/content/groups/public/</url>
					<snapshots>
						<enabled>true</enabled>
					</snapshots>
					<releases>
						<enabled>true</enabled>
					</releases>
				</repository>
			</repositories>
			
			<!-- 配置一个plugin repository -->
			<pluginRepositories>
				<pluginRepository>
					<id>nexus</id>
					<name>Nexus Repositories</name>
					<url>http://132.233.33.153:18081/nexus/content/groups/public/</url>
					<releases>
						<enabled>true</enabled>
					</releases>
					<snapshots>
						<enabled>true</enabled>
					</snapshots>
				</pluginRepository>
			</pluginRepositories>
		</profile>
</profiles>

<!-- 激活profile -->
<activeProfiles>
	<activeProfile>NexusProfile</activeProfile>
</activeProfiles>

```

