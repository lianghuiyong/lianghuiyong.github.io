---
title: MVP使用笔记
date: 2017-04-18 18:24:48
tags: [Android]
---

## 使用到的基类库

[compile 'com.better.android:appbase:x.y.z'](https://github.com/lianghuiyong/AndroidBase/)
    
### Gradle

```gradle

    allprojects {
        repositories {
            jcenter()
            maven { url "https://jitpack.io" }
        }
    }

    android {
        dataBinding {
            enabled = true
        }
    }

    dependencies {
        compile 'com.better.android:appbase:x.y.z'
    }
```

## gradle-wrapper.properties

```gradle
distributionUrl=https\://services.gradle.org/distributions/gradle-3.3-all.zip
```

## MVP使用介绍

<!--more-->

- **V**

```java
public class ExampleV extends BaseAppCompatActivity(or BaseFragment) implements ExampleContract.View{
    @Override
    protected int getLayoutId() {
        return R.layout.Example;
    }
    
    @Override
    protected MvpPresenter getPresenter() {
        return null;
    }
    
    ......
}
```

- **P**

```java
public class ExamplePresenter extends BasePresenter implements ExampleContract.Presenter {
    
    @NonNull
    private ExampleContract.View view;
    
    //你的数据管理仓库
    @NonNull
    private ExampleRepository repository;
    
    public ExamplePresenter(@NonNull ExampleContract.View view) {
        this.view = checkNotNull(view, "view cannot be null!");
        this.repository = ExampleRepository.getInstance(RemoteExampleDataSource.getInstance(), LocalExampleDataSource.getInstance());
    }
}
```

- **Contract**

```java
public interface ExampleContract {
    /**
     * view接口层  处理界面
     */
    interface View extends MvpView{
        ......
    }
    
    /**
     * Presenter接口层 处理业务
     */
    interface Presenter extends MvpPresenter{
        ......
    }
}
```

- **M**

```java
/**
 * 数据管理仓库，控制选择使用remote数据还是local数据（SP、数据库、缓存）
 */
public class ExampleRepository implements ExampleApi {
    @Nullable
    private static ExampleRepository INSTANCE = null;
    @NonNull
    private final ExampleApi localDataSource;
    @NonNull
    private final ExampleApi remoteDataSource;
    
    public ExampleRepository(@NonNull ExampleApi localDataSource, @NonNull ExampleApi remoteDataSource) {
        this.localDataSource = checkNotNull(localDataSource);
        this.remoteDataSource = checkNotNull(remoteDataSource);
    }
    
    public static ExampleRepository getINSTANCE(@NonNull ExampleApi localDataSource, @NonNull ExampleApi remoteDataSource) {
        if (INSTANCE == null) {
            INSTANCE = new ExampleRepository(localDataSource, remoteDataSource);
        }
        return INSTANCE;
    }
    
    @Override
    public Observable<BaseResponseData<String>> register(@Body ExampleRegisterBean sendBean) {
        return remoteDataSource.register(sendBean);
    }
    
    @Override
    public Observable<BaseResponseData<String>> unregister(@Body ExampleUnRegisterBean sendBean) {
        return localDataSource.unregister(sendBean);
    }
}
```

```java
public interface ExampleApi {
 
    /**
     * 注册
     * 使用retrofit请求
     */
    @POST("services/device/register")
    Observable<BaseResponseData<String>> register(@Body RegisterBean sendBean);

    /**
     * 注销
     * 使用非retrofit请求
     */
    Observable<BaseResponseData<String>> unregister(UnRegisterBean sendBean);
}
```

```java
//remote数据实现
public class RemoteExampleDataSource implements ExampleApi {

    private static RemoteExampleDataSource INSTANCE;

    public static RemoteExampleDataSource getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new RemoteExampleDataSource();
        }
        return INSTANCE;
    }
    
    @Override
    public Observable<BaseResponseData<String>> register(@Body RegisterBean sendBean) {
        /**
        * public static ExampleApi getExampleApi() {
        *     if (ExampleApi == null) {
        *         Retrofit retrofit = new Retrofit.Builder()
        *                 .client(new OkHttpClient())
        *                 .baseUrl(UrlConstants.HOST)
        *                 .addConverterFactory(GsonConverterFactory.create())
        *                 .addCallAdapterFactory(RxJava2CallAdapterFactory.create())
        *                 .build();
        *         ExampleApi = retrofit.create(ExampleApi.class);
        *     }
        *     return ExampleApi;
        * }
        */
        return NetWork.getExampleApi().register(sendBean);
    }
    
    @Override
    public Observable<BaseResponseData<String>> unregister(UnRegisterBean sendBean) {
        return null;
    }
}
```

```java
public class LocalExampleDataSource implements ExampleApi {

    @Nullable
    private static LocalExampleDataSource INSTANCE;

    public static LocalExampleDataSource getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new LocalExampleDataSource();
        }
        return INSTANCE;
    }
    
    @Override
    public Observable<BaseResponseData<String>> register(@Body RegisterBean sendBean) {
        return null;
    }
    
    @Override
    public Observable<BaseResponseData<String>> unregister(UnRegisterBean sendBean) {
        ......
        return Observable.just(baseResponseData);
    }
}
```

## 致谢
 
 - [![](https://img.shields.io/badge/OsChina%20App-2.8.0-brightgreen.svg)](http://git.oschina.net/oschina/android-app)
 - [![](https://img.shields.io/badge/RxJava-2.0-brightgreen.svg)](https://github.com/ReactiveX/RxJava)   
 - [![](https://img.shields.io/badge/butterknife-8.5.1-brightgreen.svg)](https://github.com/JakeWharton/butterknife)   
 - [![](https://img.shields.io/badge/todo-MVP-brightgreen.svg)](https://github.com/googlesamples/android-architecture/tree/todo-mvp/) 
 - [![](https://img.shields.io/badge/todo-DataBinding-brightgreen.svg)](https://github.com/googlesamples/android-architecture/tree/todo-databinding/) 
 - [![](https://img.shields.io/badge/BaseRecyclerViewAdapterHelper-2.9.0-brightgreen.svg)](https://github.com/CymChad/BaseRecyclerViewAdapterHelper) 
 - [![](https://img.shields.io/badge/AndroidUtilCode-1.3.5-brightgreen.svg)](https://github.com/Blankj/AndroidUtilCode) 