pip install –index https://pypi.mirrors.ustc.edu.cn/simple/ XXX【库名】
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package

# csv
```python
import csv  

f = open('mm.csv','a',newline='', encoding='utf-8')
cw = csv.writer(f)
cw.writerow(['id','name'])
cw.writerow(['1','小明'])
cw.writerow(['2','小红'])
```

# requests
```python
res = requests.get("https://asd.com/getdata.html")
res = requests.post("https://asd.com/getdata.html",data={'page':pn})

res.status_code
res.text #str
res.content #二进制数据，图片等
json.loads(res.text) #dict
json.dumps(j,ensure_ascii=False) #str,json格式

```

# file
```python

#写文件
fw = open('./img/asd.jpg', 'wb')  
fw.write(res.content)  
fw.flush()  
fw.close()

#追加文本
fw = open('./info/asd.json', 'w',encoding="utf-8")  
fw.write("zxc")  
fw.flush()  
fw.close()

```