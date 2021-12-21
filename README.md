# ExtensionKit
the Extensions Module is wrapper for Unity and .Net's API

简单来说都是对 .Net 和  Unity 的 API 进行了一层封装

由 QFramework 团队官方维护的独立工具包（不依赖 QFramework）。




## 环境要求

* Unity 2018.4LTS

## 安装

* PackageManager

    * add from package git url：https://github.com/liangxiegame/ExtensionKit.git 
    * 或者国内镜像仓库：https://gitee.com/liangxiegame/ExtensionKit.git

* 或者直接复制[此代码](ExtensionKit.cs)到自己项目中的任意脚本中

    

#### QuickStart:

``` csharp
// traditional style
var playerPrefab = Resources.Load<GameObject>("playerPrefab");
var playerObj = Instantiate(playerPrefab);
playerObj.transform.SetParent(null);
playerObj.transform.localRotation = Quaternion.identity;
playerObj.transform.localPosition = Vector3.left;
playerObj.transform.localScale = Vector3.one;
playerObj.layer = 1;
playerObj.layer = LayerMask.GetMask("Default");

Debug.Log("playerPrefab instantiated");


// Extension's Style,same as above 
Resources.Load<GameObject>("playerPrefab")
	.Instantiate()
	.transform
	.Parent(null)
	.LocalRotationIdentity()
	.LocalPosition(Vector3.left)
	.LocalScaleIdentity()
	.Layer(1)
	.Layer("Default")
	.ApplySelfTo(_ => { Debug.Log("playerPrefab instantiated"); });
```


### DotNet Extensions:

#### 1.IsNull,IsNotNull:

``` csharp
var simpleClass = new object();

if (simpleClass.IsNull()) // simpleClass == null
{

}
else if (simpleClass.IsNotNull()) // simpleClasss != null
{

}
```

#### 2.InvokeGracefully:

**Action:**

``` csharp
Action action = () => Debug.Log("action called");
action.InvokeGracefully(); // if (action != null) action();
```

**Func:**

``` csharp
Func<int> func = () => 1;
func.InvokeGracefully();
			
/*
public static T InvokeGracefully<T>(this Func<T> selfFunc)
{
	return null != selfFunc ? selfFunc() : default(T);
}
*/ 
```

**Delegate:**

``` csharp
public delegate void TestDelegate();
...
TestDelegate testDelegate = () => { };
testDelegate.InvokeGracefully();
```



#### 3.ForEach:

**Array:**

``` csharp
var testArray = new int[] {1, 2, 3};
testArray.ForEach(number => Debug.Log(number));
```

**IEnumrable<T>:**

``` csharp
IEnumerable<int> testIenumerable = new List<int> {1, 2, 3};
testIenumerable.ForEach(number => Debug.Log(number));
```

**List<T>:**

``` csharp
var testList = new List<int> {1, 2, 3};
testList.ForEach(number => Debug.Log(number));
testList.ForEachReverse(number => Debug.Log(number));
```

#### 4.Merge:

**Dictionary<T,K>:**

``` csharp
var dictionary1 = new Dictionary<string, string> {{"1", "2"}};
var dictionary2 = new Dictionary<string, string> {{"3", "4"}};
var dictionary3 = dictionary1.Merge(dictionary2);
dictionary3.ForEach(pair => Debug.LogFormat("{0}:{1}", pair.Key, pair.Value));
```

#### 5.CreateDirIfNotExists,DeleteDirIfExists,DeleteFileIfExists

``` csharp
var testDir = Application.persistentDataPath.CombinePath("TestFolder");
testDir.CreateDirIfNotExists();

Debug.Log(Directory.Exists(testDir));
testDir.DeleteDirIfExists();
Debug.Log(Directory.Exists(testDir));

var testFile = testDir.CombinePath("test.txt");
testDir.CreateDirIfNotExists();
File.Create(testFile);
testFile.DeleteFileIfExists();
```

#### 6.ImplementsInterface<T>

``` csharp
this.ImplementsInterface<ISingleton>();
```

#### 7.ReflectionExtension.GetAssemblyCSharp()

``` csharp
var selfType = ReflectionExtension.GetAssemblyCSharp().GetType("QFramework.Example.ExtensionExample");
Debug.Log(selfType);
```

#### 8.string's extension

``` csharp
var emptyStr = string.Empty;

Debug.Log(emptyStr.IsNotNullAndEmpty());
Debug.Log(emptyStr.IsNullOrEmpty());
emptyStr = emptyStr.Append("appended").Append("1").ToString();
Debug.Log(emptyStr);
Debug.Log(emptyStr.IsNullOrEmpty());
```

### Unity Extensions :

#### 1.Enable,Disable

``` csharp
var component = gameObject.GetComponent<MonoBehaviour>();

component.Enable(); // component.enabled = true
component.Disable(); // component.enabled = false
```

#### 2.CaptrueCamera:

``` csharp
var screenshotTexture2D = Camera.main.CaptureCamera(new Rect(0, 0, Screen.width, Screen.height));
```

