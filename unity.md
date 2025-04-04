# 64

[EditorGUILayout](https://docs.unity.cn/cn/560/ScriptReference/EditorGUILayout.html)

# 63
copy object hierarchy path
```csharp
public class TestGetPath: Editor
{
    [MenuItem("GameObject/GetPath", false, 11)]
    public static void CopyHierarchyPath(){
        GameObject go = Selection.activeGameObject;
        string path = SearchUtils.GetObjectPath(go);
        GUIUtily.systemCopyBuffer = path;
    }
}
```

# 62
unity旋转物体

```csharp
transform.rotation = Quaternion.Euler(new Vector3());
```

# 61
```
IEnumerable.Skip()
IEnumerable.Take()
IEnumerable.SkipWhile(element => element < value)
```

# 60
```
using System.Threading.Tasks;
async
await
```

# 59
[使用 $ 的字符串内插](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/tokens/interpolated)
```csharp
var name = "Mark";
var date = DateTime.Now;

// Composite formatting:
Console.WriteLine("Hello, {0}! Today is {1}, it's {2:HH:mm} now.", name, date.DayOfWeek, date);
// String interpolation:
Console.WriteLine($"Hello, {name}! Today is {date.DayOfWeek}, it's {date:HH:mm} now.");
// Both calls produce the same output that is similar to:
// Hello, Mark! Today is Wednesday, it's 19:40 now.
```

# 58
EventHandler表示将用于处理不具有事件数据的事件的方法。
```csharp
public delegate void EventHandler(object? sender, EventArgs e);
```

# 57
```csharp
using System;
using UnityEngine;
using UnityEngine.Events;

public class Player : MonoBehaviour
{
    //C# 委托/事件/Action
    public delegate void HealthChanged();
    public HealthChanged HealthChangedDelegate;         //C# 委托
    public Action HealthChangedAction;                  //C# Action: 无参数无返回值的委托，相当于一个语法糖（定义无参数无返回值的委托时，如果懒的取名字就用它）。
    public Func<int, string> HealthChangedFunc;         //C# Func: 与 Action 类似，但是可以有返回值。
    public event HealthChanged HealthChangedEvent;      //C# 事件
    //public event EventHandler HealthChangedEvent;      //C# 事件
    
    //Unity 封装 事件/Action
    public UnityEvent HealthChangedUnityEvent;          //Unity 封装事件（好处是脚本挂载到对象后可在 Inspector 面板添加处理函数）
    public UnityAction HealthChangedUnityAction;        //Unity Action（定义和 C# Action 没有任何区别，但 UnityEvent 的 AddListener() 只接受该类型参数。大概是为了命名的统一性吧？）

    public Player()
    {
        //委托/事件/Action 初始化
        HealthChangedDelegate = null;
        HealthChangedAction = null;
        HealthChangedEvent = null;
        HealthChangedUnityAction = null;
        HealthChangedUnityEvent = new UnityEvent();

        //委托/事件/Action 订阅(绑定)
        HealthChangedDelegate += OnHealthChanged;                           //C# 委托/事件/Action 支持 +=/-= 操作符
        HealthChangedAction += OnHealthChanged;                             //可以直接用 +=/-= 来增加/移除处理函数
        HealthChangedEvent += OnHealthChanged;        
        
        HealthChangedUnityAction += OnHealthChanged;
        HealthChangedUnityEvent.AddListener(OnHealthChanged);             //UnityEvent 不支持 +=/-= 操作符（改用 AddListener()函数）
        HealthChangedUnityEvent.AddListener(HealthChangedUnityAction);      //AddListener()函数参数既可以是处理函数，也可以是 UnityAction 实例

        //委托/事件触发(调用)
        HealthChangedDelegate.Invoke();
        HealthChangedAction.Invoke();
        HealthChangedEvent.Invoke();
        HealthChangedUnityAction.Invoke();
        HealthChangedUnityEvent.Invoke();
    }

    public void OnHealthChanged()
    {
        //...
    }
}
```

# 56
[readonly](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/readonly)
 
# 55
Object reference not set to an instance of an object  
未绑定对象

# 54
[RuntimeInitalizeOnLoadMethod]

# 53
```csharp
Resources.LoadAdd<>();
```

# 52
提供支持某些查询的类和接口，这些查询使用语言集成查询 (LINQ)。
```csharp
using System.Linq;
List.Max(a => a.xx)
```

# 51
sealed  
应用于某个类时，sealed 修饰符可阻止其他类继承自该类。

# 50
Image  
Image Type -> Simple  
Use Sprite Mesh  
Rreserve Aspect  
Raycast Target = False  

# 49
[Aspect Ratio Filter](https://docs.unity.cn/cn/2023.2/Manual/script-AspectRatioFitter.html)

# 48
Slice Sprite  
Texture Type = Sprite (2D and UI)  
Sprite Model = Multiple  
Sprite Editor  

# 47
[DoTween](https://dotween.demigiant.com/documentation.php)  
Perference  
Setting Locatoin = DoTween > Resources  
Recycle Tween = True  
AutoPlay = None  
Ease = In Our Sine  
AutoKill = True  


# 46
Canvas Group -> Blocks Raycasts

# 45
EventTragger
[EventType](https://docs.unity3d.com/ScriptReference/EventType.html)  
[UIE](https://docs.unity3d.com/Manual/UIE-Events-Reference.html)  
IXXXHandler

# 44
IBeginDragHandler, IDragHandler, IEndDragHandler  
Layout Element  
IDropHandler  
IPointEnterHandler, IPointExitHandler  
Canvas Group

# 43
```
public void Show(Action onCloseButton)
{
	this.onCloseButton = onCloseButton;

	gameObject.SetActive(true);

	soundEffectButton.Select();
}

optionsButton.onClick.AddListener(() =>
{
	Hide();
	OptionsUI.Instance.Show(Show);
});
```

# 42
```csharp
playerInputActions.Player.Interact.bindings[0].ToString();

public void Rebinding(Binding binding, Action onActionRebound)
{
	playerInputActions.Player.Disable();

	InputAction inputAction;
	int bindingIndex;

	switch (binding)
	{
		default:
		case Binding.Move_Up:
			inputAction = playerInputActions.Player.Move;
			bindingIndex = 1;
			break;
		case Binding.Move_Down:
			inputAction = playerInputActions.Player.Move;
			bindingIndex = 2;
			break;
		case Binding.Move_Left:
			inputAction = playerInputActions.Player.Move;
			bindingIndex = 3;
			break;
		case Binding.Move_Right:
			inputAction = playerInputActions.Player.Move;
			bindingIndex = 4;
			break;
		case Binding.Interact:
			inputAction = playerInputActions.Player.Interact;
			bindingIndex = 0;
			break;
		case Binding.InteractAlt:
			inputAction = playerInputActions.Player.InteractAlternate;
			bindingIndex = 0;
			break;
		case Binding.Pause:
			inputAction = playerInputActions.Player.Pause;
			bindingIndex = 0;
			break;
	}

	inputAction.PerformInteractiveRebinding(bindingIndex)
		.WithControlsExcluding("Mouse")
		.OnComplete(callback =>
		{
			callback.Dispose();
			playerInputActions.Player.Enable();
			onActionRebound();

			PlayerPrefs.SetString(PLAYER_PREFS_BINDINDS, playerInputActions.SaveBindingOverridesAsJson());
			PlayerPrefs.Save();
		})
	.Start();
}
```

# 41
```csharp
PlayerPrefs.SetFloat(PLAYER_PREFS_MUSIC_VOLUME, volume);
PlayerPrefs.Save();
PlayerPrefs.DeleteAll();
```

# 40
- Action是无返回值的泛型委托。
- Action 表示无参，无返回值的委托
- Action<int,string,bool> 表示有传入参数int,string,bool无返回值的委托
- Action<int,int,int,int> 表示有传入4个int型参数，无返回值的委托
- Action至少0个参数，至多16个参数，无返回值。

# 39
[UnityEvent](https://docs.unity3d.com/ScriptReference/Events.UnityEvent.html)
```
using UnityEngine;
using UnityEngine.Events;
using System.Collections;

public class ExampleClass : MonoBehaviour
{
    UnityEvent m_MyEvent;

    void Start()
    {
        if (m_MyEvent == null)
            m_MyEvent = new UnityEvent();

        m_MyEvent.AddListener(Ping);
    }

    void Update()
    {
        if (Input.anyKeyDown && m_MyEvent != null)
        {
            m_MyEvent.Invoke();
        }
    }

    void Ping()
    {
        Debug.Log("Ping");
    }
}
```

# 38
```
Time.scaleTime = 0f;
OnAnyCut.GetInvocationList().Length
```

# 37
File -> Building Settings  
static Loader and LoaderCallback
```csharp
public static class Loader
{

	public enum Scene
	{
		MainMenuScene,
		GameScene,
		LoadingScene
	}

	private static Scene targetScene;

	public static void Load(Scene targetScene)
	{
		Loader.targetScene = targetScene;

		SceneManager.LoadScene(Scene.LoadingScene.ToString());

	}

	public static void LoaderCallback()
	{
		SceneManager.LoadScene(targetScene.ToString());
	}
}
```
```csharp
public class LoaderCallback : MonoBehaviour
{
	private bool isFirstUpdate = true;

	private void Update()
	{
		if (isFirstUpdate)
		{
			isFirstUpdate = false;

			Loader.LoaderCallback();
		}
	}
}
```

# 36
```csharp
AudioClip
AudioSource.PlayClipAtPoint()  
```

# 35
Audio Source  
Audio Maxier  

# 34
shift+space maximize screen  
uv  
UVs are two-dimensional texture coordinates that correspond with the vertex information for your geometry, connecting a surface mesh to texture application.

# 33
Anchor Presets

# 32
[C# 编程指南](https://learn.microsoft.com/zh-cn/dotnet/csharp/programming-guide/)

# 31
[如何实现接口事件（C# 编程指南）](https://learn.microsoft.com/zh-cn/dotnet/csharp/programming-guide/events/how-to-implement-interface-events)

# 30
C# 和 .NET 只支持单一继承。  

接口包含非抽象 class 或 struct 必须实现的一组相关功能的定义。 接口可以定义 static 方法，此类方法必须具有实现。 接口可为成员定义默认实现。 接口不能声明实例数据，如字段、自动实现的属性或类似属性的事件。

GetComponent<interface>  
```
hasProgress = hasProgressGameObject.GetComponent<IHasProgress>();
if (hasProgress == null)
{
	Debug.LogError("Game Object" + hasProgressGameObject + " does not have a component that implements IHasProgress!");
}
```

# 29
LookAt
```csharp
transform.LookAt(Camera.main.transform);
transform.forward = Camera.main.transform.forward;
```

# 28
Canvas  
ProgressBar  
FillAmount  
Event -> Script  

# 27
连击动画  
attack layer  
attack1.anim -> finishScript.cs -> OnStateExit()  
attack2.anim  
attack3.anim  
attack4.anim  
1->2 bool  
2->3 isTrigger   
3->4 isTrigger  

# 26
(Virtual)虚拟方法与(abstract)抽象方法的区别?
虚拟方法和抽象方法都可以供派生类重写和都用override重写。那么它们之间有什么区别呢？
1. 抽象方法使用abstract关键字,虚拟方法使用virtual关键字
2. 抽象方法是可以看成是没有实现体的虚拟方法
3. 虚拟方法与多态性关系密切,虚拟方法为子类提供了重写该方法的选项允许子类完全或部分重写该类的方法，必须写方法体。
抽象方法只是一个定义，没有提供实现部分，也就是没有{}，也不要在里面写内容，需要在子类中实现,抽象方法是一种强制子类重写的方法，否则子类将不能被实例化。
4. 抽象方法必须在派生类中重写，这一点跟接口类似，虚拟方法不必。
5. 抽象方法不能声明方法体，而虚拟方法可以。
6. 抽象类不能被实例化（不可以new），只能实例化实现了全部抽象方法的派生类；而包含虚方法的类可以实例化。
7. 虚拟方法是指能被重载覆盖的方法,而抽象方法是虚拟方法中的特例,指完全没有具体实现的虚拟方法.
8. 如果类中包含抽象方法，那么类就必须定义为抽象类，不论是否还包含其它一般方法
9. 抽象方法是必须被派生类覆写的方法,调用虚拟方法，运行时将确定调用对象是什么类的实例，并调用适当的覆写的方法
10. 一个虚拟方法的实现可以由派生类取代。取代所继承的虚拟方法的实现的过程称为重写该方法；在一个虚拟方法调用中，该调用所涉及的那个实例的运行时类型确定了要被调用的究竟是该方法的哪一个实现。
11. 抽象方法是需要子类去实现的.虚方法,是已经实现了,子类可以去覆盖,也可以不覆盖取决于需求.

# 25
Scriptable Object  
gameObject.transform  
transform.gameObject  
```csharp
[CreateAssetMenu()]
public class KitchenObjectSO : ScriptableObject 
{
	public Transform prefab;
	public Sprite sprite;
	public string objectName;
}

public class KitchenObject : MonoBehaviour
{
	[SerializeField] private KitchenObjectSO kitchenObjectSO;

	public KitchenObjectSO GetKitchenObjectSO ()
	{
		return kitchenObjectSO;
	}
}
```

# 24
localPosition
```csharp
Transform tomatoTransform = Instantiate(tomatoPrefab, counterTopPoint);
tomatoTransform.localPosition = Vector3.zero;
```

# 23
```csharp
private static Player instance;
public static Player Instance {
	get {
		return instance;
	}
	set {
		instance = value
	}
}

public static Player Instance { get; set; }
```

# 22
event(+= or -=)  
delegate(=)  
action(void)  
func(return)  
[区别委托和事件](https://learn.microsoft.com/zh-cn/dotnet/csharp/distinguish-delegates-events)


# 21
```csharp
// GameInput (publisher)
public event EventHandler OnInteractAction;

playerInputActions.Player.Interact.performed += Interact_performed;

private void Interact_performed(UnityEngine.InputSystem.InputAction.CallbackContext obj){
	OnInteractAction?.Invoke(this, EventArgs.Empty);
}

// Player (subscriber)
[SerializeField] private GameInput gameInput;

gameInput.OnInteractAction += GameInput_OnInteractAction;
```

# 20
```
raycastHit.transform.TryGetComponent(out ClearCounter clearCounter)
```

# 19
view tool
```
left mouse: pan
middle mouse: zoom
right mouse: orbit 
alt: fps
```

# 18
```csharp
Physics.Raycast(transform.position, moveDir, playerSize);
Physics.CapsuleCast(transform.position, transform.position + Vector3.up * playerHeight , playerRaduis, moveDir);
```

# 17
```csharp
transform.position += moveDir * Time.deltaTime * moveSpeed;
```

# 16
1. Package Manager -> Input Sytem
2. Edit -> Project Setting -> Player -> Other Setting -> Active Input Handling -> Both
3. Project -> Input Action
4. Action Maps -> Player -> Actions Move -> Action Type: Value &Control Type: Vector2
5. Add UP/Down/Left/Right Composite -> add listend key -> Save Assert
6. PlayerInputActions -> Generate C# Class
```
PlayerInputActions.scheme.binding.ReadValue<Vector2>();
```

# 15
cinemachine
Virtual Camera
Follow
Look At
noise
Body -> bind mode -> world space


# 14
Slerp 线性插值
Lerp 球形插值

# 13
yield
```csharp
var numbers = ProduceEvenNumbers(5);
Console.WriteLine("Caller: about to iterate.");
foreach (int i in numbers)
{
    Console.WriteLine($"Caller: {i}");
}

IEnumerable<int> ProduceEvenNumbers(int upto)
{
    Console.WriteLine("Iterator: start.");
    for (int i = 0; i <= upto; i += 2)
    {
        Console.WriteLine($"Iterator: about to yield {i}");
        yield return i;
        Console.WriteLine($"Iterator: yielded {i}");
    }
    Console.WriteLine("Iterator: end.");
}
// Output:
// Caller: about to iterate.
// Iterator: start.
// Iterator: about to yield 0
// Caller: 0
// Iterator: yielded 0
// Iterator: about to yield 2
// Caller: 2
// Iterator: yielded 2
// Iterator: about to yield 4
// Caller: 4
// Iterator: yielded 4
// Iterator: end.
```
如前面的示例所示，当开始对迭代器的结果进行迭代时，迭代器会一直执行，直到到达第一个 yield return 语句为止。 然后，迭代器的执行会暂停，调用方会获得第一个迭代值并处理该值。 在后续的每次迭代中，迭代器的执行都会在导致上一次挂起的 yield return 语句之后恢复，并继续执行，直到到达下一个 yield return 语句为止。 当控件到达迭代器或 yield break 语句的末尾时，迭代完成。

# 12
Coroutine
```csharp
IEnumerator Fade()
{
    Color c = renderer.material.color;
    for (float alpha = 1f; alpha >= 0; alpha -= 0.1f)
    {
        c.a = alpha;
        renderer.material.color = c;
        yield return null; // back to unity after a frame
    }
}
```

# 11
Timd.deltaTime 
The interval in seconds from the last frame to the current one.

# 10
provent linecast hit own boxCollider.
```
boxCollider.enabled = false;
Physics.Linecast(start, end, blockingLayer);
boxCollider.enabled = true;
```

# 9
StartCorotine

# 8
Perference -> External Tools -> Regenerate project files

# 7
.sln include project
.csproj include dll

# 6
[GameObject](https://docs.unity.cn/2023.2/Documentation/ScriptReference/GameObject.html)
Base class for all entities in Unity Scenes.


# 5
C# GC
https://learn.microsoft.com/zh-cn/dotnet/standard/garbage-collection/

# 4
Singleton Pattern
```csharp
class A:
	public static B single;
	public static B getSingle(){
		if(single == null){
			single = new B();
		}
		else{
			return single;
		}
	}
```


# 3
[Ojbect](https://docs.unity.cn/2023.2/Documentation/ScriptReference/Object.html)

Any public variable you make that derives from Object gets shown in the inspector as a drop target, allowing you to set the value from the GUI.

# 2
import assert from store
```
Toolbar
PackageManager
Packages: My Asserts
Download
Import
```

# 1
(Toolbar, Hierarchy, Game, Scene, Overlays, Inspector, Project, Status bar)[https://docs.unity3d.com/2021.3/Documentation/Manual/UsingTheEditor.html]
