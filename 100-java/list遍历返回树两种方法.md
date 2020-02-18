### list遍历返回树两种方法

#### 方法1 ： 完整遍历两次list 

```java
for (Resource temp1 : inList) {
	if (0 == temp1.getPid()) {
		trees.add(temp1);
	}
	for (Resource temp2 : inList) {
			if (temp1.getId()==temp2.getPid()) {
				if (temp1.getChildren() == null) {
					temp1.setChildren(new ArrayList<Resource>());
				}
				temp1.getChildren().add(temp2);
			}
	}
	if (temp1.getChildren() == null)
		temp1.setLeaf(true);
}
```

#### 方法2： 一个list遍历，另一个遇到就结束 

```java
for(Resource p1 : inList) {
	if(p1.getPid()==0) {
		trees.add(p1);
	}else {
		for(Resource p2 : inList) {
			if(p2.getId()==p1.getPid()) {
				if (p2.getChildren() == null) {
					p2.setChildren(new ArrayList<Resource>());
				}
				p1.setLeaf(true);
				p2.getChildren().add(p1);
				break;
			}
		}
	}
}
```

