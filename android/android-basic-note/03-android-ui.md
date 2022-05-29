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

![image-20220530011100978](F:/note/synnote/res/img/image-20220530011100978.png)