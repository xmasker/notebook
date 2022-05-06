# Activity 基本用法

## 在 Activity 使用 Toast

- Kotlin

```kotlin
override fun onCreate (savedInstanceState: Bundle?) {
    supen.onCreate (savedInstanceState)
    setContentView(R. layout.first_layout)
    // findViewById()返回的是一个继承自View的泛型对象，Kotlin无法自动推导出是什么控件，要将button1变量显示声明成Button类型
    val buttonl: Button findViewById (R.id.buttonl)
    buttonl.setOnClickListener {
    Toast.makeText(this, "You clicked Button",Toast LENGTH SHORT), show ())
    }
}                
```

使用Kotlin编写的Android项目在app/build.gradle文件的头部默认引入了一个kotlin-android-extensions插件，这个插件会根据布局文件中定义的控件id自动生成一个具有相同名称的变量，我们可以在Activity里直接使用这个变量，而不用再调用findViewById()方法了

- Java

[View点击事件：三种方式实现OnClickListener接口_rongwenbin的博客-CSDN博客_onclicklistener接口](https://blog.csdn.net/rongwenbin/article/details/90767720)

```java
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.first_layout);
        Button button1 = findViewById(R.id.button1);
        button1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(FirstActivity.this, "button1 Clicked.", Toast.LENGTH_SHORT).show();
            }
        });
    }
```



## 在 Activity 使用 Menu

- Kotlin

```kotlin
override fun onCreateOptionsMenu(menu: Menu?): Boolean {
    // 调用了getMenuInfater()得到MenuInflater对象，再调用inflate()给Activity创建菜单
    menuInflater.inflate(R.menu.main, menu)     
	return true 
}
override fun onOptionsItemSelected(item: MenuItem): Boolean {     
    when (item.itemId) {         
        R.id.add_item -> Toast.makeText(this, "You clicked Add",                             Toast.LENGTH_SHORT).show()         
        R.id.remove_item -> Toast.makeText(this, "You clicked Remove",                                Toast.LENGTH_SHORT).show()     
    }     
    return true
}
```

- Java

[Android 的 onCreateOptionsMenu() 创建菜单 Menu 详解 | 孟坤博客 (mkblog.cn)](https://mkblog.cn/881/)

```java
public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.main,menu);
        return true;
}
// 菜单监听
public boolean onOptionsItemSelected(@NonNull MenuItem item) {
	switch (item.getItemId()){
		case R.id.add_item:
             Toast.makeText(this,"add_item",Toast.LENGTH_SHORT).show();
             break;
         case R.id.remove_item:
			Toast.makeText(this,"remove_item",Toast.LENGTH_SHORT).show();
             break;
    }
    return true;
}
```

# 参考链接

- [View点击事件：三种方式实现OnClickListener接口_rongwenbin的博客-CSDN博客_onclicklistener接口](https://blog.csdn.net/rongwenbin/article/details/90767720)
- [Android 的 onCreateOptionsMenu() 创建菜单 Menu 详解 | 孟坤博客 (mkblog.cn)](https://mkblog.cn/881/)