#### 3.Color:

``` csharp
var color = "#C5563CFF".HtmlStringToColor();
```

#### 4.GameObject:

``` csharp
var boxCollider = gameObject.AddComponent<BoxCollider>();

gameObject.Show(); // gameObject.SetActive(true)
this.Show(); // this.gameObject.SetActive(true)
boxCollider.Show(); // boxCollider.gameObject.SetActive(true)
transform.Show(); // transform.gameObject.SetActive(true)

gameObject.Hide(); // gameObject.SetActive(false)
this.Hide(); // this.gameObject.SetActive(false)
boxCollider.Hide(); // boxCollider.gameObject.SetActive(false)
transform.Hide(); // transform.gameObject.SetActive(false)

this.DestroyGameObj();
boxCollider.DestroyGameObj();
transform.DestroyGameObj();

this.DestroyGameObjGracefully();
boxCollider.DestroyGameObjGracefully();
transform.DestroyGameObjGracefully();

this.DestroyGameObjAfterDelay(1.0f);
boxCollider.DestroyGameObjAfterDelay(1.0f);
transform.DestroyGameObjAfterDelay(1.0f);

this.DestroyGameObjAfterDelayGracefully(1.0f);
boxCollider.DestroyGameObjAfterDelayGracefully(1.0f);
transform.DestroyGameObjAfterDelayGracefully(1.0f);

gameObject.Layer(0);
this.Layer(0);
boxCollider.Layer(0);
transform.Layer(0);

gameObject.Layer("Default");
this.Layer("Default");
boxCollider.Layer("Default");
transform.Layer("Default");
```

#### 5.Graphic

``` csharp
var image = gameObject.AddComponent<Image>();
var rawImage = gameObject.AddComponent<RawImage>();

// image.color = new Color(image.color.r,image.color.g,image.color.b,1.0f);
image.ColorAlpha(1.0f);
rawImage.ColorAlpha(1.0f);
```

#### 6.Image

``` csharp
var image1 = gameObject.AddComponent<Image>();

image1.FillAmount(0.0f); // image1.fillAmount = 0.0f;
```

#### 7.Object

``` csharp
gameObject.Instantiate()
    .Name("ExtensionExample")
	.DestroySelf();

gameObject.Instantiate()
	.DestroySelfGracefully();

gameObject.Instantiate()
	.DestroySelfAfterDelay(1.0f);

gameObject.Instantiate()
	.DestroySelfAfterDelayGracefully(1.0f);

gameObject
	.ApplySelfTo(selfObj => Debug.Log(selfObj.name))
	.Name("TestObj")
	.ApplySelfTo(selfObj => Debug.Log(selfObj.name))
	.Name("ExtensionExample")
	.DontDestroyOnLoad();
```

#### 8.Transform

``` csharp
transform
	.Parent(null)
	.LocalIdentity()
	.LocalPositionIdentity()
	.LocalRotationIdentity()
	.LocalScaleIdentity()
	.LocalPosition(Vector3.zero)
	.LocalPosition(0, 0, 0)
	.LocalPosition(0, 0)
	.LocalPositionX(0)
	.LocalPositionY(0)
	.LocalPositionZ(0)
	.LocalRotation(Quaternion.identity)
	.LocalScale(Vector3.one)
	.LocalScaleX(1.0f)
	.LocalScaleY(1.0f)
	.Identity()
	.PositionIdentity()
	.RotationIdentity()
	.Position(Vector3.zero)
	.PositionX(0)
	.PositionY(0)
	.PositionZ(0)
	.Rotation(Quaternion.identity)
	.DestroyAllChild()
	.AsLastSibling()
	.AsFirstSibling()
	.SiblingIndex(0);

this
	.Parent(null)
	.LocalIdentity()
	.LocalPositionIdentity()
	.LocalRotationIdentity()
	.LocalScaleIdentity()
	.LocalPosition(Vector3.zero)
	.LocalPosition(0, 0, 0)
	.LocalPosition(0, 0)
	.LocalPositionX(0)
	.LocalPositionY(0)
	.LocalPositionZ(0)
	.LocalRotation(Quaternion.identity)
	.LocalScale(Vector3.one)
	.LocalScaleX(1.0f)
	.LocalScaleY(1.0f)
	.Identity()
	.PositionIdentity()
	.RotationIdentity()
	.Position(Vector3.zero)
	.PositionX(0)
	.PositionY(0)
	.PositionZ(0)
	.Rotation(Quaternion.identity)
	.DestroyAllChild()
	.AsLastSibling()
	.AsFirstSibling()
	.SiblingIndex(0);
```

#### 9.UnityAction

``` csharp
UnityAction action = () => { };
UnityAction<int> actionWithInt = num => { };
UnityAction<int, string> actionWithIntString = (num, str) => { };

action.InvokeGracefully();
actionWithInt.InvokeGracefully(1);
actionWithIntString.InvokeGracefully(1, "str");
```



## 10. More

请自己体验更多的 API



## 更多

* QFramework 地址: https://github.com/liangxiegame/qframework