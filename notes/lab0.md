# lab0中几个头文件的理解

```c++
#include <file_descriptor.hh>
```

1. TUN/TAP设备

  *是操作系统内核中的虚拟网络设备*

​	a. TAP是一个以太网设备，用于操作以太网数据帧等第二层数据包

​	b. TUN模拟了网络层设备，用于操作IP数据包等第三层数据包

操作系统通过TUN/TAP设备向绑定了该设备的用户空间程序发送数据，反之，用户空间程序也可以像操作硬件网络设备那样通过TUN/TAP设备发送数据

*Socket+TunTapFD = FileDescriptor*

2. FDWrapper

   对kernel中的fd的封装

   ```c++
   bool _closed = false; // indicate whether FDWrapper::_fd has benn closed
   bool _eof = false; // indicate whether FDWrapper:_fd is at EOF
   unsigned _read_count = 0, _write_count = 0; // number of times _fd has been read/writen
   ```

   

3. read/write函数

   write和read定义了多个重载

4. *class BufferViewList*

   一个类似c++17中的string_view的类（用于处理只读字符串）

   1. 对于指向的字符串没有所有权
   2. 可以用于取代const char*和const stirng&
   3. 代替const string&可以避免不必要的内存分配 

   ```c++
   #include <socket.hh> //是FileDescriptor的派生类
   ```

   

5. class Address

   封装了class Raw， 内含

   ```c++
   class Raw {
       public:
   	sockaddr_storage storage {};
       operator sockaddr *();
       operator const sockaddr *()const;
   }
   // operator sockaddr *() 对类型转换进行重新解释
   // 使用reinterpret_cast<type>()进行重载
   ```

   