# 常见控件的使用

## TextView

固定值表示表示给控件指定一个固定的尺寸，单位一般用dp，这是一种屏幕密度无关的尺寸单位，可以保证在不同分辨率的手机上显示效果尽可能地一致。

android:gravity来指定文字的对齐方式，可选值有top、bottom、start、end、center等，可以用“|”来同时指定多个值，这里我们指定的是"center"，效果等同于"center_vertical|center_horizontal"，表示文字在垂直和水平方向都居中对齐。

android:textSize属性可以指定文字的大小。文字大小要使用sp作为单位，这样当用户在系统中修改了文字显示尺寸时，应用程序中的文字大小也会跟着变化。

## Button

Android系统默认会将按钮上的英文字母全部转换成大写，可能是认为按钮上的内容都比较重要吧。如果这不是你想要的效果，可以在XML中添加android:textAllCaps="false"这个属性，这样系统就会保留你指定的原始文字内容了。

```kotlin
// 函数式API写法，监听点击事件
button.setOnClickListener {
	...
}

// 实现接口的方式注册监听器
button.setOnClickListener(this)

override fun onClick(v: View?) {
	when (v?.id) {
		R.id.button -> {
			...
		}
	}
}
```

## EditText

android:hint属性指定了一段提示性的文本

android:maxLines指定了EditText的最大行数

## ImageView

android:src属性给ImageView指定了一张图片

## ProgressBar

ProgressBar用于在界面上显示一个进度条，表示我们的程序正在加载一些数据。通过style属性可以将它指定成水平进度条，style="?android:attr/progressBarStyleHorizontal" 。android:max="100" 给进度条设置一个最大值。

Android控件的可见属性通过android:visibility进行指定，可选值有3种：visible、invisible和gone。visible表示控件是可见的，这个值是默认值，不指定android:visibility时，控件都是可见的。invisible表示控件不可见，但是它仍然占据着原来的位置和大小，可以理解成控件变成透明状态了。gone则表示控件不仅不可见，而且不再占用任何屏幕空间。

## AlertDialog

AlertDialog可以在当前界面弹出一个对话框，这个对话框是置顶于所有界面元素之上的，能够屏蔽其他控件的交互能力，因此AlertDialog一般用于提示一些非常重要的内容或者警告信息。

```kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {   ...     override fun onClick(v: View?) {         
    when (v?.id) {             
        R.id.button -> {                 
            AlertDialog.Builder(this).apply {                     
                setTitle("This is Dialog")                     
                setMessage("Something important.")                     
                setCancelable(false)                     
                setPositiveButton("OK") { dialog, which ->                 
                }                    
                setNegativeButton("Cancel") { dialog, which ->                    			}                     
                show()                 
            }             
        }         
    }     
} 
}
```

在apply函数中为这个对话框设置标题、内容、可否使用Back键关闭对话框等属性，接下来调用setPositiveButton()方法为对话框设置确定按钮的点击事件，调用setNegativeButton()方法设置取消按钮的点击事件，最后调用show()方法将对话框显示出来就可以了。

# 基本布局

## LinearLayout

android:layout_weight。这个属性允许我们使用比例的方式来指定控件的大小，它在手机屏幕的适配性方面可以起到非常重要的作用。

仅指定了EditText的android:layout_weight属性，并将Button的宽度改回了wrap_content。这表示Button的宽度仍然按照wrap_content来计算，而EditText则会占满屏幕所有的剩余空间。

## RelativeLayout

相对于父布局进行定位的android:layout_alignParentLeft

相对于控件进行定位，android:layout_above属性可以让一个控件位于另一个控件的上方，需要为这个属性指定相对控件id的引用

RelativeLayout中还有另外一组相对于控件进行定位的属性，android:layout_alignLeft表示让一个控件的左边缘和另一个控件的左边缘对齐

## FrameLayout

所有的控件都会默认摆放在布局的左上角

# 自定义控件

## 控件和布局的继承结构

