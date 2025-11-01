## 指针和引用
动画教程： https://www.bilibili.com/video/BV1Gi4y1w7yG
```c
//要点：表示：*x=x的值，&x=x的地址。
//定义和表示有区分，int *x =&y，意为定义指针x指向y的地址，这时x==&y,*x==y,&x==新地址
//int* x =&y和int *x =&y意义相同，建议前一种写法，不然可能会陷入*x==&y的误区，int x =&y是错误的，可以说地址是特殊的类型int*，不是int
//int& x=y，意为定义x为y的引用，这时x==y，&x==&y，当x变时，y会一起变

//定义普通变量
int x = 1;
//定义指针x指向y地址
int* x = &y; //错误写法：int* x = y;
auto x = &y;
auto *x = &y;
//定义x引用y
int& x = y;

//数组中的指针
//string中的char指针
string x = "asd";
//定义临时a指针指向x的第一个字符a，C语言中的字符串表示
char* a = x.c_str(); 
char* a = x.data(); //与c_str相等
//指针移动，当a+1时，则指向下一位s
char* b = a+1; 
//运算，b-a=1，会自动转为long值计算
```
字符串即一个字符数组，各字符的内存地址相连，初始化字符串时会根据字符串长度分配一段内存地址，string a = "123";当a+="s"时，如果a所分配的内存不足以容纳更长的字符串，`std::string`会重新分配足够的内存空间
## 数据处理函数
```c
//比较俩字符串头number个字符是否相等，相等则s=0，n可以大于前面字符串长度
auto s = strncasecmp("Hello", "Hello", n);
//字符串初始化，reserve与resize
std::string sTmp;
sTmp.reserve(512);// 预留512个字符的空间，但sTmp仍然是空字符串
sTmp.resize(512);// 将sTmp的大小设置为512个字符，并用空格填充
//strlen，返回char*到字符串结尾的长度
string str = "Hello, world!";  
char* ch = str.c_str(); // 
char* ch = "Hello, world!";  
long length = strlen(ch);//等同于str.length(),str.size()
ch++;
long length = strlen(ch);
std::string a(ch);//将ch指针对应的字符串定义为a,string类型
//empty,字符串是否为空
str.empty();!str.empty();
//字符串截断
string zz = "123456789";
zz = zz.substr(1); //23456789
zz = zz.substr(0, 6); //234567
zz = zz.substr(0, 9); //234567
zz[2] = '0'; //230567
zz[3] = 0; //230\000\066\067 对指针来说，字符串被截断了，0为结束标记位
//字符串赋值
std::string sBuf;  
sBuf.assign(cStr);// 赋值cStr
sBuf.assign(cStr, 5);// 赋值cStr的前5个字符  
//将内存块中的每个字节设置为指定的值，s指针 ch设置的字符 n设置的数量
void *memset(void *s, int ch, size_t n);

```

