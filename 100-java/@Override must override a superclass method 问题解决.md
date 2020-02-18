### @Override must override a superclass method 问题解决

 如果在使用Eclipse开发Java项目时，在使用 @Override 出现以下错误：
The method *** of type *** must override a superclass method

主要是因为你的Compiler是jdk5,(5不支持@Override等形式的批注）只要把它改为8就可以了。
方法：将window->preferences->java-compiler中的Compiler compliance level修改为8。 