### 前言
- Android源码基于API14；
- 继承自 [InputEvent](SDK-InputEvent.md) ，实现了 [Parcelable](SDK_Parcelable.md) 接口；

***

### 概述
- 该对象用来记录运动事件（包括mouse，pen，finger，trackball），持有相对或者绝对Motion事件以及其他数据；


- Motion事件通过一个Action Code和一系列坐标轴的值来描述运动（movement），Action Code用来描述状态变化，坐标轴值用来描述位置和其他运动属性；


- 多点触摸       
    - 每一个产生运动轨迹的手指或者其他东西被称为指针（Pointer）；
    - 有些设备可以同时记录多个运动轨迹，多点触摸屏每个手指对应一个指针；
    - 每个pointer都有一个唯一的id（一般通过index获取id），在down的时候被分配，在up或者cancel之前都是有效的，pointer的数量只有在up和down以及cancel的时候会发生改变；


- 批处理
    - 为了提高效率，move事件可能在一个MotionEvent对象中存储一个批次的motion样本；
    - getX(),getY()获取最新坐标，getHistoricalX(int),getHistoricalY(int)获取本批次中的历史样本数据；


- 设备类型
    - MotionEvent内容会随着设备类型而变化；
    - SOURCE_CLASS_POINTER
        - 在指针类型设备(SOURCE_CLASS_POINTER)(比如触摸屏)上，指针坐标是绝对坐标；
        - 每一个完整的手势由一系列的MotionEvent组成，包括状态改变(down，up，cancel)和运动(move)，由down开始新的手势，up或者cancel结束一个手势；
        - 每当一个额外的Pointer发生down或者up时，framework会生成对应的ACTION_POINTER_DOWN或者ACTION_POINTER_UP事件；
        - 有些指针类设备(比如鼠标)支持水平/垂直滚动，滚动事件对应的Action Code是ACTION_SCROLL，包含了水平/垂直方向的相对偏移量，getAxisValue(int)获取相关信息；
    - SOURCE_CLASS_TRACKBALL
        - 在轨迹球设备(SOURCE_CLASS_TRACKBALL)上，指针坐标是相对坐标；
        - 轨迹球手势是由一系列的move和down/up事件组成(轨迹球按钮被按压或者释放)；
    - SOURCE_CLASS_JOYSTICK
        - 在操纵杆设备(SOURCE_CLASS_JOYSTICK)上，指针坐标是绝对坐标；
        - 坐标值被归化到-1.0到1.0范围内，0.0表示中心位置，可以通过InputDevice.getMotionRange(int axes)获取指定轴上的范围；
        - 操纵杆轴，一般包括AXIS_X，AXIS_Y，AXIS_HAT_X，AXIS_HAT_Y，AXIS_Z，AXIS_RZ(Z轴的旋转)；


- 连续性保证
    - 运动事件总是作为连续的事件流传递到View，什么是连续的事件流要视设备而定，比如，对于触摸屏，一个down，一系列的move，一个up或者cancel事件就是一个连续的事件流；
    - 虽然framework视图确保完成的事件流，但是它不保证；
    - 一些事件可能在到达View之前就被丢掉或者修改，从而导致事件流的不连续性；
    - View应该随时准备好处理cancel事件和容忍在前一个事件流还没结束时，就接收到down事件；

***

### 源码

***

### 思考点