![image-20220530011100978](../../res/img/image-20220530011100978.png)

所用的所有控件都是直接或间接继承自View的，所用的所有布局都是直接或间接继承自ViewGroup的。View是Android中最基本的一种UI组件，它可以在屏幕上绘制一块矩形区域，并能响应这块区域的各种事件，我们使用的各种控件其实就是在View的基础上又添加了各自特有的功能。而ViewGroup则是一种特殊的View，它可以包含很多子View和子ViewGroup，是一个用于放置控件和布局的容器。

## 创建自定义控件

自定义布局，include引入即可

如果布局中有一些控件要求能够响应事件，我们还是需要在每个Activity中为这些控件单独编写一次事件注册的代码。比如标题栏中的返回按钮，其实不管是在哪一个Activity中，这个按钮的功能都是相同的，即销毁当前Activity。而如果在每一个Activity中都需要重新注册一遍返回按钮的点击事件，无疑会增加很多重复代码，这种情况最好是使用自定义控件的方式来解决。

```kotlin
class TitleLayout(context: Context, attrs: AttributeSet) : LinearLayout(context, attrs) {
	init {
		LayoutInflater.from(context).inflate(R.layout.title, this)
		titleBack.setOnClickListener {
			val activity = context as Activity
			activity.finish()
		}
		titleEdit.setOnClickListener {
			
		}
	}
}

// 布局中添加自定义控件
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" android:layout_width="match_parent"     android:layout_height="match_parent" >     
<com.example.uicustomviews.TitleLayout         
android:layout_width="match_parent"         
android:layout_height="wrap_content" /> 
</LinearLayout> 
```

在TitleLayout的主构造函数中声明了Context和AttributeSet这两个参数，在布局中引入TitleLayout控件时就会调用这个构造函数。然后在init结构体中需要对标题栏布局进行动态加载，这就要借助LayoutInflater来实现了。通过LayoutInflater的from()方法可以构建出一个LayoutInflater对象，然后调用inflate()方法就可以动态加载一个布局文件。inflate()方法接收两个参数：第一个参数是要加载的布局文件的id，这里我们传入R.layout.title；第二个参数是给加载好的布局再添加一个父布局，这里我们想要指定为TitleLayout，于是直接传入this。分别给返回和编辑这两个按钮注册了点击事件。TitleLayout中接收的context参数实际上是一个Activity的实例，在返回按钮的点击事件里，我们要先将它转换成Activity类型，然后再调用finish()方法销毁当前的Activity。Kotlin中的类型强制转换使用的关键字是as

# ListView

由于手机屏幕空间比较有限，能够一次性在屏幕上显示的内容并不多，当我们的程序中有大量的数据需要展示的时候，就可以借助ListView来实现。ListView允许用户通过手指上下滑动的方式将屏幕外的数据滚动到屏幕内，同时屏幕上原有的数据会滚动出屏幕。查看QQ聊天记录，翻阅微博最新消息

```kotlin
// 
class MainActivity : AppCompatActivity() {     
    private val data = listOf("Apple", "Banana", "Orange", "Watermelon",        "Pear", "Grape", "Pineapple", "Strawberry", "Cherry", "Mango",        "Apple", "Banana", "Orange", "Watermelon", "Pear", "Grape",        "Pineapple", "Strawberry", "Cherry", "Mango")
                                          
override fun onCreate(savedInstanceState: Bundle?) {         super.onCreate(savedInstanceState)         setContentView(R.layout.activity_main)         
val adapter = ArrayAdapter<String>(this,android.R.layout.simple_list_item_1,data)        
listView.adapter = adapter     } 
} 
```

