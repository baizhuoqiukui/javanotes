# MongoDB

## 使用

	1.	在C盘创建data文件夹
 	2.	在data中创建db文件夹
 	3.	打开mongod服务器（不能关）
 	4.	打开mongo客户端

## 文件结构

​		数据库 集合（表） 文档（行）

​		--在MongoDB中，数据库和集合都不需要手动创建

​		--当我们创建文档时，如果文档所在的集合或数据库不存在会自动创建数据库和集合

## 基本指令

​		show dbs;

​		show databases;

​				--显示当前所有的数据库

​		use  数据库名

​				--进入到指定的数据库

​		db

​				--db表示的是当前所处的数据库

​		show collections

​				--显示数据库中所有的集合

## 数据库的CRUD

### 		向数据库中插入文档

​				db.collection.insertOne()

​					-插入一个文档对象

​				db.collection.insertMany()

​					-插入多个文档对象

​				db.collection.insert(doc)

​						例子：向test数据库中的stus集合插入一个新的学生对象

​									{name:"zhangsan",age:"34",gender:"男"}

​									db.stus.insert(									{name:"zhangsan",age:"34",gender:"男"}

)

### 		插入多个

​				db.collection.insert([

​									{name:"zhangsan",age:"34",gender:"男"}，

​									{name:"fdaf",age:"2",gender:"男"}，

​									{name:"fsde",age:"43",gender:"男"}

])；

​		--当我们向集合中插入文档时，如果没有给文档指定 _id属性，则数据库会自动为文档添加 _ id，该属性作为文档的唯一标识





### 		查询当前集合中的所有文档

​				db.collection.find()

​				-	find()用来查询集合中**所有**符合条件的文档	

​				-    find()可以用来接受一个对象作为条件参数

​					{}表示查询集合中的所有文档

​					{字段名：值} 查询字段是指定值的文档

​				db.stus.find({ _id:"hello"});

​				db.stus.find({age:16,name:"白骨精"});

​				db.stus.find({ })[1]

​				--中括号中是索引的值	 

​				db.stus.find({ }).count()

​				-- 查询所有结果的数量

​	db.collection.findOne()

​				-- 查询集合中符合条件的第**一个**文档

### 修改

​				db.collection.update(查询条件,新对象)

​				-- 默认情况会使用新对象替换旧对象，其他属性没了

​				--如果只是需要修改指定的属性，而不是替换需要使用”修改操作符“

​					$set可以用来修改文档中的指定属性

​					$unset可以用来删除文档的指定属性

​				--update默认情况下只改一个，都改一个updateMany或者

​				update第三个属性{multi:true}

​				db.stus.update({name:"沙和尚",{age:"28"}})				

​				db.stus.update({

"_id":"fdsafa"

},

{

​	$set:{

​		name:"沙和尚"，gender:"男"

}

}

)

db.stus.update({

"_id":"fdsafa"

},

{

​	$unset:{

​		name: （随便写）

}

}

)

​			db.collection.updateMany()

​			-- 同时修改多个符合条件的文档

​			db.collection.updateOne()

​			--修改一个符合条件的文档

​			db.collection.replaceOne()

​			--替换一个文档

### 删除

​			db.collection.remove()

​			--删除符合条件的所有文档

​			--如果第二个参数传递一个true，则只会删除一个

​			--如果只传递一个空对象作为参数，则会删除整个集合（性能较差），用db.stus.drop();,删库db.dropDatabase();

​			

​			--根据条件删除文档，传递的方式和find()一样

​			db.collection.deleteOne()

​			db.collection.deleteMany()



​			

