## 列表相关
```java
List<Integer> list = Arrays.asList(1,2,3);
List<Integer> list = Collections.singletonList(123);
//拼接list
List<Integer> list1 = Arrays.asList(1,2,3);  
List<Integer> list2 = Arrays.asList(4,5,6,6,7,2);  
List<Integer> list3 = Arrays.asList(7,8,9);  
List<Integer> list = Stream.of(list1,list2,list3).flatMap(Collection::stream).collect(Collectors.toList());
//list去重
List<Integer> list= new ArrayList<>(new  HashSet<>(list2)); //无序效率最高
List<Integer> list= new ArrayList<>(new  TreeSet<>(list2)); //有序效率最高
List<Integer> list= list2.stream().distinct().collect(Collectors.toList());//java8stream特性,有序，效率一般
//Java中遍历Map的两种方法：keySet和entrySet
Set<String> set = map.keySet();   
for (String s:set) {  
	System.out.println(s+","+map.get(s));  
}
Set<Map.Entry<String, String>> entryseSet=map.entrySet();  
for (Map.Entry<String, String> entry:entryseSet) {  
	System.out.println(entry.getKey()+","+entry.getValue());  
}
```

## 线程
```java
//实例化线程
    public static void main(String[] args) {
        Thread tr1=new TestThread(); //实例化线程

        tr1.start(); //启动线程
        Thread tr2=new Thread(new TestRunable()); //实例化线程并传入线程体

        tr2.start(); //启动线程
        Thread tr3=new Thread(new TestThreadpool()); //实例化线程并传入线程体
        ExecutorService tp=Executors.newFixedThreadPool(10); //创建具有10个线程的线程池
        tp.execute(tr3);//启动线程
    }
    //使用Thread创建线并启动线程
    class TestThread extends Thread{
        public void run(){
            for(int i=0;i<10;i++){
                System.out.println("Thread>"+Thread.currentThread().getName()+":"+i);
            }
        }

    }
    //使用Runnable创建并启动线程
    class TestRunable implements Runnable{
        public void run(){
            for(int j=0;j<10;j++){
                System.out.println("Runnable>"+Thread.currentThread().getName()+":"+j);
            }
        }

    }
    //使用ExecutorService实现线程池
    class TestThreadpool implements Runnable{
        public void run(){
            for(int k=0;k<10;k++){
                System.out.println("ThreadPool>"+Thread.currentThread().getName()+":"+k);
            }
        }

    }
```

```java
    //匿名内部类方式实例化线程
    public static void main(String[] args) throws IOException {


       //使用Thread创建线并启动线程
        new Thread(){
        public void run(){
            while(true){
                System.out.println("1");
            }
        }
        }.start();

        //使用Runnable创建并启动线程
        new Thread(new Runnable(){
            public void run(){
                while(true){
                    System.out.println("2");
                }
            }
        }).start();

        //使用ExecutorService实现线程池
        Executors.newFixedThreadPool(10).execute(new Runnable() {
            @Override
            public void run() {
                while(true){
                    System.out.println("3");
                }
            }
        });
    }
```

## 类型转换

```java
String > char > int
int num = Character.getNumericValue(str.charAt(i));

int > char > String
String str = Character.toString((char)(ch + '0'))
```

## 