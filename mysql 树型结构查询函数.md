
# 设置能够添加函数

```set global log_bin_trust_function_creators = 1;```

# 如果已经存在FUNCTION，就先删除了

```DROP FUNCTION `getChildLst`;```

# 遍历子节点

```CREATE FUNCTION `getChildLst`(`rootId` int) RETURNS varchar(1000)
BEGIN
	DECLARE sTemp VARCHAR (1000);
	DECLARE sTempChd VARCHAR (1000);
	SET sTemp = '$';
	SET sTempChd = cast(rootId AS CHAR);
	WHILE sTempChd IS NOT NULL DO
		SET sTemp = concat(sTemp, ',', sTempChd);
		SELECT group_concat(id) INTO sTempChd FROM event_category WHERE FIND_IN_SET(parent_id, sTempChd) > 0;
	END WHILE;
	RETURN sTemp;
END;```

# 如果已经存在FUNCTION，就先删除了

```drop FUNCTION `getParentList`;```

# 遍历父节点

```CREATE FUNCTION `getParentList`(rootId varchar(100), rootName varchar(100)) 
RETURNS varchar(1000) 
BEGIN 
	DECLARE fid varchar(100) default '';
	DECLARE sTempName varchar(100) default '';
	DECLARE sTemp varchar(1000) default rootName;
	WHILE rootId is not null  do
		SET fid =(SELECT parent_id FROM event_category WHERE id = rootId); 
		IF fid is not null THEN 
			SET sTempName = (SELECT `name` FROM event_category WHERE id = rootId);
			SET sTemp = concat(sTempName, '__', sTemp);
			SET rootId = fid; 
		ELSE 
			SET rootId = fid;
		END IF; 
	END WHILE; 
	RETURN sTemp;
END```

# 查询
```select * from event_category where FIND_IN_SET(id, getChildLst(3));```
