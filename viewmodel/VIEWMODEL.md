# ViewModel

##### 该组件系对viewmodel 订阅事件的类EventBus处理

###### 包含三个模块

* [viewmodel-annotation](ANNOTATION.md) 注解
* [viewmodel-core](CORE.md) viewmodel、lifedata、observer可控粘性事件基类
* [viewmodel-processor](PROCESSOR.md) 注解处理器，处理注解生成订阅代理类

使用方式

1. 首先在使用模块project gradle 中添加mavencentral() 仓库

   ```groovy
    repositories {
           ...
           mavenCentral()
       }
   ```

   然后在使用模块module gradle 中添加

```kotlin
	
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-kapt'
}
android{
 defaultConfig {
       ...
        kapt{
            arguments{
                arg("module_name",project.getName())
            }
        }
    }
}

dependencies {
   
   ...
    implementation "io.github.sunshaobei:satis-core:1.0.0"
    kapt "io.github.sunshaobei"
}


```

2. 使创建的ViewModel类继承 BaseViewModel

```kotlin
class ViewModel(application: Application) :BaseViewModel(application) {
    fun doSomething(){
        setValue("sdfsdfs")
    }
}

```
3. 使activity 基础 MVVMActivity

```kotlin
class SatisViewModelActivity :MVVMActivity<ActivitySatisViewModelBinding,ViewModel>() {
    // 定义需要 在viewModel中订阅的事件 当viewmodel中需要回调时 在viewmodel中调用生成的扩展函数onText即可（如果在子线程使用，也提供了ioXX（） 其中xx表示方法名称，供调用）
    @Observe(sticky = fasle)
    fun onText(s:String){
        Log.e("sunshaobei",s)
    }
}
```

4. 编译后在ViewModel中使用（会生成 相关扩展函数： onText、ioonText）

```kotlin
class ViewModel(application: Application) :BaseViewModel(application) {
    init {
        onText("eenene")
    }
}
```



5. 最后在application初始化时 将使用的模块添加到 "商店"

```kotlin

//有多少个模块使用就相应添加
  SatisViewModel.addObserverStore(ObserveStore_app())
```