[jumpf-Parcel]:          Parcel.md                          "Parcel"
[jumpf-Serializable]:    ../../java/io/Serializable.md      "Serializable"  
[jumpf-ClassLoader]:     ../../java/lang/ClassLoader.md     "ClassLoader"
### 前言
- Android源码基于API14；
- 是接口；
- 内部接口Creator<T>；
- 内部接口ClassLoaderCreator<T> extends Creator<T>，继承于内部接口Creator<T>；
- 相关类：[Parcel][jumpf-Parcel]，[Serializable][jumpf-Serializable]，[ClassLoader][jumpf-ClassLoader]；
***
### 概述
- 实现了该接口的类对象可以写入Parcel或者从Parcel中读取数据，读写顺序必须一致；
- 实现类该接口的类必须有一个public static final CREATOR变量，该变量实现了Parcelable.Creator接口；
***
### 源码
#### 1.成员变量
```
public static final int PARCELABLE_WRITE_RETURN_VALUE = 0x0001;
```
```
public static final int CONTENTS_FILE_DESCRIPTOR = 0x0001;
```
#### 2.成员函数
- 描述Parcel中的特殊对象
    ```
    public int describeContents();
    ```
- 写入Parcel
    ```
    public void writeToParcel(Parcel dest, int flags);
    ```
    - flags表示该对象该如何被写入，一般为0；
- Creator\<T\>
    - 读取
        ```
        public T createFromParcel(Parcel source);
        ```
        - 从Parcel中读取一个对象；
    - 创建一个相关类型的数组
        ```
        public T[] newArray(int size);
        ```
        - 创建一个新的数组
- ClassLoaderCreator\<T\>
    - 继承于Creator\<T\>接口；
    - 读取
        ```
        public T createFromParcel(Parcel source, ClassLoader loader);
        ```
        - 用给定的ClassLoader从Parcel读取一个对象；
***
### 思考点
- [ ] PARCELABLE_WRITE_RETURN_VALUE的含义以及createFromParcel中的flags；
- [ ] CONTENTS_FILE_DESCRIPTOR的含义以及describeContents方法；
- [ ] Parcelable嵌套写入，读取；
- [ ] ClassLoader
- [ ] Parcelable存储原理；
- [ ] Parcelable，Parcel与IBinder；
