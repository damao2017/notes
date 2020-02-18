# extjs formpanel 一些操作

```javascript
var form =   new Ext.form.FormPanel({});

//手工输入的，用这个可以清空
form.getForm().reset(); 

//如果加载的数据，用这个清空
form.getForm().getEl().dom.reset();

//设定某个field的值
form.getForm().findField("id").setValue(0);

//得到某个dield的值
form.form.findField("queryNo").getValue()
```

