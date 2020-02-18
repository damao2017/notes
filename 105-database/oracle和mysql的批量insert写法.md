### oracle和mysql的批量insert写法

#### mysql写法

```sql
insert into zywb_base_user_role(id,user_id,role_id)
 values(1,#{user_id},#{role_id}),
 (2,#{user_id},#{role_id}),
 (3,#{user_id},#{role_id}),
 (4,#{user_id},#{role_id})
 
##mybatis
insert into zywb_base_user_role(id,user_id,role_id)
values
<foreach collection="role_ids" item="role_id"  separator=",">
(zywb_sequence.nextval,#{user_id},#{role_id})
</foreach>
```

#### oracle写法

```sql
insert all 
into zywb_base_user_role(id,user_id,role_id) values (zywb_sequence.nextval,#{user_id},#{role_id})
into zywb_base_user_role(id,user_id,role_id) values (zywb_sequence.nextval,#{user_id},#{role_id})
into zywb_base_user_role(id,user_id,role_id) values (zywb_sequence.nextval,#{user_id},#{role_id})
select 1 from dual

##mybatis
insert all 
<foreach collection="resource_ids" item="resource_id"  >
into zywb_base_role_resource(id,role_id,resource_id) values (zywb_sequence.nextval,#{role_id},#{resource_id})
</foreach>
select 1 from dual
```

