## RecyclerView使用

官方文档：https://developer.android.com/guide/topics/ui/layout/recyclerview

#### 引入库

```gradle
dependencies {
     implementation 'com.android.support:recyclerview-v7:28.0.0'
}
```



#### 创建Adaptor

Adaptor
> 三个重要方法	
> 1. `onCreateViewHolder` 获取ViewHolder
> 2. `onBindViewHolder`对ViewHolder中视图绑定数据
> 3. `getItemCount`返回行数

ViewHolder
> 负责包裹视图, 获取并引用子视图避免频繁findViewById



配置Activity

1. setup adaptor
2. setup layout



示例代码：

```kotlin
class MyAdaptor(private val names: List<String>): RecyclerView.Adapter<MyAdaptor.ViewHolder>() {

    class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val tvName = view.findViewById<TextView>(R.id.tvName)
    }


    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val itemView = LayoutInflater.from(parent.context).inflate(R.layout.item_view, parent, false)
        return ViewHolder(itemView)
    }

    override fun getItemCount(): Int {
        return names.size
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        if (position < names.size) {
            holder.tvName.text = names[position]
        }
    }
}

class MainActivity : AppCompatActivity() {

    private lateinit var recyclerView: RecyclerView
    private lateinit var viewAdaptor: RecyclerView.Adapter<*>
    private lateinit var viewManager: RecyclerView.LayoutManager

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        viewManager = LinearLayoutManager(this)
        viewAdaptor = MyAdaptor(listOf("1111111", "2222222", "3333333", "444444", "555555", "1111111", "2222222", "3333333", "444444", "555555","1111111", "2222222", "3333333", "444444", "555555"))
        recyclerView = findViewById<RecyclerView>(R.id.rvNames).apply {
            setHasFixedSize(true)
            layoutManager = viewManager
            adapter = viewAdaptor
        }
    }
}
```

 

setHasfixedSize用途

> 长话短说就是优化显示性能，知道固定大小的话就可以避免刷新整个layout

```
/**
     * RecyclerView can perform several optimizations if it can know in advance that RecyclerView's
     * size is not affected by the adapter contents. RecyclerView can still change its size based
     * on other factors (e.g. its parent's size) but this size calculation cannot depend on the
     * size of its children or contents of its adapter (except the number of items in the adapter).
     * <p>
     * If your use of RecyclerView falls into this category, set this to {@code true}. It will allow
     * RecyclerView to avoid invalidating the whole layout when its adapter contents change.
     *
     * @param hasFixedSize true if adapter changes cannot affect the size of the RecyclerView.
     */
```