集合中的数据是无法直接传递给ListView的，我们还需要借助适配器来完成。Android中提供了很多适配器的实现类，其中我认为最好用的就是ArrayAdapter。它可以通过泛型来指定要适配的数据类型，然后在构造函数中把要适配的数据传入。ArrayAdapter有多个构造函数的重载，你应该根据实际情况选择最合适的一种。由于我们这里提供的数据都是字符串，因此将ArrayAdapter的泛型指定为String，然后在ArrayAdapter的构造函数中依次传入Activity的实例、ListView子项布局的id，以及数据源。android.R.layout.simple_list_item_1作为ListView子项布局的id，这是一个Android内置的布局文件，里面只有一个TextView，可用于简单地显示一段文本。

## 定制 ListView 界面

```kotlin
// 定义一个实体类，作为ListView适配器的适配类型。imageId表示水果对应图片的资源id
class Fruit(val name:String, val imageId: Int) 

class FruitAdapter(activity: Activity, val resourceId: Int, data: List<Fruit>) : ArrayAdapter<Fruit>(activity, resourceId, data) {     
    override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {      
        // 使用LayoutInflater来为这个子项加载我们传入的布局。false，表示只让我们在父布局中声明的layout属性生效，但不会为这个View添加父布局。因为一旦View有了父布局之后，它就不能再添加到ListView中了
        val view = LayoutInflater.from(context).inflate(resourceId, parent, false)        
        val fruitImage: ImageView = view.findViewById(R.id.fruitImage)        
        val fruitName: TextView = view.findViewById(R.id.fruitName)        
        val fruit = getItem(position) // 获取当前项的Fruit实例        
        if (fruit != null) {             
            fruitImage.setImageResource(fruit.imageId) 
            fruitName.text = fruit.name         
        }         
        return view     
    }
} 
    
class MainActivity : AppCompatActivity() {     
    private val fruitList = ArrayList<Fruit>()     
    override fun onCreate(savedInstanceState: Bundle?) {         
        super.onCreate(savedInstanceState)         
        setContentView(R.layout.activity_main)         
        initFruits() // 初始化水果数据        
        val adapter = FruitAdapter(this, R.layout.fruit_item, fruitList)     
        listView.adapter = adapter     
    }     
    private fun initFruits() {         
        repeat(2) {             
            fruitList.add(Fruit("Apple", R.drawable.apple_pic))            
            fruitList.add(Fruit("Banana", R.drawable.banana_pic))           
            fruitList.add(Fruit("Orange", R.drawable.orange_pic))           
        }     
    } 
}


```



## ListView优化

在FruitAdapter的getView()方法中，每次都将布局重新加载了一遍，当ListView快速滚动的时候，这就会成为性能的瓶颈。仔细观察你会发现，getView()方法中还有一个convertView参数，这个参数用于将之前加载好的布局进行缓存，以便之后进行重用，我们可以借助这个参数来进行性能优化。

```kotlin
class FruitAdapter(activity: Activity, val resourceId: Int, data: List<Fruit>) :        ArrayAdapter<Fruit>(activity, resourceId, data) {     
    override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {        
        val view: View         
        if (convertView == null) {            
            view = LayoutInflater.from(context).inflate(resourceId, parent, false)        
        } else {             
            view = convertView         
        }         
        val fruitImage: ImageView = view.findViewById(R.id.fruitImage)        
        val fruitName: TextView = view.findViewById(R.id.fruitName)        
        val fruit = getItem(position) // 获取当前项的Fruit实例        
        if (fruit != null) {             
            fruitImage.setImageResource(fruit.imageId)             
            fruitName.text = fruit.name         
        }         
        return view     
    } 
} 
```

现在我们在getView()方法中进行了判断：如果convertView为null，则使用LayoutInflater去加载布局；如果不为null，则直接对convertView进行重用。这样就大大提高了ListView的运行效率，在快速滚动的时候可以表现出更好的性能。虽然现在已经不会再重复去加载布局，但是每次在getView()方法中仍然会调用View的findViewById()方法来获取一次控件的实例。我们可以借助一个ViewHolder来对这部分性能进行优化。

