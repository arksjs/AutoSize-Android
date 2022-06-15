![Logo](art/autosize_banner.jpg)
![Official](https://raw.githubusercontent.com/JessYanCoding/MVPArms/master/image/official.jpeg)

<p align="center">
   <a href="https://bintray.com/jessyancoding/maven/autosize/_latestVersion">
    <img src="https://img.shields.io/badge/Jcenter-v1.2.1-brightgreen.svg?style=flat-square" alt="Latest Stable Version" />
  </a>
</p>


##  Android 屏幕适配方案.



### Jcenter ([ ⚠️ 已停止维护: 2022 年 2 月 1 日之后 JCenter 远程仓库将无法使用](https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter)):
``` gradle
    implementation 'com.github.GreeAutoSize-Android:1.0.1'
```

### JitPack:
Step 1. Add the JitPack repository in your root build.gradle at the end of repositories:

```gradle
allprojects {
    repositories {
        ...
        maven { url "https://jitpack.io" }
    }
}
```

Step 2. Add the dependency

```gradle
dependencies {
    implementation 'com.github.GreeAutoSize-Android:1.0.1'
}
```

## Usage
### Step 1 (只需要以下这一步，框架就可以对项目中的所有页面进行适配)
* **请在 AndroidManifest 中填写全局设计图尺寸 (单位 dp)，如果使用副单位，则可以直接填写像素尺寸，不需要再将像素转化为 dp，详情请查看 [demo-subunits](https://github.com/JessYanCoding/AndroidAutoSize/tree/master/demo-subunits)**
```xml
<manifest>
    <application>            
        <meta-data
            android:name="design_width_in_dp"
            android:value="360"/>
        <meta-data
            android:name="design_height_in_dp"
            android:value="640"/>           
     </application>           
</manifest>
```

## Advanced (以下用法看不懂？答应我，认真看 demo 好不好？)

### Activity
* **当某个 Activity 的设计图尺寸与在 AndroidManifest 中填写的全局设计图尺寸不同时，可以实现 CustomAdapt 接口扩展适配参数**
```java
public class CustomAdaptActivity extends AppCompatActivity implements CustomAdapt {

    @Override
    public boolean isBaseOnWidth() {
        return false;
    }

    @Override
    public float getSizeInDp() {
        return 667;
    }
}
```

* **当某个 Activity 想放弃适配，请实现 CancelAdapt 接口**
```java
public class CancelAdaptActivity extends AppCompatActivity implements CancelAdapt {

}
```

### Fragment
* **首先开启支持 Fragment 自定义参数的功能**
```java
AutoSizeConfig.getInstance().setCustomFragment(true);
```

* **当某个 Fragment 的设计图尺寸与在 AndroidManifest 中填写的全局设计图尺寸不同时，可以实现 CustomAdapt 接口扩展适配参数**
```java
public class CustomAdaptFragment extends Fragment implements CustomAdapt {

    @Override
    public boolean isBaseOnWidth() {
        return false;
    }

    @Override
    public float getSizeInDp() {
        return 667;
    }
}
```

* **当某个 Fragment 想放弃适配，请实现 CancelAdapt 接口**
```java
public class CancelAdaptFragment extends Fragment implements CancelAdapt {

}
```

### Subunits (请认真看 demo-subunits，里面有详细介绍)
* 可以在 **pt、in、mm** 这三个冷门单位中，选择一个作为副单位，副单位是用于规避修改 **DisplayMetrics#density** 所造成的对于其他使用 **dp** 布局的系统控件或三方库控件的不良影响，使用副单位后可直接填写设计图上的像素尺寸，不需要再将像素转化为 **dp**


```java
AutoSizeConfig.getInstance().getUnitsManager()
        .setSupportDP(false)
        .setSupportSP(false)
        .setSupportSubunits(Subunits.MM);
```

