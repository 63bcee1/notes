## MYSQL常用语句

### MYSQL表操作

#### MYSQL查看表结构

```sql
desc prepush;
```

#### MYSQL清空表

```sql
truncate table prepush;
```

```sql
delete from prepush;
```

两者都是清空表，truncate比较块，delete比较慢。

### MYSQL字段操作

#### MYSQL添加字段

```sql
ALTER TABLE prepush add pubtime varchar(255) not null;
```

#### MYSQL修改字段

```sql
ALTER TABLE prepush change pubtime varchar(255) not null;
```

#### MYSQL删除字段

```sql
ALTER TABLE prepush DROP source;
```

#### 替换字段内容

```sql
UPDATE project SET pubtime = REPLACE(pubtime,'/','-') WHERE source = '中国联通电子招标投标交易平台';
```

### MYSQL数据库操作