```kotlin
class FruitAdapter(activity: Activity, val resourceId: Int, data: List<Fruit>) :        ArrayAdapter<Fruit>(activity, resourceId, data) {     
    inner class ViewHolder(val fruitImage: ImageView, val fruitName: TextView)    
    override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {        
        val view: View         
        val viewHolder: ViewHolder         
        if (convertView == null) {             
            view = LayoutInflater.from(context).inflate(resourceId, parent, false)           
            val fruitImage: ImageView = view.findViewById(R.id.fruitImage)
            val fruitName: TextView = view.findViewById(R.id.fruitName)            
            viewHolder = ViewHolder(fruitImage, fruitName)             
            view.tag = viewHolder         
        } else {             
            view = convertView             
            viewHolder = view.tag as ViewHolder        
        }         
        val fruit = getItem(position) // 获取当前项的Fruit实例        
        if (fruit != null) {             
            viewHolder.fruitImage.setImageResource(fruit.imageId)           
            viewHolder.fruitName.text = fruit.name         
        }         
        return view     
    }
}
```

新增了一个内部类ViewHolder，用于对ImageView和TextView的控件实例进行缓存，Kotlin中使用inner class关键字来定义内部类。当convertView为null的时候，创建一个ViewHolder对象，并将控件的实例存放在ViewHolder里，然后调用View的setTag()方法，将ViewHolder对象存储在View中。当convertView不为null的时候，则调用View的getTag()方法，把ViewHolder重新取出。这样所有控件的实例都缓存在了ViewHolder里，就没有必要每次都通过findViewById()方法来获取控件实例了。

## ListView点击事件

```kotlin
listView.setOnItemClickListener { parent, view, position, id ->            
                                 val fruit = fruitList[position]             
                                 Toast.makeText(this, fruit.name, Toast.LENGTH_SHORT).show()        
                                }
```



# RecyclerView

RecyclerView。它可以说是一个增强版的ListView，不仅可以轻松实现和ListView同样的效果，还优化了ListView存在的各种不足之处。Android官方更加推荐使用RecyclerView。RecyclerView属于新增控件，Google将RecyclerView控件定义在了AndroidX当中，我们只需要在项目的build.gradle中添加RecyclerView库的依赖，就能保证在所有Android系统版本上都可以使用RecyclerView控件了。由于RecyclerView并不是内置在系统SDK当中的，所以需要把完整的包路径写出来。

## RecyclerView 基本用法

为RecyclerView准备一个适配器，新建FruitAdapter类，让这个适配器继承自RecyclerView.Adapter，并将泛型指定为FruitAdapter.ViewHolder。其中，ViewHolder是我们在FruitAdapter中定义的一个内部类

```kotlin
class FruitAdapter(val fruitList: List<Fruit>) :         RecyclerView.Adapter<FruitAdapter.ViewHolder>() {     
    inner class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {        
        val fruitImage: ImageView = view.findViewById(R.id.fruitImage)        
        val fruitName: TextView = view.findViewById(R.id.fruitName)    
    }     
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {        
        val view = LayoutInflater.from(parent.context).inflate(R.layout.fruit_item, parent, false)         
        return ViewHolder(view)     
    }     
    override fun onBindViewHolder(holder: ViewHolder, position: Int) {        
        val fruit = fruitList[position]         
        holder.fruitImage.setImageResource(fruit.imageId)         
        holder.fruitName.text = fruit.name     
    }     
    override fun getItemCount() = fruitList.size 
} 

```

