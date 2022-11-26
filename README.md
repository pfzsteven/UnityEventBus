# 组件: UnityEventBus

背景: 实现类似`Android`的`EventBus` 事件交互，并在 C#或Unity项目中应用。本次主要以在Unity工程中使用为例。

# 使用说明

1.导入 [UnityEventBus.dll](https://github.com/pfzsteven/UnityEventBus/blob/main/UnityEventBus.dll) 文件到工程里

2.Unity使用示例: 

```c#
public class SomeMonobehaviour : Monobehaviour {

    // 笔者注: Android项目也不要直接注册Activity/Fragment/View 这些系统提供的类，这样做性能会非常低 ！！
    //        所以强烈建议创建一个内部类或者独立类给EventBus 注册/解绑
    private class EventBusImpl {
    
       public void Register() {
          EventBus.GetInstance().Register(this);
       }
       
       public void UnRegister() {
          EventBus.GetInstance().UnRegister(this);
       }
       
       // 笔者注: 在方法上添加 `EventBusSubscribe` 注解即可被识别，否则接收不到事件
       //        XXXFunction - 你的自定义方法名
       //        XXXData - 你自定义的数据类名，跟Android使用方式一致
       [EventBusSubscribe]
       public void XXXFunction(XXXData data) {
       
       }
    }
    
    private readonly EventBusImpl _eventBusImpl = new EventBusImpl();
    
    private void Awake() {
        _eventBusImpl.Register();
    }
    
    private void Destroy() {
       _eventBusImpl.UnRegister();
    }
    
    // 测试发送EventBus消息
    public void TestPostEvent() {
      // XXXData - 你自定义的数据类名
      EventBus.GetInstance().PostEvent(new XXXData(...));
    }
}
```

# 调试截图

![image](https://user-images.githubusercontent.com/4396725/204088516-8cf6e82c-f688-4e57-9374-160a251a972b.png)
