# eclipse maven项目library中没有Maven Dependencies

先关闭项目

找到项目目录下的`.classpath`文件

```xml
<classpathentry kind="con" path="org.eclipse.jst.j2ee.internal.web.container"/>
<classpathentry kind="con" path="org.eclipse.jst.j2ee.internal.module.container"/>
<classpathentry kind="output" path="bin"/>
<!--增加这一句 -->
<classpathentry kind="con" path="org.eclipse.m2e.MAVEN2_CLASSPATH_CONTAINER"/>

```

 找到项目目录下的`.project`文件 

```xml
<natures>
		<nature>org.eclipse.jem.workbench.JavaEMFNature</nature>
		<nature>org.eclipse.wst.common.modulecore.ModuleCoreNature</nature>
		<nature>org.eclipse.m2e.core.maven2Nature</nature>
		<nature>org.eclipse.wst.common.project.facet.core.nature</nature>
		<nature>org.eclipse.jdt.core.javanature</nature>
		<nature>org.eclipse.wst.jsdt.core.jsNature</nature>
<!--增加这一句 -->
		<nature>org.maven.ide.eclipse.maven2Nature</nature>
</natures>
```

 重新打开项目就行了 