重写onCreateViewHolder()、onBindViewHolder()和getItemCount()这3个方法。onCreateViewHolder()方法是用于创建ViewHolder实例的，我们在这个方法中将fruit_item布局加载进来，然后创建一个ViewHolder实例，并把加载出来的布局传入构造函数当中，最后将ViewHolder的实例返回。onBindViewHolder()方法用于对RecyclerView子项的数据进行赋值，会在每个子项被滚动到屏幕内的时候执行，这里我们通过position参数得到当前项的Fruit实例，然后再将数据设置到ViewHolder的ImageView和TextView当中即可。getItemCount()方法就非常简单了，它用于告诉RecyclerView一共有多少子项，直接返回数据源的长度就可以了。

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {         
    super.onCreate(savedInstanceState)         
    setContentView(R.layout.activity_main)         
    initFruits() // 初始化水果数据        
    val layoutManager = LinearLayoutManager(this)         
    recyclerView.layoutManager = layoutManager         
    val adapter = FruitAdapter(fruitList)         
    recyclerView.adapter = adapter     
} 
```

在onCreate()方法中先创建了一个LinearLayoutManager对象，并将它设置到RecyclerView当中。LayoutManager用于指定RecyclerView的布局方式，这里使用的LinearLayoutManager是线性布局的意思，可以实现和ListView类似的效果。接下来我们创建了FruitAdapter的实例，并将水果数据传入FruitAdapter的构造函数中，最后调用RecyclerView的setAdapter()方法来完成适配器设置，这样RecyclerView和数据之间的关联就建立完成了。

## 实现横向滚动和瀑布流布局

## RecyclerView的点击事件

RecyclerView并没有提供类似于setOnItemClickListener()这样的注册监听器方法，而是需要我们自己给子项具体的View去注册点击事件

```kotlin
class FruitAdapter(val fruitList: List<Fruit>) :         RecyclerView.Adapter<FruitAdapter.ViewHolder>() {     
    ...     
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {        
        val view = LayoutInflater.from(parent.context).inflate(R.layout.fruit_item, parent, false)   
        val viewHolder = ViewHolder(view)         
        viewHolder.itemView.setOnClickListener {             
            val position = viewHolder.adapterPosition             
            val fruit = fruitList[position]             
            Toast.makeText(parent.context, "you clicked view ${fruit.name}",Toast.LENGTH_SHORT).show()         
        }         
        viewHolder.fruitImage.setOnClickListener {
            val position = viewHolder.adapterPosition             
            val fruit = fruitList[position]             
            Toast.makeText(parent.context, "you clicked image ${fruit.name}",                Toast.LENGTH_SHORT).show()         
        }         
        return viewHolder     
    }     ... 
} 
```

在onCreateViewHolder()方法中注册点击事件。上述代码分别为最外层布局和ImageView都注册了点击事件，itemView表示的就是最外层布局。RecyclerView的强大之处也在于此，它可以轻松实现子项中任意控件或布局的点击事件。

# 编写界面实践

## 制作9-Patch图片

## 编写聊天界面



# Kotlin

## 变量延迟初始化

Kotlin语言的许多特性，包括变量不可变，变量不可为空，等等。这些特性都是为了尽可能地保证程序安全而设计的，但是有些时候这些特性也会在编码时给我们带来不少的麻烦。比如，如果你的类中存在很多全局变量实例，为了保证它们能够满足Kotlin的空指针检查语法标准，你不得不做许多的非空判断保护才行，即使你非常确定它们不会为空。

延迟初始化使用的是lateinit关键字，它可以告诉Kotlin编译器，我会在晚些时候对这个变量进行初始化，这样就不用在一开始的时候将它赋值为null了。

```kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {    
    private lateinit var adapter: MsgAdapter     
    override fun onCreate(savedInstanceState: Bundle?) {
        // 通过代码来判断一个全局变量是否已经完成了初始化
        if (!::adapter.isInitialized) {             
            adapter = MsgAdapter(msgList)         
        }          
    }     
    override fun onClick(v: View?) {              
        adapter.notifyItemInserted(msgList.size - 1)         
    } 
} 
```

使用lateinit关键字也不是没有任何风险，如果我们在adapter变量还没有初始化的情况下就直接使用它，那么程序就一定会崩溃，并且抛出一个UninitializedPropertyAccessException异常。当你对一个全局变量使用了lateinit关键字时，请一定要确保它在被任何地方调用之前已经完成了初始化工作，否则Kotlin将无法保证程序的安全性。

## 使用密封类优化代码