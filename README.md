# Usage

## Download
Download [UnityEventBus.dll](https://github.com/pfzsteven/UnityEventBus/blob/main/UnityEventBus.dll) and import into the `Asset/Plugins` folder.

## Example

```c# 
{
   // For example, create a script file named 'events.cs'.It contains declared classes.
   public class LoginSuccessEvent {
       
       public readonly UserBean User;
       // ... and so on
       
       public LoginSuccessEvent(UserBean u) {
          User = u;
       }
   }
   
   public class LogoutSuccessEvent {
     
   }
 
}

```

```c#
public class SomeMonobehaviour : Monobehaviour {

    private class EventBusImpl {
    
       public void Register() {
          EventBus.GetInstance().Register(this);
       }
       
       public void UnRegister() {
          EventBus.GetInstance().UnRegister(this);
       }
       
       // Important: Must declare [EventBusSubscribe]
       [EventBusSubscribe]
       public void OnLoginSuccessEvent(LoginSuccessEvent e) {
         // refresh ui here
       }
       
       [EventBusSubscribe]
       public void OnLogOutEvent(LogoutSuccessEvent e) {
         // refresh ui here
       }
    }
    
    private readonly EventBusImpl _eventBusImpl = new EventBusImpl();
    
    private void Awake() {
        _eventBusImpl.Register();
    }
    
    private void Destroy() {
       _eventBusImpl.UnRegister();
    }
}
```

```C#
class SomeLogicClass {
 
   public void LoginSuccess() {
      UserBean user = new UserBean();
      EventBus.GetInstance().PostEvent(new LoginSuccessEvent(user));
   }
   
   public void LogOutSuccess() {
      EventBus.GetInstance().PostEvent(new LogoutSuccessEvent());
   }
}
```
