# 【json&pickle】dumps,loads,dump,load的区别

## 1 json.dumps()

**json.dumps()是将字典类型转化成字符串类型。**

```python
import json
 
name_emb = {'a':'1111','b':'2222','c':'3333','d':'4444'} 
jsObj = json.dumps(name_emb)    
 
print(name_emb)
print(jsObj)
```

## 2 json.dump()

**json.dump()用于将dict类型的数据转成str，并写入到json文件中**

```python
import json  

name_emb = {'a':'1111','b':'2222','c':'3333','d':'4444'}  

emb_filename = ('/home/cqh/faceData/emb_json.json')  

# solution 1
jsObj = json.dumps(name_emb)    
with open(emb_filename, "w") as f:  
    f.write(jsObj)  
    f.close()  

# solution 2   
json.dump(name_emb, open(emb_filename, "w"))
```

## 3 json.loads()

**json.loads()将字符串类型转化成字典类型**

```python
import json

name_emb = {'a':'1111','b':'2222','c':'3333','d':'4444'} 

jsDumps = json.dumps(name_emb)    

jsLoads = json.loads(jsDumps) 

print(name_emb)
print(jsDumps)
print(jsLoads)
```

## 4 json.load()

**json.load()用于从json文件中读取数据。**

```python
# json_load.py 
strList = json.load(with open("listStr.json",mode='r')) 
print strList 
print strDict 
# {u'city': u'\u5317\u4eac', u'name': u'\u5927\u5218'} 
```

- **pickle常用操作参考json**

- 格式要求:

  - 只能包含int/ str/ list/ dict/ bool/float  不存在元组/集合
  - 最外层必须是一个列表或字典
  - 在json中如果有字符串,必须是双引号"json中的字符串"
  - 真假小写true/false

- 字典或者列表中存在中文,序列化的时候或转为UNcode格式,如果想保存中文就需要进行以下操作

  ```python
  v1=[1,2,3,4,'2','大牛']
  val = json.dumps(v1,ensure_ascii=False)
  print(val)     #[1, 2, 3, 4, "2", "大牛"]
  ```

- json:优点:所有语言通用,缺点:只能序列化部分数据类型;不能连续load多次

- pickle;可以序列化多有数据,但是序列化之后只有python识别,能连续load多次

总结:  dump与json转化时可以结合文件操作,dumps与jsons不能.