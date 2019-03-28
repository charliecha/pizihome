# 新特性

## java1.7 try-with-resource

```
用法

private static void printFileJava7() throws IOException {
    try(FileInputStream input = new FileInputStream("file.txt")) {
        int data = input.read();
        while(data != -1){
            System.out.print((char) data);
            data = input.read();
        }
    }
}

所有实现了java.lang.AutoCloseable这个接口的类都可以在try-with-resources结构中使用。
```

[参考](http://ifeve.com/java-7%E4%B8%AD%E7%9A%84try-with-resources/)

