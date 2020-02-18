# extjs grid checked

```javascript
function cu(object, event, data ) {
	var objectType = object.constructor.xtype;
	
	if(objectType=="button"&&data=="CHECKED"){
	    //获取多选
		var records = grid.getSelectionModel().getSelections();
		console.log(records);
		
		//遍历得到id
		var data = '';
		Ext.each(records, function(record) {
				data +='&role_id='+ record.data.id;
		});
		console.log(data);			
	}
}

var sm = new Ext.grid.CheckboxSelectionModel({singleSelect:false});

var grid = new Ext.grid.GridPanel({
				region : 'west',
				width : 400,
				id : 'left',
				split : true,
				store:role_store,  
				collapsible : false,
				tbar : [{
							text : '获取checked',
							handler : cu.createDelegate(this, ['CHECKED'], true)
						}],
				sm: sm,    //必须有
				columns : [
						sm, //必须有
						{header : '角色ID',dataIndex : 'id',sortable : false,menuDisabled:true}, 
						{header : '角色名称',dataIndex : 'name',sortable : false,menuDisabled:true},
						{header : '角色描述',dataIndex : 'describe',sortable : false,menuDisabled:true},
						{header : '创建时间',dataIndex : 'createtime',sortable : false,menuDisabled:true}
						]
			});

```

