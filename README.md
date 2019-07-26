# About Me
t turns out that many apps use the same or similar layout principles to implement entirely different functionality.

* Creating the About Me App

#### Add a TextView, ImageView, and Styling

```xml
    <style name="NameStyle">
       <item name="android:fontFamily">@font/roboto</item>
       <item name="android:paddingTop">@dimen/small_padding</item>
       <item name="android:textSize">@dimen/text_size</item>
       <item name="android:textColor">@android:color/black</item>
       <item name="android:layout_marginTop">@dimen/layout_margin</item>
    </style>
```

```xml 
    <resources>
       <dimen name="text_size">20sp</dimen>
       <dimen name="small_padding">8dp</dimen>
       <dimen name="layout_margin">16dp</dimen>
    </resources>
```

#### Add a ScrollView
```xml
    <ScrollView
       android:layout_width="match_parent"
       android:layout_height="match_parent">
    
       <LinearLayout
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:orientation="vertical">
    
           <ImageView
               android:layout_width="match_parent"
               android:layout_height="wrap_content"
               app:srcCompat="@android:drawable/ic_menu_view" />
    
           <TextView
               android:layout_width="match_parent"
               android:layout_height="wrap_content"
               android:text="@string/some_long_text"/>
    
       </LinearLayout>
    
    </ScrollView>
```

#### Add EditText, Done Button, ClickHandler

```xml
    findViewById<Button>(R.id.done_button).setOnClickListener {
       addNickname(it)
    }
```

```kotlin
    // Hide the keyboard.
    val imm = getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
    imm.hideSoftInputFromWindow(view.windowToken, 0)
```

#### Implement Data Binding

+ Enable data binding in your build.gradle file in the app module inside the android section:

```kotlin
    dataBinding {
        enabled = true
    }
```

+ Wrap all views in `activity_main.xml` into a `<layout>` tag, and move the namespace declarations into 
the the `<layout>` tag.

+ In MainActivity, create a binding object:

```xml
    private lateinit var binding: ActivityMainBinding
```

+ In onCreate, use DataBindingUtil to set the content view:

```xml
    binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
```

+ Use the binding object to replace all calls to findViewById, for example:

```xml
    binding.doneButton.setOnClickListener….etc
```

#### Apply Data Binding

+ Create a data class MyName for the name and nickname.

```xml
    data class MyName(var name: String = "", var nickname: String = "")
```

+ Add a <data> block to activity_main.xml. The data block goes inside the layout tag but before 
the view tags. Inside the data block, add a variable for the MyName class.

```xml 
    <data>
        <variable
            name="myName"
            type="software.yesaya.aboutme.MyName" />
    </data>
```

+ In name_text, nickname_edit, and nickname_text, replace the references to string text resources with references to 
the variables, for example>

```xml
    android:text="@={myName.name}"
```

+ In MainActivity, create an instance of MyName.

```kotlin
    // Instance of MyName data class.
    private val myName: MyName = MyName("Yesaya R. Athuman")
```

+ And in onCreate(), set binding.myName to it.

```kotlin
    binding.myName = myName
```

+ In addNickname, set the value of nickname in myName, call invalidateAll(), and the data should show 
in your views.

```kotlin
    myName?.nickname = nicknameEdit.text.toString()
    // Invalidate all binding expressions and request a new rebind to refresh UI
    invalidateAll()
```




