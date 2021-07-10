# SQL に関するメモ

基本書式

	SELECT * FROM tableName;

条件付き書式(整数)

	SELECT * FROM TABLENAME WHERE columnName=1;
	SELECT * FROM TABLENAME WHERE columnName='value';
	SELECT * FROM TABLENAME WHERE columnA=1 AND columnB='value';
	SELECT * FROM TABLENAME WHERE columnA=1 OR columnA=2;

LIMIT
	
	-- 取得数の上限を設定する
	SELECT * FROM TABLE_NAME LIMIT 10;
	
	-- 取得を開始する位置を指定する。下記では２行目から取得を開始する。
	SELECT * FROM TABLE_NAME LIMIT 10 OFFSET 2;
	
	-- where と同時使用
	-- where + offset 同時使用の際は、where が優先される。
	SELECT * FROM TABLE_NAME WHERE columnName='name' LIMIT 10 OFFSET 2;
	
	-- where, order by と同時使用
	SELECT * FROM TABLE_NAME WHERE columnName='name' ORDER BY columnName DESC LIMIT 10;