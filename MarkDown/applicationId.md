#applicationId  
applicationId在安卓设备和商店中是作为应用的唯一标识，所以，一旦发布了应用程序，就不应该再修改applicationId，否则将被视为完全不同的应用。applicationId是在创建项目时，匹配的packagename，除了这个时候他们有关联以外，他们是彼此独立的。在创建完项目后，你单独的修改packagename或者单独修改applicationId，都不会对另外一个造成影响。  
applicationId看起来很像package name，但是它的命名规则是有一些限制的：  

1. 它必须至少有两段，也就是一个或者多个点来间隔它们。
2. 每个小段都必须以字母开头。
3. 所有字符必须为字母数字或下划线[a-zA-Z0-9_]。

如果你想在设备和应用商店中，为你的应用创建不同的版本，比如说“免费版”和“专业版”，则需要创建不同applicationId的应用。在这种需求下，我们就需要定义不同的产品风格。在productFlavors {}块中的每个flavor中，我们可以重新定义applicationId属性，或者可以使用applicationIdSuffix给默认的applicationId附加后缀，如下所示：

```Java
android {
    defaultConfig {
        applicationId "com.example.myapp"
    }
    productFlavors {
        free {
            applicationIdSuffix ".free"
        }
        pro {
            applicationIdSuffix ".pro"
        }
    }
}
```
用上面这个方式，“free”这个flavor中，它的applicationId就是“com.example.myapp.free”。
或者直接使用覆盖的形式，例如：

```Java
android {
    defaultConfig {
        applicationId "com.example.myapp"
    }
    productFlavors {
        free {
            applicationId "com.example.myapp.free"
        }
        pro {
            applicationId "com.example.myapp.pro"
        }
    }
}
```
这样，就可以很明显的看出来applicationId，“free”的是“com.example.myapp.free”。
你还可以在buildTypes中使用applicationIdSuffix，例如：

```Java
android {
    ...
    buildTypes {
        debug {
            applicationIdSuffix ".debug"
        }
    }
}
```     
**注：**为了与之前的SDK tools兼容，如果没有在build.gradle文件中设置applicationId属性，那么构建工具就会使用AndroidManifest.xml文件中的包名作为applicationId。在这种情况下，如果你重构了包名，那么applicationId也会相应的修改。

**提示：**如果需要在清单文件（也就是AndroidManifest.xml文件）中引用applicationId，可以在任何清单属性中使用“${applicationId}”占位符，在编译期间，Gradle会将这个标记替换成实际的applicationId。

**重要的一点**  
packagename仅仅只是文件的一个结构路径，applicationId才是app的唯一标识符，一些第三方要求你填写的packagename，实际上是指应用的唯一标识符（applicationId），这个希望不要搞混了。

**补充**  
虽然packagename和Gradle中的applicationId可能会不同，但是再build之后，build工具会将applicationId复制到apk最终生成的manifest文件中。如果我们在build过后，检查我们的最终manifest文件，就会发现，manifest文件的packagename已经改变。  
*注意：*这里要注意一下，app/src/main目录下的manifest文件，并不是我们这里指的最终生成的manifest。我们所说的最终manifest文件，是指build文件夹下的，也指打包后，反编译出来的manifest文件。
<br>
<img src="https://github.com/xujinhan/Blog/blob/master/Image/applicationid_one.png" alt="applicationid" title="applicationid"width="500"/>
<br>
在这张图片中，我们设置packagename为“com.vickie.laboratory”，设置applicationid为“com.vickie.laboratorys”，为了偷懒，这里只是在后面加了一个s，在看图片的时候需要细心点。
<br>
<img src="https://github.com/xujinhan/Blog/blob/master/Image/applicationid_two.png" alt="applicationid" title="applicationid"width="500"/>
<br>
在第二张图片中，我们可以发现，build之后，右边的manifest中，activity的路径被补全了，在前面添加了package，而上面的package，被替换成了applicationid。
<br>
<img src="https://github.com/xujinhan/Blog/blob/master/Image/applicationid_three.png" alt="applicationid" title="applicationid"width="500"/>
<br>
第三张图片，是我将工程打包成apk之后，再反编译得出的文件，查看反编译出来的manifest，会发现，其中的activity也是同样路径被补全，package被替换成applicationid。  
到此位置applicationid就结束了，如果有更多的注意事项，今后会在底部追加。  
