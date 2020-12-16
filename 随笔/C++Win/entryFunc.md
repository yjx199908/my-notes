## 包含头文件

- ```c++
  #include <windows.h>//头文件
  ```

## 程序入口函数

- ```c++
  //WINAPI代表__stdcall参数的传递顺序：从右向左 依次入栈，并且在函数返回前清空堆栈
  int WINAPI WinMain(HINSTANCE hInstance,//当前句柄实例
                     HINSTANCE hPrevInstance,//上一个句柄实例
                     LPSTR lpCmdLine,//命令行参数
                     int nShowCmd)//显示命令 最大化，最小化，正常
  {
      //设计窗口
      //注册窗口
      //创建窗口
      //显示和更新
      //通过循环取消息
      //处理消息（窗口过程）
      
      //1.设计窗口
      WNDCLASS wc;
      
  }
  ```

- 