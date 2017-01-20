[jumpf-Parcelable]:     ../os/Parcelable.md     "Parcelable"
[jumpf-KeyEvent]:       KeyEvent.md             "KeyEvent"
[jumpf-MotionEvent]:    MotionEvent.md          "MotionEvent"
### 前言
- Android源码基于API14;
- 是抽象类，实现了 [Parcelable][jumpf-parcelable] 接口；
- 子类包括 [KeyEvent][jumpf-KeyEvent] 和 [MotionEvent][jumpf-MotionEvent];
***
### 概述
***
### 源码
#### 1.成员变量
-   Token常量
    ```
    /** @hide */
    protected static final int PARCEL_TOKEN_MOTION_EVENT = 1;
    /** @hide */
    protected static final int PARCEL_TOKEN_KEY_EVENT = 2;
    ```
- Parcelable.Creator
    ```
    public static final Parcelable.Creator<InputEvent> CREATOR
            = new Parcelable.Creator<InputEvent>() {
        public InputEvent createFromParcel(Parcel in) {
            int token = in.readInt();
            if (token == PARCEL_TOKEN_KEY_EVENT) {
                return KeyEvent.createFromParcelBody(in);
            } else if (token == PARCEL_TOKEN_MOTION_EVENT) {
                return MotionEvent.createFromParcelBody(in);
            } else {
                throw new IllegalStateException("Unexpected input event type token in parcel.");
            }
        }

        public InputEvent[] newArray(int size) {
            return new InputEvent[size];
        }
    };
    ```
    - 用于Parcelable读取；
#### 2.成员方法
- 构造方法
    ```
    /*package*/ InputEvent() {}
    ```
- 获取事件来源的设备Id
    ```
    public abstract int getDeviceId();
    ```
    - 如果返回0，表明不是物理设备，会关联到默认键盘；其他值是随意的，不要依赖于该值；
    - 相关：InputDevice#getDevice；
- 获取事件来源的设备
    ```
    public final InputDevice getDevice() {
        return InputDevice.getDevice(getDeviceId());
    }
    ```
    - 如果返回null，表示未知；
- 获取事件源
    ```
    public abstract int getSource();
    ```
    - 相关：InputDevice里定义了很多事件源；
- 修改事件源
    ```
    public abstract void setSource(int source);
    ```
- 获取拷贝
    ```
    public abstract InputEvent copy();
    ```
    - 深拷贝；
- 回收
    ```
    public abstract void recycle();
    ```
    - 只有系统可以使用该方法；
- 事件是否被污染
    ```
    public abstract boolean isTainted();
    ```
    - 是否系统已经发现该事件和之前的一系列事件不一致，比如up或者事件，但是之前没有down事件；
- 设置该事件是否被污染
    ```
    public abstract void setTainted(boolean tainted);
    ```
- 事件发生时间(纳秒)
    ```
    public abstract long getEventTimeNano();
    ```
    - 单位是纳秒
### 思考点
- [ ] getDeviceId和getSource()区别
- [ ] InputEvent的回收；
- [ ] Parcelable的使用，token，write，read；
- [ ] Parcelable和Serializable的区别；
