- ##### 修改项目缓存路径为当前项目

  - 寻找目录**%UE安装目录%\\UE_4.25\\Engine\\Config\\BaseEngine.ini**

  - 查找字符串**InstalledDerivedDataBackendGraph**

  - 修改

    - 原条目

      ```ini
      Local=(Type=FileSystem, ReadOnly=false, Clean=false, Flush=false, PurgeTransient=true, DeleteUnused=true, UnusedFileAge=34, FoldersToClean=-1, Path="%ENGINEVERSIONAGNOSTICUSERDIR%DerivedDataCache", EditorOverrideSetting=LocalDerivedDataCache)
      ```

    - 更改为

      ```ini
      Local=(Type=FileSystem, ReadOnly=false, Clean=false, Flush=false, PurgeTransient=true, DeleteUnused=true, UnusedFileAge=34, FoldersToClean=-1, Path="%GAMEDIR%DerivedDataCache", EditorOverrideSetting=LocalDerivedDataCache)
      ```

      

- ##### 如果是win7系统，则要保证版本在Service Pack1 以上

- ##### 1个虚拟单位uu等于现实世界的1cm,所以在建模软件上要把单位调为厘米

- ##### 修改模型坐标轴原点步骤

  - 1.使用鼠标中键按住物体自身坐标原点，并移动到目标位置
  - 2.右击物体坐标原点，选择支点（锚点）->设置为支点（枢纽）偏移

- ##### 框选物体：ctrl+alt+鼠标左键框选

```java
//P7 0:00 ----2020年10月15日15:56:09
```

- ##### 项目打包

  - 先将Intermediate和Saved文件夹删除
  - 再将工程文件夹压缩打包

- ##### 项目迁移

  - 迁移目标目录必须是某个项目的Content目录
  - 一次只能迁移一个目录，但一次能迁移多个对象（需要右击选择资源操作->迁移）
  - 对于网上下载的素材
    - 打开到其Content那一层目录
    - 将里面的内容复制到项目的Content目录下

- ##### 模型

  - 静态网格体
    - 没有互动，仅供观赏
  - 骨架网格体
    - 有动画的

- ##### 旧的挖空型不能减去新的添加型模型，可以通过更改排序使挖空型起作用

- ##### 避免模型的材质和模型一起拉伸：手动更改细节即可

  ![image-20201016142718117](D:\notes\随笔\note-imgs\image-20201016142718117.png)

- ##### 模型勾选中空会使模型具有墙壁和墙壁厚度

  ![image-20201016143020064](D:\notes\随笔\note-imgs\image-20201016143020064.png)

- ##### 固体性

  - ![image-20201016143756249](D:\notes\随笔\note-imgs\image-20201016143756249.png)

  - 固体
    - 默认类型
    - 阻挡游戏中的玩家和射弹
    - 可以是添加型或挖空型
    - 在周围画刷中创建BSP分割
  - 半固体
    - 可以放置在关卡中其不会对周围的世界几何体创建BSP分割的碰撞画刷
    - 阻挡玩家和射弹
    - 只可以是添加型，绝对不可以是挖空型
    - 不会在周围画刷中创建BSP分割
  - 非固体
    - 不会阻挡玩家或射弹
    - 只可以是添加型，不可以是挖空型
    - 不会在周围画刷中创建BSP分割

```java
//P9 14：00 ----2020年10月16日15:53:37
```

```java
//P16 0:00 ----2020年10月19日16:54:20
```

