# Todo Samples

## mvp

![image-20200426170458558](upload\image-20200426170458558.png)

主要问题：

Activity既要负责自己的业务逻辑，又要负责配置对象的创建。

Injection可以负责提供Repository对象，但是不管是否以单例形式提供，对象的生命周期都需要在业务上层方控制。

## mvp-dagger

![image-20200426170421093](upload\image-20200426170421093.png)

*XXXModule：负责提供View和Presenter*

XXXRepositoryModule：负责提供Repository

对象的生命周期直接交给dagger管理

## mvp-clean

![image-20200426193424908](upload\image-20200426193424908.png)

MVP模式的问题在于，业务都放在Presenter中，随着业务的增加，Presenter可能产生膨胀。

解决方法，将Presenter中的业务，按照用例进行划分，拆分为一个个单独的用例。

## mvp-rxjava

和mvp类似，主要差异在于回调接口的消除和线程调度的方面呢。

## mvp-kotlin

使用kotlin实现了一遍mvp

## mvvm-databinding

和mvp主要区别，vm只管理状态，不再直接更新UI，UI的更新通过databinding自动绑定的方式来实现。

实际上，就是把UI的更新逻辑从主动变为被动。VM维持所有会影响UI更新的状态，状态发生变化就通知UI更新。

## mvvm-livedata

**LiveData 是一个可以感知 Activity 、Fragment生命周期的数据容器**。





