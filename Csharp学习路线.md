---
date: 2026-02-28
aliases:
  - C#学习路线
tags:
  - study
---
# 入门
## 实践项目
![[15.结束场景逻辑实现_代码【2024】.cs]]
- 光标隐藏
```
      Console.CursorVisible = false;[^1]
```
- 控制台及缓存区的大小控制[^2]
```
 //通过两个变量来存储 舞台的大小
 int w = 50;
 int h = 30;
 //设置舞台（控制台）的大小
 Console.SetWindowSize(w, h);
 Console.SetBufferSize(w, h);
```
- 控制台的清空
```
  Console.Clear();
```
- 设置光标位置
```
//3.设置光标的位置
//控制台左上角为原点0 0 右侧是X轴正方向 下方是Y轴正方向 它是一个平面二维坐标系
//注意：
//1.边界问题
//2.横纵距离单位不同 1y = 2x 视觉上的
Console.SetCursorPosition(10, 5);
Console.WriteLine("123123");
```
- 颜色设置
```
 //4.设置颜色相关
 //文字颜色设置
 Console.ForegroundColor = ConsoleColor.Red;
 Console.WriteLine("123123123");
 Console.ForegroundColor = ConsoleColor.Green;
 //背景颜色设置
 //Console.BackgroundColor = ConsoleColor.White;
 //重置背景颜色过后 需要Clear一次 才能把整个背景颜色改变
 //Console.Clear();
```
- 关闭控制台
```
 //6.关闭控制台
 Environment.Exit(0);
```
- 移动
```
  //黄色方块感觉像人一样 这个人身上有位置信息
  // x y
  int x = 0;
  int y = 0;

  //不停的输入 wasd键 都可以控制它移动
  //根据不停 就分析 用while循环是最简单的一种方式
  while (true)
  {
      //第一种清空之前信息的方式
      //Console.Clear();
      Console.SetCursorPosition(x, y);
      Console.Write("■");
      //得到玩家的输入信息
      char c = Console.ReadKey(true).KeyChar;
      //第二种方式把之前的方块擦除了
      Console.SetCursorPosition(x, y);
      Console.Write("  ");
      switch (c)
      {
          //贯穿
          case 'W':
          case 'w':
              y -= 1;
              //改变位置过后 判断新位置 是否越界
              if( y < 0 )
              {
                  y = 0;
              }
              break;
          case 'A':
          case 'a':
              //中文符号 在控制台上占2个位置
              x -= 2;
              if( x < 0 )
              {
                  x = 0;
              }
              break;
          case 'S':
          case 's':
              y += 1;
              if( y > Console.BufferHeight - 1 )
              {
                  y = Console.BufferHeight - 1;
              }
              break;
          case 'D':
          case 'd':
              x += 2;
              if( x > Console.BufferWidth - 2 )
              {
                  x = Console.BufferWidth - 2;
              }
              break;
      }
  }
```
- 生成随机数
```
 int i = r.Next(); //生成一个非负数的随机数
 Console.WriteLine(i);

 i = r.Next(100); // 生成一个 0~99的随机数 左边始终是0 左包含  右边是100 右不包含
 Console.WriteLine(i);

 i = r.Next(5, 100); // 生成一个 5到99的随机数 左包含 右不包含
 Console.WriteLine(i);
```
## 大纲
![[C 入门【2024】.xmind]]
## 个人知识点记忆
[[个人知识点记忆]]
# 基础
## 实践项目
![[10.结束场景逻辑.cs]]
## 必备知识
## 枚举
1.申明

```

//1.namespace语句块中（常用）
//2.class语句块中 struct语句块中
//注意：枚举不能在函数语句块中申明！！！

enum E_MonsterType
{
    Normal,//0

    Boss,//1
}

enum E_PlayerType
{ 
    Main,
    Other,
}

```

2.使用与类型转换
```
 //申明枚举变量
 //自定义的枚举类型  变量名 = 默认值;(自定义的枚举类型.枚举项)
 E_PlayerType playerType = E_PlayerType.Other;

 if( playerType == E_PlayerType.Main )
 {
     Console.WriteLine("主玩家逻辑");
 }
 else if(playerType == E_PlayerType.Other)
 {
     Console.WriteLine("其它玩家逻辑");
 }

 //枚举和switch是天生一对
 E_MonsterType monsterType = E_MonsterType.Boss;
 switch (monsterType)
 {
     case E_MonsterType.Normal:
         //Console.WriteLine("普通怪物逻辑");
         //break;
     case E_MonsterType.Boss:
         Console.WriteLine("Boss逻辑");
         break;
     default:
         break;
 }
 
   #region 知识点四 枚举的类型转换
  // 1.枚举和int互转
  int i = (int)playerType;
  Console.WriteLine(i);
  //int 转枚举
  playerType = 0;

  // 2.枚举和string相互转换
  string str = playerType.ToString();
  Console.WriteLine(str);

  //把string转成枚举呢
  //Parse后 第一个参数 ：你要转为的是哪个 枚举类型 第二个参数：用于转换的对应枚举项的字符串
  //转换完毕后 是一个通用的类型 我们需要用括号强转成我们想要的目标枚举类型
  playerType = (E_PlayerType)Enum.Parse(typeof(E_PlayerType), "Other");
  Console.WriteLine(playerType);

  #endregion
```
## 数组
```
基础的数组与简单总结
 // 变量类型[] 数组名;//只是申明了一个数组 但是并没有开房
 // 变量类型 可以是我们学过的 或者 没学过的所有变量类型
 int[] arr1;

 // 变量类型[] 数组名 = new 变量类型[数组的长度];
 int[] arr2 = new int[5]; //这种方式 相当于开了5个房间 但是房间里面的int值 默认为0

 // 变量类型[] 数组名 = new 变量类型[数组的长度]{内容1,内容2,内容3,.......};
 int[] arr3 = new int[5] { 1, 2, 3, 4, 5 };

 // 变量类型[] 数组名 = new 变量类型[]{内容1,内容2,内容3,.......};
 int[] arr4 = new int[] { 1,2,3,4,5,6,7,8,9}; //后面的内容就决定了 数组的长度 “房间数”

 // 变量类型[] 数组名 = {内容1,内容2,内容3,.......};
 int[] arr5 = { 1,3,4,5,6};//后面的内容就决定了 数组的长度 “房间数”


 bool[] arr6 = { true, false };
 #endregion

 #region 知识点三 数组的使用

 int[] array = { 1, 2, 3, 4, 5 };
 //1.数组的长度
 // 数组变量名.Length
 Console.WriteLine(array.Length);

 //2.获取数组中的元素
 //数组中的下标和索引 他们是从0开始的
 //通过 索引下标去 获得数组中某一个元素的值时
 //一定注意！！！！！！！！
 //不能越界  数组的房间号 范围 是 0 ~ Length-1
 Console.WriteLine(array[0]);
 Console.WriteLine(array[2]);
 Console.WriteLine(array[4]);

 //3.修改数组中的元素
 array[0] = 99;
 Console.WriteLine(array[0]);

 //4.遍历数组 通过循环 快速获取数组中的每一个元素
 Console.WriteLine("**********************");
 for (int i = 0; i < array.Length; i++)
 {
     Console.WriteLine(array[i]);
 }
 Console.WriteLine("**********************");
 //5.增加数组的元素
 // 数组初始化以后 是不能够 直接添加新的元素的
 int[] array2 = new int[6];
 //搬家
 for (int i = 0; i < array.Length; i++)
 {
     array2[i] = array[i];
 }
 array = array2;
 for (int i = 0; i < array.Length; i++)
 {
     Console.WriteLine(array[i]);
 }
 array[5] = 999;
 Console.WriteLine("**********************");
 //6.删除数组的元素
 // 数组初始化以后 是不能够 直接删除元素的
 // 搬家的原理
 int[] array3 = new int[5];
 //搬家
 for (int i = 0; i < array3.Length; i++)
 {
     array3[i] = array[i];
 }
 array = array3;
 Console.WriteLine(array.Length);

 //7.查找数组中的元素
 // 99 2 3 4 5 
 // 要查找 3这个元素在哪个位置
 // 只有通过遍历才能确定 数组中 是否存储了一个目标元素
 int a = 3;

 for (int i = 0; i < array.Length; i++)
 {
     if( a == array[i] )
     {
         Console.WriteLine("和a相等的元素在{0}索引位置", i);
         break;
     }
 }

 #endregion

 //总结
 //1.概念：同一变量类型的数据集合
 //2.一定要掌握的知识：申明，遍历，增删查改
 //3.所有的变量类型都可以申明为 数组
 //4.她是用来批量存储游戏中的同一类型对象的 容器  比如 所有的怪物 所有玩家
```
```
二维数组与简单总结
            #region 知识点一 基本概念
            //二维数组 是使用两个下标(索引)来确定元素的数组
            //两个下标可以理解成 行标  和 列标
            //比如矩阵
            // 1 2 3
            // 4 5 6
            // 可以用二维数组 int[2,3]表示 
            // 好比 两行 三列的数据集合
            #endregion

            #region 知识点二 二维数组的申明

            //变量类型[,] 二维数组变量名;
            int[,] arr; //申明过后 会在后面进行初始化

            //变量类型[,] 二维数组变量名 = new 变量类型[行,列];
            int[,] arr2 = new int[3, 3];

            //变量类型[,] 二维数组变量名 = new 变量类型[行,列]{ {0行内容1, 0行内容2, 0行内容3.......}, {1行内容1, 1行内容2, 1行内容3.......}.... };
            int[,] arr3 = new int[3, 3] { { 1, 2, 3 }, 
                                          { 4, 5, 6 }, 
                                          { 7, 8, 9 } };

            //变量类型[,] 二维数组变量名 = new 变量类型[,]{ {0行内容1, 0行内容2, 0行内容3.......}, {1行内容1, 1行内容2, 1行内容3.......}.... };
            int[,] arr4 = new int[,] { { 1, 2, 3 },
                                       { 4, 5, 6 },
                                       { 7, 8, 9 } };

            //变量类型[,] 二维数组变量名 = { {0行内容1, 0行内容2, 0行内容3.......}, {1行内容1, 1行内容2, 1行内容3.......}.... };
            int[,] arr5 = { { 1, 2, 3 },
                            { 4, 5, 6 },
                            { 7, 8, 9 } };
            #endregion

            #region 知识点三 二维数组的使用
            int[,] array = new int[,] { { 1, 2, 3 },
                                        { 4, 5, 6 } };
            //1.二维数组的长度
            //我们要获取 行和列分别是多长
            //得到多少行
            Console.WriteLine(array.GetLength(0));
            //得到多少列
            Console.WriteLine(array.GetLength(1));

            //2.获取二维数组中的元素
            // 注意：第一个元素的索引是0 最后一个元素的索引 肯定是长度-1
            Console.WriteLine(array[0, 1]);
            Console.WriteLine(array[1, 2]);

            //3.修改二维数组中的元素
            array[0, 0] = 99;
            Console.WriteLine(array[0, 0]);
            Console.WriteLine("**********");
            //4.遍历二维数组
            for (int i = 0; i < array.GetLength(0); i++)
            {
                for (int j = 0; j < array.GetLength(1); j++)
                {
                    //i 行 0 1
                    //j 列 0 1 2
                    Console.WriteLine(array[i, j]);
                    //0,0  0,1  0,2
                    //1,0  1,1  1,2
                }
            }

            //5.增加数组的元素
            // 数组 声明初始化过后 就不能再原有的基础上进行 添加 或者删除了
            int[,] array2 = new int[3, 3];
            for (int i = 0; i < array.GetLength(0); i++)
            {
                for (int j = 0; j < array.GetLength(1); j++)
                {
                    array2[i, j] = array[i, j];
                }
            }
            array = array2;
            array[2, 0] = 7;
            array[2, 1] = 8;
            array[2, 2] = 9;
            Console.WriteLine("**********");
            for (int i = 0; i < array.GetLength(0); i++)
            {
                for (int j = 0; j < array.GetLength(1); j++)
                {
                    //i 行 0 1
                    //j 列 0 1 2
                    Console.WriteLine(array[i, j]);
                    //0,0  0,1  0,2
                    //1,0  1,1  1,2
                }
            }

            //6.删除数组的元素
            //留给大家思考 自己去做一次

            //7.查找数组中的元素
            // 如果要在数组中查找一个元素是否等于某个值
            // 通过遍历的形式去查找

            #endregion
       
            //总结：
            //1.概念：同一变量类型的 行列数据集合
            //2.一定要掌握的内容：申明，遍历，增删查改
            //3.所有的变量类型都可以申明为 二维数组
            //4.游戏中一般用来存储 矩阵，再控制台小游戏中可以用二维数组 来表示地图格子
            
            //交错数组 是 数组的数组，每个维度的数量可以不同

//注意：二维数组的每行的列数相同，交错数组每行的列数可能不同
#endregion

#region 知识点二 数组的申明

//变量类型[][] 交错数组名;
int[][] arr1;

//变量类型[][] 交错数组名 = new 变量类型[行数][];
int[][] arr2 = new int[3][];

//变量类型[][] 交错数组名 = new 变量类型[行数][]{ 一维数组1, 一维数组2,........ };
int[][] arr3 = new int[3][] { new int[] { 1, 2, 3 },
                              new int[] { 1, 2 },
                              new int[] { 1 }};

//变量类型[][] 交错数组名 = new 变量类型[][]{ 一维数组1, 一维数组2,........ };
int[][] arr4 = new int[][] { new int[] { 1, 2, 3 },
                              new int[] { 1, 2 },
                              new int[] { 1 }};

//变量类型[][] 交错数组名 = { 一维数组1, 一维数组2,........ };
int[][] arr5 = { new int[] { 1, 2, 3 },
                 new int[] { 1, 2 },
                 new int[] { 1 }};
#endregion

#region 知识点三 数组的使用
int[][] array = { new int[] { 1,2,3},
                  new int[] { 4,5} };
//1.数组的长度
//行
Console.WriteLine(array.GetLength(0));
//得到某一行的列数
Console.WriteLine(array[0].Length);

//2.获取交错数组中的元素
// 注意：不要越界
Console.WriteLine(array[0][1]);

//3.修改交错数组中的元素
array[0][1] = 99;
Console.WriteLine(array[0][1]);

//4.遍历交错数组
for (int i = 0; i < array.GetLength(0); i++)
{
    for (int j = 0; j < array[i].Length; j++)
    {
        Console.Write(array[i][j] + " ");
    }
    Console.WriteLine();
}

//5.增加交错数组的元素
//6.删除交错数组的元素
//7.查找交错数组中的元素
#endregion
        
//总结
//1. 概念：交错数组 可以存储同一类型的m行不确定列的数据
//2. 一定要掌握的内容：申明、遍历、增删查改
//3. 所有的变量类型都可以申明为 交错数组
//4. 一般交错数组很少使用 了解即可
```



[^1]: 控制台的光标隐藏

[^2]: 缓冲区：可打印内容区域的宽高

## 值和引用类型
### 一般的引用类型
```
 //无符号整形
 //byte b = 1;
 //ushort us = 1;
 //uint ui = 1;
 //ulong ul = 1;
 ////有符号整形
 //sbyte sb = 1;
 //short s = 1;
 //int i = 1;
 //long l = 1;
 ////浮点数
 //float f = 1f;
 //double d = 1.1;
 //decimal de = 1.1m;
 ////特殊类型
 //bool bo = true;
 //char c = 'A';
 //string str = "strs";
 //复杂数据类型
 // enum 枚举 
 // 数组 (一维，二维，交错)

 //把以上 学过的 变量类型 分成 值类型和引用类型
 //引用类型: string, 数组, 类（未学习）
 //值类型: 其它、结构体（未学习） 
 //1.使用上的区别
 
//值类型
int a = 10;
//引用类型
int[] arr = new int[] { 1, 2, 3, 4 };

//申明了一个b让其等于之前的a
int b = a;
//申明了一个arr2让其等于之前的arr
int[] arr2 = arr;
Console.WriteLine("a={0}, b={1}", a, b);
Console.WriteLine("arr[0]={0}, arr2[0]={1}", arr[0], arr2[0]);

b = 20;
arr2[0] = 5;
Console.WriteLine("修改了b和arr2[0]之后");
Console.WriteLine("a={0}, b={1}", a, b);
Console.WriteLine("arr[0]={0}, arr2[0]={1}", arr[0], arr2[0]);

//值类型 在相互赋值时 把内容拷贝给了对方  它变我不变
//引用类型的相互赋值 是 让两者指向同一个值  它变我也变

//2.为什么有以上区别
//值类型 和 引用类型 存储在的 内存区域 是不同的 存储方式是不同的
//所以就造成了 他们在使用上的区别

// 值类型存储在 栈空间  —— 系统分配，自动回收，小而快
// 引用类型 存储在 堆空间 —— 手动申请和释放，大而慢

//new 了 就是开了新房间 和之前的 没有什么关系了 所以 arr不会有任何变化
arr2 = new int[] { 99,3,2,1};
Console.WriteLine("arr[0]={0}, arr2[0]={1}", arr[0], arr2[0]);
```


### 特殊引用类型string和断点分析
```
  #region 知识点一 复习值和引用类型
  //值类型——它变我不变——存储在栈内存中
  // 无符号整形  有符号整形 浮点数 char bool 结构体（未学习）

  //引用类型——它变我也变——存储在堆内存中
  // 数组（一维、二维、交错）  string  类（未学习）

  #endregion

  #region 知识点二 string的它变我不变
  //string str1 = "123";
  //string str2 = str1;
  ////因为string是引用类型 按理说 应该是它变我也变
  //// string非常的特殊 它具备 值类型的特征 它变我不变
  //str2 = "321";

  //Console.WriteLine(str1);
  //Console.WriteLine(str2);

  //string 虽然方便 但是有一个小缺点 就是频繁的 改变string 重新赋值
  //会产生 内存垃圾
  // 优化替代方案 我们会在 C#核心当中进行讲解 

  #endregion

  //通过断点调试 在监视窗口中查看 内存信息
  string str1 = "123";
  string str2 = str1;
  //因为string是引用类型 按理说 应该是它变我也变
  // string非常的特殊 它具备 值类型的特征 它变我不变
  str2 = "321";

  Console.WriteLine(str1);
  Console.WriteLine(str2);
```
## 函数
####  基础理解
```
  //函数（方法）
  //本质是一块具有名称的代码块
  //可以使用函数（方法）的名称来执行该代码块
  //函数（方法）是封装代码进行重复使用的一种机制

  //函数（方法）的主要作用
  //1.封装代码
  //2.提升代码复用率（少写点代码）
  //3.抽象行为
  #endregion

  #region 知识点二 函数写在哪里
  //1.class语句块中
  //2.struct语句块中
  #endregion

  #region 知识点三 基本语法
  //    1      2      3                4
  // static 返回类型 函数名(参数类型 参数名1, 参数类型 参数名2, .......)
  //{
  //      函数的代码逻辑;
  //      函数的代码逻辑;
  //      函数的代码逻辑;
  //      .............
  //       5
  //      return 返回值;(如果有返回类型才返回)
  //}

  //1. 关于static 不是必须的 在没有学习类和结构体之前 都是必须写的

  //2-1. 关于返回类型 引出一个新的关键字  void(表示没有返回值)
  //2-2. 返回类型 可以写任意的变量类型  14种变量类型 + 复杂数据类型（数组、枚举、结构体、类class）

  //3. 关于函数名 使用帕斯卡命名法命名  myName（驼峰命名法）  MyName(帕斯卡命名法)

  //4-1. 参数不是必须的，可以有0~n个参数  参数的类型也是可以是任意类型的 14种变量类型 + 复杂数据类型（数组、枚举、结构体、类class）
  //     多个参数的时候 需要用 逗号隔开
  //4-2. 参数名 驼峰命名法

  //5. 当返回值类型不为void时 必须通过新的关键词 return返回对应类型的内容  （注意：即使是void也可以选择性使用return）
  #endregion

  #region 知识点四 实际运用
  //1.无参无返回值函数
  //1     2      3      4
  static void SayHellow()
  {
      Console.WriteLine("你好世界");
      // 5 在没有返回值时 也就是返回值类型是void 可以省略
      //return;
  }

  //2.有参无返回值函数
  // 1    2       3         4
  static void SayYourName(string name)
  {
      Console.WriteLine("你的名字是：{0}", name);
      // 5
      // return;省略了
  }

  //3.无参有返回值函数
  //1      2        3        4
  static string WhatYourName()
  {
      // 5 对应返回值类型的 内容 返回出去
      return "唐老狮";
  }

  //4.有参有返回值函数
  // 1    2   3      4
  static int Sum(int a, int b)
  {
      //int c = a + b;
      //return c;
      // 5 retrun后面可以写一个表达式 只要这个表达式的结果和返回值类型是一致的就行
      return a + b;
  }

  //5.有参有多返回值函数

  // 传入两个数 然后计算两个数的和 以及他们两的平均数 得出结果返回出来
  // 函数的返回值 一定是一个类型 只能是一个内容
  //1     2     3       4
  static int[] Calc(int a, int b)
  {
      int sum = a + b;
      int avg = sum / 2;
      //int[] arr = { sum, avg };
      //return arr;
      // 5  
      // 如果用数组作为返回值出去 那么前提是 使用者 知道这个数组的规则
      return new int[] { sum, avg};
  }

  #endregion

  #region 知识点五 关于return
  //即使函数没有返回值，我们也可以使用return，
  //return可以直接不执行之后的代码，直接返回到函数外部

  static void Speak(string str)
  {
      if( str == "混蛋" )
      {
          return;
      }
      Console.WriteLine(str);
  }

  #endregion


  //总结
  //1.基本概念
  //2.函数写在哪里 —— class 或者 struct中
  //3.基本语法  1  2  3  4  5
  //4. return 可以提前结束函数逻辑   程序是线性执行的  从上到下执行

  static void Main(string[] args)
  {
      Console.WriteLine("函数");
      //使用函数 直接 写函数名（参数） 即可
      SayHellow();
      // 参数可以是 常量 变量 函数都可以
      // 参数 一定是传一个 能得到对应类型的表达式
      string str = "唐老狮";
      //传入一个string变量
      SayYourName(str);
      //传入一个string 常量
      SayYourName("唐老师2");
      //传入一个返回值时string的函数 
      SayYourName(WhatYourName());

      //有返回值的函数  要不直接拿返回值来用
      //要不就是拿变量 接收它的结果
      string str2 = WhatYourName();

      //也可以直接调用 但是 返回值 对我们来说就没用了
      WhatYourName();

      Console.WriteLine(Sum(100, 200));


      int[] arr = Calc(5, 7);
      Console.WriteLine(arr[0] + " " + arr[1]);

      Speak("混蛋123");
```
#### ref与out
```
 #region 知识点一 学习ref和out的原因
 //学习ref和out的原因
 //它们可以解决 在函数内部改变外部传入的内容 里面变了 外面也要变

 static void ChangeValue(int value)
 {
     value = 3;
 }

 static void ChangeArrayValue(int[] arr)
 {
     arr[0] = 99;
 }

 static void ChangeArray(int[] arr)
 {
     //重新申明了一个 数组
     arr = new int[] { 10, 20, 30 };
 }

 #endregion

 #region 知识点二 ref和out的使用
 //函数参数的修饰符
 //当传入的值类型参数在内部修改时 或者引用类型参数在内部重新申明时
 //外部的值会发生变化

 //ref
 static void ChangeValueRef(ref int value)
 {
     //out传入的变量必须在内部赋值 ref不用
     value = 3;
 }

 static void ChangeArrayRef( ref int[] arr )
 {
     arr = new int[] { 100, 200, 300 };
 }

 //out
 static void ChangeValueOut(out int value)
 {
     //out传入的变量必须在内部赋值 ref不用
     value = 99;
 }

 static void ChangeArrayOut(out int[] arr)
 {
     arr = new int[] { 999, 888, 777 };
 }

 #endregion

 #region 知识点三 ref和out的区别
 //1.ref传入的变量必须初始化  out不用
 //2.out传入的变量必须在内部赋值  ref不用

 // ref传入的变量必须初始化 但是在内部 可改可不改
 // out传入的变量不用初始化 但是在内部 必须修改该值（必须赋值）
 #endregion


 //总结
 //1.ref和out的作用 ： 解决值类型和引用类型 在函数内部 改值 或者 重新申明 能够影响外部传入的变量 让其也被修改
 //2.使用上：就是在申明参数的时候 前面加上 ref和out的 关键字即可 使用时 同上
 //3.区别
 // ref传入的变量必须初始化 但是在内部 可改可不改
 // out传入的变量不用初始化 但是在内部 必须修改该值（必须赋值）
```
#### 变长参数与参数默认值
```
        #region 知识点二 变长参数关键词
        //举例  函数要计算 n个整数的和
        //static int Sum(int a, int b,..........)

        //变长参数关键字 params
        static int Sum(params int[] arr)
        {
            int sum = 0;
            for (int i = 0; i < arr.Length; i++)
            {
                sum += arr[i];
            }
            return sum;
        }
        
        //params int[] 意味着可以传入n个int参数 n可以等于0  传入的参数会存在arr数组中
        // 注意：
        //1.params关键字后面必为数组
        //2.数组的类型可以是任意的类型


        //3.函数参数可以有 别的参数和 params关键字修饰的参数
        //4.函数参数中只能最多出现一个params关键字 并且一定是在最后一组参数 前面可以有n个其它参数
        static void Eat( string name, int a, int b, params string[] things)
        {

        }

        #endregion

        #region 知识点三 参数默认值
        //有参数默认值的参数 一般称为可选参数
        //作用是 当调用函数时可以不传入参数，不传就会使用默认值作为参数的值
        static void Speak(string str = "我没什么话可说")
        {
            Console.WriteLine(str);
        }


        //注意：
        //1.支持多参数默认值 每个参数都可以有默认值
        //2.如果要混用 可选参数 必须写在 普通参数后面

        static void Speak2(string a, string test, string name = "唐老狮", string str = "我没什么话可说")
        {

        }
        #endregion
        

        //总结
        // 1 变长参数关键字 params
        // 作用： 可以传入n个同类型参数   n可以是0
        // 注意：
        // 1. params后面必须是数组 意味着只能是同一类型的可变参数
        // 2. 变长参数只能有一个
        // 3. 必须在所有参数最后写变长参数 

        // 2 参数默认值（可选参数）
        // 作用：可以给参数默认值 使用时可以不传参 不传用默认的 传了用传的
        // 注意：
        // 1. 可选参数可以有多个
        // 2. 正常参数比写在可选参数前面，可选参数只能写在所有参数的后面
```
#### 函数的重载
```
   #region 知识点一 基本概念
   //重载概念
   //在同一语句块(class或者struct)中
   //函数（方法）名相同
   //参数的数量不同 
   //或者
   //参数的数量相同，但参数的类型或顺序不同

   //作用：
   //1.命名一组功能相似的函数，减少函数名的数量，避免命名空间的污染
   //2.提升程序可读性
   #endregion

   #region 知识点二 实例
   //注意：
   //1.重载和返回值类型无关，只和参数类型，个数，顺序有关
   //2.调用时 程序会自己根据传入的参数类型判断使用哪一个重载
   static int CalcSum(int a, int b)
   {
       return a + b;
   }

   //参数数量不同
   static int CalcSum(int a, int b, int c)
   {
       return a + b + c;
   }

   //数量相同 类型不同
   static float CalcSum(float a, float b)
   {
       return a + b;
   }

   //数量相同 类型不同
   static float CalcSum(int a, float f)
   {
       return a + f;
   }

   //数量相同 顺序不同
   static float CalcSum(float f, int a)
   {
       return f + a;
   }

   //ref 和 out

   // ref和out 可以理解成 他们也是一种变量类型 所以可以用在重载中 但是 ref和out不能同时修饰
   static float CalcSum(ref float f, int a)
   {
       return f + a;
   }

   static float CalcSum(int a, int b, params int[] arr)
   {
       return 1;
   }


   #endregion


   //总结
   //概念：同一个语句块中，函数名相同，参数数量、类型、顺序不同的函数 就称为我们的重载函数
   //注意：和返回值无关
   //作用：一般用来处理不同参数的同一类型的逻辑处理
```
#### 递归函数
```
  #region 知识点一 基本概念
  //递归函数 就是 让函数自己调用自己
  //static void Fun()
  //{
  //    if( false )
  //    {
  //        return;
  //    }
  //    Fun();
  //}
  //一个正确的递归函数
  // 1.必须有结束调用的条件
  // 2.用于条件判断的 这个条件 必须改变 能够达到停止的目的
  #endregion

  #region 知识点二 实例
  //用递归函数打印出 0到10
  //递归函数 就是自己调用自己
  static void Fun(int a)
  {
      //第四步：结束条件
      if( a > 10 )
      {
          return;
      }
      //第二步：完成要求 打印
      Console.WriteLine(a);
      //第三部：完成一个 递归的变化 作为我们条件的判断
      ++a;
      //第一步：构造了一个递归
      Fun(a);
  }
```
## 结构体
```using System;

namespace Lesson12_结构体
{
    #region 知识点一 基本概念
    //结构体 是一种自定义变量类型  类似枚举需要自己定义
    //它是数据和函数的集合
    //在结构体中 可以申明各种变量和方法

    //作用：用来表现存在关系的数据集合 比如用结构体表现学生 动物 人类等等
    #endregion

    #region 知识点二 基本语法
    //1.结构体一般写在 namespace语句块中
    //2.结构体关键字 struct

    //struct 自定义结构体名
    //{
    //    // 第一部分
    //    // 变量

    //    // 第二部分
    //    // 构造函数(可选)

    //    // 第三部分 
    //    // 函数
    //}
    // 注意 结构体名字 我们的规范 是 帕斯卡命名法
    #endregion

    #region 知识点三 实例
    // 表现学生数据 的 结构体
    // 申明结构体 和 申明结构体变量 也是两个概念
    // 申明结构体
    struct Student
    {
        #region 知识点五 访问修饰符
        //修饰结构体中变量和方法 是否能被外部使用
        //public 公共的 可以被外部访问
        //private 私有的 只能在内容使用
        //默认不写 为private
        #endregion


        //变量
        //结构体申明的变量 不能直接初始化
        //变量类型 可以写任意类型 包括结构体 但是 不能是自己的结构体 可以是其它的
        //Student s;// 不能是自己的结构体
        //年龄
        public int age;
        //性别
        public bool sex;
        //学号
        public int number;
        //姓名
        public string name;

        //构造函数
        #region 知识点六 结构体的构造函数
        //基本概念
        //1.没有返回值
        //2.函数名必须和结构体名相同
        //3.必须有参数
        //4.如果申明了构造函数 那么必须在其中对所有变量数据初始化

        //构造函数 一般是用于在外部方便初始化的
        public Student(int age, bool sex, int number, string name)
        {
            //新的关键字 this 
            //代表自己
            this.age = age;
            this.sex = sex;
            this.number = number;
            this.name = name;
        }

        #endregion

        //函数方法
        //表现这个数据结构的行为

        //注意 在结构体中的方法 目前不需要加static关键字 
        public void Speak()
        {
            //函数中可以直接使用结构体内部申明的变量
            Console.WriteLine("我的名字是{0},我今年{1}岁", name, age);
        }
        //可以根据需求 写无数个函数的
    }

    #endregion


   

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("结构体");

            #region 知识点四 结构体的使用
            //变量类型 变量名;
            Student s1;
            s1.age = 10;
            s1.sex = false;
            s1.number = 1;
            s1.name = "唐老狮";
            s1.Speak();

            Student s2 = new Student(18, true, 2, "小红");
            s2.Speak();
            #endregion
        }
    }

    // 总结
    //1概念：结构体 struct 是变量和函数的集合 用来表示特定的数据集合

    // 访问修饰符： public 和private 用来修饰变量和方法的 public 外部可以调用 private内部用 不写的话默认就是private
    // 构造函数 ： 没有返回值 函数名和结构体名相同 可以重载 主要是帮助我们快速初始化结构体对象的

    //注意：
    //1.在结构体中申明的变量 不能初始化  只能在外部或者在函数中赋值（初始化）
    //2.在结构体中申明的函数 不用加static的
}
```
## 基本排序问题
### 冒泡排序
```
           #region 知识点一 排序的基本概念
           //排序是计算机内经常进行的一种操作，其目的是将一组“无序”的记录序列调整为“有序”的记录序列
           //常用的排序例子
           // 8 7 1 5 4 2 6 3 9
           // 把上面的这个无序序列 变为 有序（升序或降序）序列的过程
           // 1 2 3 4 5 6 7 8 9 （升序）
           // 9 8 7 6 5 4 3 2 1 （降序）

           //在程序中 序列一般 存储在数组当中
           //所以 排序往往是对 数组进行排序
           int[] arr = new int[] { 8, 7, 1, 5, 4, 2, 6, 3, 9, };
           //把数组里面的内容变为有序的
           #endregion

           #region 知识点二 冒泡排序基本原理
           // 8 7 1 5 4 2 6 3 9
           // 两两相邻
           // 不停比较
           // 不停交换
           // 比较n轮

           #endregion

           #region 知识点三 代码实现

           //第一步 如何比较数组中两两相邻的数
           //8, 7, 1, 5, 4, 2, 6, 3, 9
           //从头开始
           //第n个数和第n+1个数 比较
           //for (int n = 0; n < arr.Length - 1; n++)
           //{
           //    //如果 第n个数 比第n+1个数 大 他们就要交换位置
           //    if( arr[n] > arr[n + 1] )
           //    {
           //        // 第二步 交换位置
           //        // 中间商不赚差价 
           //        int temp = arr[n];
           //        arr[n] = arr[n + 1];
           //        arr[n + 1] = temp;
           //    }
           //}

           //第三部 如何换m轮？
           //有几个数 就比较多少轮
           //for (int m = 0; m < arr.Length; m++)
           //{
           //    // 尽一次循环 就需要比较一轮
           //    for (int n = 0; n < arr.Length - 1; n++)
           //    {
           //        //如果 第n个数 比第n+1个数 大 他们就要交换位置
           //        if (arr[n] > arr[n + 1])
           //        {
           //            // 第二步 交换位置
           //            // 中间商不赚差价 
           //            int temp = arr[n];
           //            arr[n] = arr[n + 1];
           //            arr[n + 1] = temp;
           //        }
           //    }
           //}

           //for (int i = 0; i < arr.Length; i++)
           //{
           //    Console.WriteLine(arr[i]);
           //}

           //第四步 优化
           //1.确定位置的数字 不用比较了 
           // 确定了一轮后 极值（最大或者最小）已经放到了对应的位置（往后放）
           // 所以 没完成n轮 后面位置的数 就不用再参与比较了
           //for (int m = 0; m < arr.Length; m++)
           //{
           //    // 尽一次循环 就需要比较一轮
           //    for (int n = 0; n < arr.Length - 1 - m; n++)
           //    {
           //        //如果 第n个数 比第n+1个数 大 他们就要交换位置
           //        if (arr[n] > arr[n + 1])
           //        {
           //            // 第二步 交换位置
           //            // 中间商不赚差价 
           //            int temp = arr[n];
           //            arr[n] = arr[n + 1];
           //            arr[n + 1] = temp;
           //        }
           //    }
           //}
           //外面申明一个标识 来表示 该轮是否进行了交换
           bool isSort = false;
           //2.特使情况的优化
           for (int m = 0; m < arr.Length; m++)
           {
               //每一轮开始时 默认没有进行过交换
               isSort = false;
               // 尽一次循环 就需要比较一轮
               for (int n = 0; n < arr.Length - 1 - m; n++)
               {
                   //如果 第n个数 比第n+1个数 大 他们就要交换位置
                   if (arr[n] > arr[n + 1])
                   {
                       isSort = true;
                       // 第二步 交换位置
                       // 中间商不赚差价 
                       int temp = arr[n];
                       arr[n] = arr[n + 1];
                       arr[n + 1] = temp;
                   }
               }
               //当一轮结束过后 如果isSort这个标识 还是false
               //那就意味着 已经是最终的序列了 不需要再判断交换了
               if( !isSort )
               {
                   break;
               }
           }

           for (int i = 0; i < arr.Length; i++)
           {
               Console.WriteLine(arr[i]);
           }

           #endregion

           //总结
           //基本概念
           //两两相邻
           //不停比较
           //不停交换
           //比较m轮

           //套路写法
           //两层循环
           //外层轮数
           //内层比较
           //两值比较
           //满足交换

           //如果优化
           //1.比过不比 
           //2.加入bool
```
### 选择排序
```
   #region 知识点一 选择排序基本原理
   // 8 7 1 5 4 2 6 3 9
   // 新建中间商
   // 依次比较
   // 找出极值（最大或最小）
   // 放入目标位置
   // 比较n轮
   #endregion

   #region 知识点二 代码实现
   //实现升序 把 大的 放在最后面
   int[] arr = new int[] { 8, 7, 1, 5, 4, 2, 6, 3, 9 };

   ////第一步 申明一个中间商 来记录索引
   ////每一轮开始 默认第一个都是极值
   //int index = 0;
   ////第二步
   ////依次比较
   //for (int n = 1; n < arr.Length; n++)
   //{
   //    //第三步
   //    //找出极值（最大值）
   //    if( arr[index] < arr[n] )
   //    {
   //        index = n;
   //    }
   //}

   ////第四步 放入目标位置
   ////Length - 1 - 轮数
   ////如果当前极值所在位置 就是目标位置 那就没必要交换了
   //if( index != arr.Length - 1 - 轮数 )
   //{
   //    int temp = arr[index];
   //    arr[index] = arr[arr.Length - 1 - 轮数];
   //    arr[arr.Length - 1 - 轮数] = temp;
   //}

   //第五步 比较m轮
   for (int m = 0; m < arr.Length; m++)
   {
       //第一步 申明一个中间商 来记录索引
       //每一轮开始 默认第一个都是极值
       int index = 0;
       //第二步
       //依次比较
       // -m的目的 是排除上一轮 已经放好位置的数
       for (int n = 1; n < arr.Length - m; n++)
       {
           //第三步
           //找出极值（最大值）
           if (arr[index] < arr[n])
           {
               index = n;
           }
       }

       //第四步 放入目标位置
       //Length - 1 - 轮数
       //如果当前极值所在位置 就是目标位置 那就没必要交换了
       if (index != arr.Length - 1 - m)
       {
           int temp = arr[index];
           arr[index] = arr[arr.Length - 1 - m];
           arr[arr.Length - 1 - m] = temp;
       }
   }

   for (int i = 0; i < arr.Length; i++)
   {
       Console.Write(arr[i] + " ");
   }

   #endregion

   //总结
   //基本概念
   // 新建中间商
   // 依次比较
   // 找出极值
   // 放入目标位置
   // 比较n轮

   //套路写法
   //两层循环
   //外层轮数
   //内层寻找
   //初始索引
   //记录极值
   //内存循环外交换
```
# 核心
## 类与对象
- 定义（类）
    //基本概念
    // 具有相同特征
    // 具有相同行为
    // 一类事物的抽象
    // 类是对象的模板
    // 可以通过类创建出对象
    // 类的关键词
    // class
    一般声明在namespace语句块中
- 类的语法与示例运用
```
 类申明的语法
    class 类名
    {
        //特征——成员变量
        //行为——成员方法
        //保护特征——成员属性

        //构造函数和析构函数
        //索引器
        //运算符重载
        //静态成员
    }
```
```
类申明实例
    //这个类是用来形容人类的
    //命名：用帕斯卡命名法 
    //注意：同一个语句块中的不同类 不能重名
    class Person
    {
        //特征——成员变量
        //行为——成员方法
        //保护特征——成员属性

        //构造函数和析构函数
        //索引器
        //运算符重载
        //静态成员
    }
    //这个类用来表示机器
    class Machine
    {
        //特征——成员变量
        //行为——成员方法
        //保护特征——成员属性

        //构造函数和析构函数
        //索引器
        //运算符重载
        //静态成员
    }
```
- （类）对象的定义
 // 类的申明 和 类对象(变量)申明是两个概念  
 // 类的申明 类似 枚举 和 结构体的申明  类的申明相当于申明了一个自定义变量类型
 // 而对象 是类创建出来的 
 // 相当于申明一个指定类的变量
 // 类创建对象的过程 一般称为实例化对象
 // 类对象 都是引用类型的
 - 实例化对象的语法与实例
```
 实例化对象基本语法
            //类名 变量名;
            //类名 变量名 = null; (null代表空)
            //类名 变量名 = new 类名();
            #endregion

            #region 知识点七 实例化对象 
            Person p;
            Person p2 = null;//null 代表空 不分配堆内存空间
            Person p3 = new Person();//相当于一个人对象
            Person p4 = new Person();//相当于又是一个人对象
            //注意
            //虽然他们是来自一个类的实例化对象
            //但是他们的 特征 行为等等信息 都是他们独有的
            //千万千万 不要觉得他们是共享了数据 两个人 你是你 我是我 彼此没有关系

            Machine m = new Machine();
            Machine m1 = new Machine();
```
- 小结
 // 类的申明 和 类对象的申明时两个概念
 // 类的申明 是申明对象的模板 用来抽象（形容）显示事物的
 // 类对象的申明 是用来表示现实中的 对象个体的

 // 类是一个自定义的变量类型
 // 实例化一个类对象 是在申明变量
## 成员变量与访问修饰符
### 成员变量
```
基本规则
//1.申明在类语句块中
//2.用来描述对象的特征
//3.可以是任意变量类型
//4.数量不做限制
//5.是否赋值根据需求来定

//性别枚举
enum E_SexType
{
    Man,
    Woman,
}
//位置结构体
struct Position
{

}
//宠物类
class Pet
{

}
class Person
{
    //特征——成员变量
    //姓名
    public string name = "唐老狮";
    //年龄
    public int age;
    //性别
    public E_SexType sex;
    //女朋友
    //如果要在类中申明一个和自己相同类型的成员变量时
    //不能对它进行实例化
    public Person gridFriend;
    //朋友
    public Person[] boyFriend;
    //位置
    public Position pos;
    //宠物
    private Pet pet = new Pet();
}
```
### 访问修饰符
  // public —— 公共的  自己(内部)和别人(外部)都能访问和使用
  // private —— 私有的  自己(内部)才能访问和使用  不写 默认为private
  // protected —— 保护的  自己(内部)和子类才能访问和使用
  // 目前决定类内部的成员 的 访问权限
  
###   成员变量的使用和初始值
```
   Person p = new Person();
     //值类型来说 数字类型 默认值都是0  bool类型 false  
  //引用类型的 null
  //交给大家一个看默认值的小技巧  default(变量类型) 就能得到默认值
  Console.WriteLine(default(Person));

  p.age = 10;
  Console.WriteLine(p.age);
```
### 小结
  //成员变量
  //描述特征
  //类中申明
  //赋值随意
  //默认值，值不相同
  //默认值，引用为null
  //任意类型
  //任意数量

  //访问修饰符
  //3P
  //public 公共 内外
  //private 私有的 内
  //protected 保护的 内和子类

## 成员方法
### 成员方法的定义与声明
- 基本概念
 // 1.申明在类语句块中
 // 2.是用来描述对象的行为的
 // 3.规则和函数申明规则相同
 // 4.受到访问修饰符规则影响
 // 5.返回值参数不做限制
 // 6.方法数量不做限制

 //注意：
 //1.成员方法不要加static关键字
 //2.成员方法 必须实例化出对象 再通过对象来使用 相当于该对象执行了某个行为
 //3.成员方法 受到访问修饰符影响
-  声明实例
```
  class Person
 {
     /// <summary>
     /// 判断是否成年
     /// </summary>
     /// <returns></returns>
     public bool IsAdult()
     {
         return age >= 18;
     }

     /// <summary>
     /// 说话的行为
     /// </summary>
     /// <param name="str">说话的内容</param>
     public void Speak(string str)
     {
         Console.WriteLine("{0}说{1}", name, str);
     }

     /// <summary>
     /// 添加朋友的方法
     /// </summary>
     /// <param name="p">传入新朋友</param>
     public void AddFriend(Person p)
     {
         if(friends == null)
         {
             friends = new Person[] { p };
         }
         else
         {
             //新建一个 房子数组
             Person[] newFriends = new Person[friends.Length + 1];
             //搬家
             for (int i = 0; i < friends.Length; i++)
             {
                 newFriends[i] = friends[i];
             }
             //把新加的朋友放到最后一个
             newFriends[newFriends.Length - 1] = p;
             //地址重定向
             friends = newFriends;
         }
     }

     public string name;
     public int age;
     //朋友们
     public Person[] friends;
 }
```
### 成员方法的使用
```
  //2.成员方法 必须实例化出对象 再通过对象来使用 相当于该对象执行了某个行为
  Person p = new Person();
  p.name = "唐老狮";
  p.age = 17;
  p.Speak("我爱你");

  if(p.IsAdult())
  {
      p.Speak("我要耍朋友");
  }

  Person p2 = new Person();
  p2.name = "火山哥";
  p2.age = 16;

  p.AddFriend(p2);

  for (int i = 0; i < p.friends.Length; i++)
  {
      Console.WriteLine(p.friends[i].name);
  }
```
### 小结
  //成员方法
  //描述行为
  //类中申明
  //任意数量
  //返回值和参数根据需求决定

## 构造函数与析构函数
### 构造函数的定义与声明
- 定义
  //在实例化对象时 会调用的用于初始化的函数
  //如果不写 默认存在一个无参构造函数
- 声明
```
 //构造函数的写法
  //1.没有返回值
  //2.函数名和类名必须相同
  //3.没有特殊需求时 一般都是public的
  //4.构造函数可以被重载
//5.this代表当前调用该函数的对象自己

//注意：
// 如果不自己实现无参构造函数而实现了有参构造函数
// 会失去默认的无参构造

 class Person
 {
     public string name;
     public int age;

     //类中是允许自己申明无参构造函数的
     //结构体是不允许
     public Person()
     {
         name = "唐老狮";
         age = 18;
     }

     public Person(int age)
     {
         //this代表当前调用该函数的对象自己 
         this.age = age;
     }

     public Person(string name)
     {
         this.name = name;
     }

     public Person(int age, string name):this(age + 10)
     {
         Console.WriteLine("Person两个参数构造函数调用");
     }
     }
```
- 特殊写法
  //可以通过this 重用构造函数代码
  //访问修饰符 构造函数名(参数列表):this(参数1,参数2....)
### 析构函数
```
//基本概念
//当引用类型的堆内存被回收时，会调用该函数
//对于需要手动管理内存的语言（比如C++），需要在析构函数中做一些内存回收处理
//但是C#中存在自动垃圾回收机制GC
//所以我们几乎不会怎么使用析构函数。除非你想在某一个对象被垃圾回收时，做一些特殊处理
//注意：
//在Unity开发中析构函数几乎不会使用，所以该知识点只做了解即可

//基本语法
// ~类名()
// {
// }

    //当引用类型的堆内存被回收时
    //析构函数 是当垃圾 真正被回收的时候 才会调用的函数
    ~Person()
    {

    }
```

### GC的机制与实际体现
- 机制

| //垃圾回收，英文简写GC（Garbage Collector）<br>//垃圾回收的过程是在遍历堆(Heap)上动态分配的所有对象<br>//通过识别它们是否被引用来确定哪些对象是垃圾，哪些对象仍要被使用<br>//所谓的垃圾就是没有被任何变量，对象引用的内容<br>//垃圾就需要被回收释放<br><br>//垃圾回收有很多种算法，比如<br>//引用计数(Reference Counting)<br>//标记清除(Mark Sweep)<br>//标记整理(Mark Compact)<br>//复制集合(Copy Collection)<br><br>//注意：<br>//GC只负责堆(Heap)内存的垃圾回收<br>//引用类型都是存在堆(Heap)中的，所以它的分配和释放都通过垃圾回收机制来管理<br><br>//栈(Stack)上的内存是由系统自动管理的<br>//值类型在栈(Stack)中分配内存的，他们有自己的生命周期，不用对他们进行管理，会自动分配和释放<br><br>//C#中内存回收机制的大概原理<br>//0代内存     1代内存     2代内存<br>//代的概念：<br>//代是垃圾回收机制使用的一种算法（分代算法）<br>//新分配的对象都会被配置在第0代内存中<br>//每次分配都可能会进行垃圾回收以释放内存(0代内存满时) <br><br>//在一次内存回收过程开始时，垃圾回收器会认为堆中全是垃圾，会进行以下两步<br>//1.标记对象 从根（静态字段、方法参数）开始检查引用对象，标记后为可达对象，未标记为不可达对象<br>//  不可达对象就认为是垃圾<br>//2.搬迁对象压缩堆  （挂起执行托管代码线程） 释放未标记的对象 搬迁可达对象 修改引用地址<br><br>//大对象总被认为是第二代内存  目的是减少性能损耗，提高性能<br>//不会对大对象进行搬迁压缩  85000字节（83kb）以上的对象为大对象 |     |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |     |
- 体现
  Person p = new Person(18,"唐老狮");
  Console.WriteLine(p.age);

  p = null;

  //手动触发垃圾回收的方法 
  //一般情况下 我们不会频繁调用
  //都是在 Loading过场景时 才调用
  GC.Collect();
### 小结
 //构造函数
 //实例化对象时 调用的函数
 //主要是用来初始化成员变量的

 //基本语法
 //不写返回值
 //函数名和类名相同
 //访问修饰符根据需求而定
 //一般为public

 //注意
 //可以重载构造函数
 //可以用this语法重用代码
 //可以在函数中用this区分同名参数和成员变量
 //有参构造会顶掉默认的无参构造

 //析构函数
 //当对象呗垃圾回收时 调用的，主要是用来回收资源或者特殊处理内存的

 //基本语法
 //不写返回值
 //不写修饰符
 //不能有参数
 //函数名和类名相同
 //前面加~
## 成员属性
- 成员属性的概念
  //基本概念
  //1.用于保护成员变量
  //2.为成员属性的获取和赋值添加逻辑处理
  //3.解决3P的局限性
  //  public——内外访问
  //  private——内部访问
  //  protected——内部和子类访问
  //  属性可以让成员变量在外部
  //  只能获取 不能修改  或者  只能修改 不能获取
```
成员变量的语法与示例
  class Person
  {
      private string name;
      private int age;
      private int money;
      private bool sex;

      //属性的命名一般使用 帕斯卡命名法
      public string Name
      {
          get
          {
              //可以在返回之前添加一些逻辑规则 
              //意味着 这个属性可以获取的内容
              return name;
          }
          set
          {
              //可以在设置之前添加一些逻辑规则 
              // value 关键字 用于表示 外部传入的值
              name = value;
          }
      }
      #region 知识点四 成员属性中 get和set前可以加访问修饰符
      // 注意
      // 1.默认不加 会使用属性申明时的访问权限
      // 2.加的访问修饰符要低于属性的访问权限
      // 3.不能让get和set的访问权限都低于属性的权限
      #endregion
      public int Money
      {
          get
          {
              //解密处理
              return money - 5;
          }
          set
          {
              //加密处理
              money = value + 5;
          }
      }

      #region 知识点五 get和set可以只有一个
      //注意：
      //只有一个时  没必要在前面加访问修饰符
      //一般情况下 只会出现 只有 get的情况 基本不会出现只有set
      public bool Sex
      {
          get
          {
              return sex;
          }
          //set
          //{
          //    sex = value;
          //}
      }
      #endregion

      #region 知识点六 自动属性
      //作用：外部能得不能改的特征
      //如果类中有一个特征是只希望外部能得不能改的 又没什么特殊处理
      //那么可以直接使用自动属性
      public float Height
      {
          //没有再get和set中写逻辑的需求或者想法
          get;
          private set;
      }
      #endregion

  }
  #endregion

  class Program
  {
      static void Main(string[] args)
      {
          Console.WriteLine("成员属性");
          #region 知识点三 成员属性的使用
          Person p = new Person();
          p.Name = "唐老狮";
          Console.WriteLine(p.Name);

          p.Money = 1000;
          Console.WriteLine(p.Money);

          //Console.WriteLine(p.Sex);
          //p.Sex = true;
          #endregion
      }
  }
```
- 小结
 //1.成员属性概念：一般是用来保护成员变量的
 //2.成员属性的使用和变量一样 外部用对象点出
 //3.get中需要return内容 ； set中用value表示传入的内容
 //4.get和set语句块中可以加逻辑处理
 //5.get和set可以加访问修饰符，但是要按照一定的规则进行添加
 //6.get和set可以只有一个
 //7.自动属性是属性语句块中只有get和set，一般用于 外部能得不能改这种情况

## 索引器
- 基本概念
 //基本概念
 //让对象可以像数组一样通过索引访问其中元素，使程序看起来更直观，更容易编写
- 语法，内部逻辑，重构，使用
```
索引器语法
    //访问修饰符 返回值 this[参数类型 参数名, 参数类型 参数名.....]
    //{
    //      内部的写法和规则和索引器相同
    //      get{}
    //      set{}
    //}
    class Person
    {
        private string name;
        private int age;
        private Person[] friends;

        private int[,] array;

        #region 知识点五 索引器可以重载
        //重载的概念是——函数名相同 参数类型、数量、顺序不同
        public int this[int i, int j]
        {
            get
            {
                return array[i, j];
            }
            set
            {
                array[i, j] = value;
            }
        }

        public string this[string str]
        {
            get
            {
                switch (str)
                {
                    case "name":
                        return this.name;
                    case "age":
                        return age.ToString();
                }
                return "";
            }
        }
        #endregion

        public Person this[int index]
        {
            
            get
            {
                //可以写逻辑的 根据需求来处理这里面的内容
                #region 知识点四 索引器中可以写逻辑
                if( friends == null ||
                    friends.Length - 1 < index)
                {
                    return null;
                }
                #endregion
                return friends[index];
            }
            set
            {
                //value代表传入的值
                //可以写逻辑的 根据需求来处理这里面的内容
                if( friends == null )
                {
                    friends = new Person[] { value };
                }
                else if(index > friends.Length - 1)
                {
                    //自己定了一个规则 如果索引越界 就默认把最后一个朋友顶掉
                    friends[friends.Length - 1] = value;
                }
                friends[index] = value;
            }
        }

    }
     class Program
 {
     static void Main(string[] args)
     {
         Console.WriteLine("索引器");
         #region 知识点三 索引器的使用
         Person p = new Person();
         p[0] = new Person();
         Console.WriteLine(p[0]);

         p[0, 0] = 10;
         #endregion
     }
 }
```
- 总结
 //索引器对于我们来说的主要作用
 //可以让我们以中括号的形式范围自定义类中的元素 规则自己定 访问时和数组一样
 //比较适用于 在类中有数组变量时使用 可以方便的访问和进行逻辑处理

 //固定写法
 //访问修饰符 返回值 this[参数列表]
 //get和set语句块
 //可以重载

 //注意：结构体里面也是支持索引器

## 静态成员
- 基本概念
  //静态关键字 static
  //用static修饰的 成员变量、方法、属性等
  //称为静态成员

  //静态成员的特点是：直接用类名点出使用
  - 自定义成员变量
```
    class Test
  {
      public const float G = 9.8f;

      //静态成员变量
      static public float PI = 3.1415926f;
      //成员变量
      public int testInt = 100;

      //静态成员方法
      public static float CalcCircle(float r)
      {
          #region 知识点六 静态函数中不能使用非静态成员
          //成员变量只能将对象实例化出来后 才能点出来使用 不能无中生有
          //不能直接使用 非静态成员 否则会报错
          //Console.WriteLine(testInt);
          Test t = new Test();
          Console.WriteLine(t.testInt);
          #endregion
          //πr²
          return PI * r * r;
      }
      //成员方法
      public void TestFun()
      {
          Console.WriteLine("123");
          #region 知识点七 非静态函数可以使用静态成员
          Console.WriteLine(PI);
          Console.WriteLine(CalcCircle(2));
          #endregion
      }
  }
  #endregion
```
- 静态成员的使用
```
 class Program
 {
     static void Main(string[] args)
     {
         Console.WriteLine("静态成员");
         #region 知识点四 静态成员的使用
         Console.WriteLine(Test.PI);
         Console.WriteLine(Test.G);
         Console.WriteLine(Test.CalcCircle(2));

         Test t = new Test();
         Console.WriteLine(t.testInt);
         t.TestFun();
         #endregion
     }
 }
```
- 为什么可以直接点出来使用
//记住！
//程序中是不能无中生有的
//我们要使用的对象，变量，函数都是要在内存中分配内存空间的
//之所以要实例化对象，目的就是分配内存空间，在程序中产生一个抽象的对象

//静态成员的特点
//程序开始运行时 就会分配内存空间。所以我们就能直接使用。
//静态成员和程序同生共死
//只要使用了它，直到程序结束时内存空间才会被释放
//所以一个静态成员就会有自己唯一的一个“内存小房间”
//这让静态成员就有了唯一性
//在任何地方使用都是用的小房间里的内容，改变了它也是改变小房间里的内容。
- 静态成员的使用
 //静态变量：
 //1.常用唯一变量的申明 
 //2.方便别人获取的对象申明
 //静态方法：
 //常用的唯一的方法申明 比如 相同规则的数学计算相关函数
 - 常量与静态成员
 //const（常量）可以理解为特殊的static（静态）
//相同点
//他们都可以通过类名点出使用
//不同点
//1.const必须初始化，不能修改 static没有这个规则
//2.const只能修饰变量、static可以修饰很多
//3.const一定是写在访问修饰符后面的 ，static没有这个要求
- 总结
 //总结
 //概念：用static修饰的成员变量、成员方法、成员属性等 就称为静态成员
 //特点：直接用类名点出来使用(全局性)
 //生命周期：和程序同生共死
 //         程序运行后就会一直存在内存中，知道程序结束后才会释放，因此静态成员具有唯一性
 //注意：
 //1.静态函数中不能直接使用非静态成员
 //2.非静态函数中可以直接使用静态成员

 //常量和静态变量
 //常量时特殊的静态变量
 //相同点
 //他们都可以通过类名点出来使用
 //不同点
 //1.const必须初始化不能被修改 static没有这个规则
 //2.const只能修饰变量，static可以修饰很多
 //3.const不能写在访问修饰符前面 一定是写在变量申明前面 static没有这个规则
## 静态类和静态构造函数
- 静态类的定义，概念与作用
 //概念
 //用static修饰的类

 //特点
 //只能包含静态成员
 //不能被实例化

 //作用
 //1.将常用的静态成员写在静态类中 方便使用
 //2.静态类不能被实例化，更能体现工具类的 唯一性
 //比如 Console就是一个静态类
```
  static class Tools
 {
     //静态成员变量
     public static int testIndex = 0;

     public static void TestFun()
     {

     }

     public static int TestIndex
     {
         get;
         set;
     }
 }
```
- 静态构造函数的概念，定义与作用
//概念
// 在构造函数加上 static 修饰

//特点
//1.静态类和普通类都可以有
//2.不能使用访问修饰符
//3.不能有参数  
//4.只会自动调用一次  

//作用
//在静态构造函数中初始化 静态变量

```
 //使用
 //1.静态类中的静态构造函数
 static class StaticClass
 {
     public static int testInt = 100;
     public static int testInt2 = 100;

     static StaticClass()
     {
         Console.WriteLine("静态构造函数");
         testInt = 200;
         testInt2 = 300;
     }
 }

 //2.普通类中的静态构造函数
 class Test
 {
     public static int testInt = 200;
     static Test()
     {
         Console.WriteLine("静态构造");
     }

     public Test()
     {
         Console.WriteLine("普通构造");
     }
 }
 #endregion
 class Program
 {
     static void Main(string[] args)
     {
         Console.WriteLine("静态类和静态构造函数!");

         Console.WriteLine(StaticClass.testInt);
         Console.WriteLine(StaticClass.testInt2);
         Console.WriteLine(StaticClass.testInt);

         Console.WriteLine(Test.testInt);
         Test t = new Test();
         Test t2 = new Test();
     }
 }
```
- 总结
//总结
//静态类
//用static 修饰的类
//特点
//只能包含静态成员
//不能实例化
//作用
//工具类
//拓展方法

//静态构造函数
//用static修饰的构造函数
//特点
//静态类和普通类都可以有静态构造函数
//不能使用访问修饰符
//不能有参数
//只会自动调用一次
//作用
//初始化静态成员
## 拓展方法
- 基本概念
 //概念 
 //为现有非静态 变量类型 添加 新方法
 //作用
 //1.提升程序拓展性
 //2.不需要再对象中重新写方法
 //3.不需要继承来添加方法
 //4.为别人封装的类型写额外的方法
 //特点
 //1.一定是写在静态类中
 //2.一定是个静态函数
 //3.第一个参数为拓展目标
 //4.第一个参数用this修饰
 - 基本语法与实例
    //访问修饰符 static 返回值 函数名(this 拓展类名 参数名, 参数类型 参数名,参数类型 参数名....)
```
      static class Tools
  {
      //为int拓展了一个成员方法
      //成员方法 是需要 实例化对象后 才 能使用的
      //value 代表 使用该方法的 实例化对象
      public static void SpeakValue(this int value)
      {
          //拓展的方法 的逻辑
          Console.WriteLine("唐老狮为int拓展的方法" + value);
      }

      public static void SpeakStringInfo(this string str, string str2, string str3)
      {
          Console.WriteLine("唐老狮为string拓展的方法");
          Console.WriteLine("调用方法的对象" + str);
          Console.WriteLine("传的参数" + str2 + str3);
      }

      public static void Fun3(this Test t)
      {
          Console.WriteLine("为test拓展的方法");
      }

  }
```
- 为自定义的类型拓展方法
```
 class Test
 {
     public int i = 10;
     public void Fun1()
     {
         Console.WriteLine("123");
     }

     public void Fun2()
     {
         Console.WriteLine("456");
     }
 }
```
使用：   class Program
   {
       static void Main(string[] args)
       {
           Console.WriteLine("拓展方法");
           #region 知识点四 使用
           int i = 10;
           i.SpeakValue();

           string str = "000";
           str.SpeakStringInfo("唐老狮", "111");

           Test t = new Test();
           t.Fun2();
           #endregion
       }
   }
   
-  总结
 //概念：为现有的非静态 变量类型 添加 方法
 //作用：
 // 提升程序拓展性
 // 不需要再在对象中重新写方法
 // 不需要继承来添加方法
 // 为别人封装的类型写额外的方法

 //特点：
 //静态类中的静态方法
 //第一个参数 代表拓展的目标
 //第一个参数前面一定要加 this

 //注意：
 //可以有返回值 和 n个参数
 //根据需求而定

## 运算符重载
- 基本概念
 //概念
 //让自定义类和结构体
 //能够使用运算符

 //使用关键字 
 //operator

 //特点
 //1.一定是一个公共的静态方法
 //2.返回值写在operator前
 //3.逻辑处理自定义

 //作用
 //让自定义类和结构体对象可以进行运算
 //注意
 //1.条件运算符需要成对实现
 //2.一个符号可以多个重载
 //3.不能使用ref和out
- 基本语法
 //public static 返回类型 operator 运算符(参数列表)
- 实例与使用
   class Point
   {
       public int x;
       public int y;

       public static Point operator +(Point p1, Point p2)
       {
           Point p = new Point();
           p.x = p1.x + p2.x;
           p.y = p1.y + p2.y;
           return p;
       }

       public static Point operator +(Point p1, int value)
       {
           Point p = new Point();
           p.x = p1.x + value;
           p.y = p1.y + value;
           return p;
       }

       public static Point operator +(int value, Point p1)
       {
           Point p = new Point();
           p.x = p1.x + value;
           p.y = p1.y + value;
           return p;
       }
    }
     class Program
 {
     static void Main(string[] args)
     {
         Console.WriteLine("运算符重载");


         Point p = new Point();
         p.x = 1;
         p.y = 1;
         Point p2 = new Point();
         p2.x = 2;
         p2.y = 2;

         Point p3 = p + p2;

         Point p4 = p3 + 2;
         Point p5 = 2 + p3;
         
     }
 }
- 可重载的与不可重载的运算符
可重载
```
 #region 可重载的运算符

 #region 算数运算符
 //注意 符号需要两个参数还是一个参数
 public static Point operator -(Point p1, Point P2)
 {
     return null;
 }
 public static Point operator *(Point p1, Point P2)
 {
     return null;
 }
 public static Point operator /(Point p1, Point P2)
 {
     return null;
 }
 public static Point operator %(Point p1, Point P2)
 {
     return null;
 }

 public static Point operator ++(Point p1)
 {
     return null;
 }
 public static Point operator --(Point p1)
 {
     return null;
 }
 #endregion

 #region 逻辑运算符
 //注意 符号需要两个参数还是一个参数
 public static bool operator !(Point p1)
 {
     return false;
 }
 #endregion

 #region 位运算符
 //注意 符号需要两个参数还是一个参数
 public static Point operator |(Point p1, Point p2)
 {
     return null;
 }
 public static Point operator &(Point p1, Point p2)
 {
     return null;
 }
 public static Point operator ^(Point p1, Point p2)
 {
     return null;
 }
 public static Point operator ~(Point p1)
 {
     return null;
 }
 public static Point operator <<(Point p1, int num)
 {
     return null;
 }
 public static Point operator >>(Point p1, int num)
 {
     return null;
 }
 #endregion

 #region 条件运算符
 //1.返回值一般是bool值 也可以是其它的
 //2.相关符号必须配对实现
 public static bool operator >(Point p1, Point p2)
 {
     return false;
 }
 public static bool operator <(Point p1, Point p2)
 {
     return false;
 }
 public static bool operator >=(Point p1, Point p2)
 {
     return false;
 }
 public static bool operator <=(Point p1, Point p2)
 {
     return false;
 }
 public static bool operator ==(Point p1, Point p2)
 {
     return false;
 }
 public static bool operator !=(Point p1, Point p2)
 {
     return false;
 }
 public static bool operator true(Point p1)
 {
     return false;
 }
 public static bool operator false(Point p1)
 {
     return false;
 }
 #endregion
 #endregion
```
不可重载
```
  #region 不可重载的运算符
  //逻辑与(&&) 逻辑或(||)
  //索引符 []
  //强转运算符 ()
  //特殊运算符 
  //点.   三目运算符? :  赋值符号=

  #endregion
```
## 内部类和分部类
-  内部类
 //概念
 //在一个类中再申明一个类

 //特点
 //使用时要用包裹者点出自己

 //作用
 //亲密关系的变现

 //注意
 //访问修饰符作用很大
```
class Person
    {
        public int age;
        public string name;
        public Body body;
        public class Body
        {
            Arm leftArm;
            Arm rightArm;
            class Arm
            {

            }
        }
    }
```
- 分部类
  //概念
  //把一个类分成几部分申明
  //关键字
  //partial
  //作用
  //分部描述一个类
  //增加程序的拓展性
  //注意
  //分部类可以写在多个脚本文件中
  //分部类的访问修饰符要一致
  //分部类中不能有重复成员
```
  partial class Student
 {
     public bool sex;
     public string name;

     partial void Speak();
 }

 partial class Student
 {
     public int number;

     partial void Speak()
     {
         //实现逻辑
     }

     public void Speak(string str)
     {

     }
 }
```
- 分部方法
 //概念
 //将方法的申明和实现分离
 //特点
 //1.不能加访问修饰符 默认私有
 //2.只能在分部类中申明
 //3.返回值只能是void
 //4.可以有参数但不用 out关键字
 //局限性大，了解即可
```
  class Program
 {
     static void Main(string[] args)
     {
         Console.WriteLine("内部类和分部类");

         Person p = new Person();

         Person.Body body = new Person.Body();
     }
 }
```
## 继承的基本概念
- 基本概念
 //一个类A继承一个类B
 //类A将会继承类B的所有成员
 //A类将拥有B类的所有特征和行为

 //被继承的类
 //称为 父类、基类、超类

 //继承的类
 //称为子类、派生类

 //子类可以有自己的特征和行为    

 //特点
 //1.单根性 子类只能有一个父类
 //2.传递性 子类可以间接继承父类的父类  
- 基本语法
  //class 类名 : 被继承的类名
  //{

  //}
- 实例
    class Test
    {

    }

    class Teacher
    {
        //姓名
        public string name;
        //职工号
        protected int number;
        //介绍名字
        public void SpeakName()
        {
            number = 10;
            Console.WriteLine(name);
        }
    }

    class TeachingTeacher : Teacher
    {
        //科目
        public string subject;
        //介绍科目
        public void SpeakSubject()
        {
            number = 11;
            Console.WriteLine(subject + "老师");
        }
    }

    class ChineseTeacher:TeachingTeacher
    {
        public void Skill()
        {
            Console.WriteLine("一行白鹭上青天");
        }
    }
- 访问修饰符的影响
 //public - 公共 内外部访问
 //private - 私有 内部访问
 //protected - 保护 内部和子类访问

 //之后讲命名空间的时候讲
 //internal - 内部的 只有在同一个程序集的文件中，内部类型或者是成员才可以访问
- 子类和父类的同名成员
 //概念 
 //C#中允许子类存在和父类同名的成员
 //但是 极不建议使用
```
 class Program
 {
     static void Main(string[] args)
     {
         Console.WriteLine("继承的基本规则");

         TeachingTeacher tt = new TeachingTeacher();
         tt.name = "唐老狮";
         //tt.number = 1;
         tt.SpeakName();

         tt.subject = "Unity";
         tt.SpeakSubject();

         ChineseTeacher ct = new ChineseTeacher();
         ct.name = "唐老师";
         //ct.number = 2;
         ct.subject = "语文";
         ct.SpeakName();
         ct.SpeakSubject();
         ct.Skill();
     }
 }
```
- 总结
 //总结
 //继承基本语法
 // class 类名:父类名
 // 1.单根性：只能继承一个父类
 // 2.传递性：子类可以继承父类的父类。。。的所有内容
 // 3.访问修饰符 对于成员的影响

 // 4.极奇不建议使用 在子类中申明和父类同名的成员（以后学习了多态再来解决这个问题）
## 里氏替换原则
- 基本概念
  // 里氏替换原则是面向对象七大原则中最重要的原则
  // 概念：
  // 任何父类出现的地方，子类都可以替代
  // 重点：
  // 语法表现——父类容器装子类对象,因为子类对象包含了父类的所有内容
  // 作用：
  // 方便进行对象存储和管理
- 基本实现
```
 class GameObject
 {

 }

 class Player:GameObject
 {
     public void PlayerAtk()
     {
         Console.WriteLine("玩家攻击");
     }
 }

 class Monster:GameObject
 {
     public void MonsterAtk()
     {
         Console.WriteLine("怪物攻击");
     }
 }

 class Boss:GameObject
 {
     public void BossAtk()
     {
         Console.WriteLine("Boss攻击");
     }
 }
```
- is和as
 //基本概念 
 // is：判断一个对象是否是指定类对象
 // 返回值：bool  是为真 不是为假

 // as：将一个对象转换为指定类对象
 // 返回值：指定类型对象
 // 成功返回指定类型对象，失败返回null

 //基本语法
 // 类对象 is 类名   该语句块 会有一个bool返回值 true和false
 // 类对象 as 类名   该语句块 会有一个对象返回值 对象和null

 if( player is Player )
 {
     //Player p = player as Player;
     //p.PlayerAtk();
     (player as Player).PlayerAtk();
 }

 for (int i = 0; i < objects.Length; i++)
 {
     if( objects[i] is Player )
     {
         (objects[i] as Player).PlayerAtk();
     }
     else if( objects[i] is Monster )
     {
         (objects[i] as Monster).MonsterAtk();
     }
     else if (objects[i] is Boss)
     {
         (objects[i] as Boss).BossAtk();
     }
 }
 - 实现
   class Program
  {
      static void Main(string[] args)
      {
          Console.WriteLine("里氏替换原则");

          //里氏替换原则 用父类容器 装载子类对象
          GameObject player = new Player();
          GameObject monster = new Monster();
          GameObject boss = new Boss();

          GameObject[] objects = new GameObject[] { new Player(), new Monster(), new Boss() };
}
- 小结
  //总结
  //概念：父类容器装子类对象
  //作用：方便进行对象的存储和管理
  //使用：is和as
  // is 用于判断
  // as 用于转换
  // 注意：不能用子类容器装父类对象
## 继承中的构造函数
- 基本概念
 //特点
 //当申明一个子类对象时
 //先执行父类的构造函数
 //再执行子类的构造函数

 //注意：
 //1.父类的无参构造 很重要
 //2.子类可以通过base关键字 代表父类 调用父类构造
- 继承中构造函数的执行顺序
  // 父类的父类的构造——>。。。父类构造——>。。。——>子类构造
```
 class GameObject
 {
     public GameObject()
     {
         Console.WriteLine("GameObject的构造函数");
     }
 }
 class Player:GameObject
 {
     public Player()
     {
         Console.WriteLine("Player的构造函数");
     }
 }
 class MainPlayer : Player
 {
     public MainPlayer()
     {
         Console.WriteLine("MainPlayer的构造函数");
     }
 }
```
- 父类的无参构造函重要
 //子类实例化时 默认自动调用的 是父类的无参构造 所以如果父类无参构造被顶掉 会报错
```
class Father
{
    //public Father()
    //{

    //}

    public Father(int i)
    {
        Console.WriteLine("Father构造");
    }
}
```
```
 class Son:Father
  {
      #region 知识点四 通过base调用指定父类构造
      public Son(int i) : base(i)
      {
          Console.WriteLine("Son的一个参数的构造");
      }

      public Son(int i, string str):this(i)
      {
          Console.WriteLine("Son的两个参数的构造");
      }
      #endregion
  }
```
```
实施： class Program
 {
     static void Main(string[] args)
     {
         Console.WriteLine("继承中的构造函数");

         MainPlayer mp = new MainPlayer();

         Son s = new Son(1,"123");
     }
 }
```
- 总结
 //总结
 //继承中的构造函数
 //特点
 //执行顺序 是先执行父类的构造函数 再执行子类的 从老祖宗开始 依次一代一代向下执行

 //父类中的无参构造函数 很重要
 //如果被顶掉 子类中就无法默认调用无参构造了
 //解决方法：
 //1.始终保持申明一个无参构造
 //2.通过base关键字 调用指定父类的构造
 //注意：
 //区分this和base的区别

## 万物之父与装箱拆箱
- 基本概念
 //万物之父
 //关键字：object
 //概念：
 //object是所有类型的基类，它是一个类（引用类型）
 //作用：
 //1.可以利用里氏替换原则，用object容器装所有对象
 //2.可以用来表示不确定类型，作为函数参数类型
- 运用与装箱拆箱
```
class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("万物之父和装箱拆箱");
        #region 知识点二 万物之父的使用
        Father f = new Son();
        if( f is Son )
        {
            (f as Son).Speak();
        }

        //引用类型 
        object o = new Son();
        //用is as 来判断和转换即可
        if( o is Son )
        {
            (o as Son).Speak();
        }
        //值类型 
        object o2 = 1f;
        //用强转
        float fl = (float)o2;

        //特殊的string类型
        object str = "123123";
        string str2 = str as string;

        object arr = new int[10];
        int[] ar = arr as int[];
        #endregion

        #region 知识点三 装箱拆箱
        //发生条件
        //用object存值类型（装箱）
        //再把object转为值类型（拆箱）

        //装箱
        //把值类型用引用类型存储
        //栈内存会迁移到堆内存中

        //拆箱
        //把引用类型存储的值类型取出来
        //堆内存会迁移到栈内存中

        //好处：不确定类型时可以方便参数的存储和传递
        //坏处：存在内存迁移，增加性能消耗

        //装箱
        object v = 3;
        //拆箱
        int intValue = (int)v;

        TestFun(1, 2, 3, 4f, 34.5, "123123", new Son());
        #endregion
    }

    static void TestFun( params object[] array )
    {

    }
}
```
- 总结
 //万物之父：object
 //基于里氏替换原则的 可以用object容器装载一切类型的变量
 //它是所有类型的基类

 // 装箱拆箱
 // 用object存值类型（装箱）
 // 把object里面存的值 转换出来(拆箱)
 // 好处
 // 不去定类型时可以用 方便参数存储和传递
 // 坏处
 // 存在内存的迁移 增加了性能消耗

 // 不是不用，尽量少用

## 密封类
- 基本概念
 //密封类 是使用 sealed密封关键字修饰的类
 //作用：让类无法再被继承
- 实例
```
  class Father
  {

  }

  sealed class Son:Father
  {

  }
```
- 作用
   //在面向对象程序的设计中，密封类的主要作用就是不允许最底层子类被继承
 //可以保证程序的规范性、安全性
 //目前对于大家来说 可能用处不大
 //随着大家的成长，以后制作复杂系统或者程序框架时 便能慢慢体会到密封的作用
- 总结
 // 关键字：sealed
 // 作用：让类无法再被继承
 // 意义： 加强面向对象程序设计的 规范性、结构性、安全性

## 多态-vob
- 多态的概念
  // 多态按字面的意思就是“多种状态”
  // 让继承同一父类的子类们 在执行相同方法时有不同的表现（状态）
  // 主要目的
  // 同一父类的对象 执行相同行为（方法）有不同的表现
  // 解决的问题
  // 让同一个对象有唯一行为的特征
- 解决的问题
```
class Father
{
    public void SpeakName()
    {
        Console.WriteLine("Father的方法");
    }
}

class Son:Father
{
    public new void SpeakName()
    {
        Console.WriteLine("Son的方法");
    }
}
```
- 多态的实现
 //我们目前已经学过的多态
 //编译时多态——函数重载，开始就写好的
 //我们将学习的：
 //运行时多态( vob、抽象函数、接口 )
 //我们今天学习 vob
 //v: virtual(虚函数)
 //o: override(重写)
 //b: base(父类)
```
class GameObject
{
    public string name;
    public GameObject(string name)
    {
        this.name = name;
    }

    //虚函数 可以被子类重写
    public virtual void Atk()
    {
        Console.WriteLine("游戏对象进行攻击");
    }
}

class Player:GameObject
{
    public Player(string name):base(name)
    {

    }

    //重写虚函数
    public override void Atk()
    {
        //base的作用
        //代表父类 可以通过base来保留父类的行为
        base.Atk();
        Console.WriteLine("玩家对象进行攻击");
    }
}

class Monster:GameObject
{
    public Monster(string name):base(name)
    {

    }

    public override void Atk()
    {
        Console.WriteLine("怪物对象进行攻击");
    }
}

#endregion

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("多态vob");

        #region 解决的问题
        Father f = new Son();
        f.SpeakName();
        (f as Son).SpeakName();
        #endregion

        #region 多态的使用
        GameObject p = new Player("唐老狮");
        p.Atk();
        (p as Player).Atk();

        GameObject m = new Monster("小怪物");
        m.Atk();
        (m as Monster).Atk();
        #endregion
    }
}
```
- 总结
 //多态：让同一类型的对象，执行相同行为时有不同的表现
 //解决的问题： 让同一对象有唯一的行为特征
 //vob:
 // v:virtual 虚函数
 // o:override 重写
 // b:base 父类
 // v和o一定是结合使用的 来实现多态
 // b是否使用根据实际需求 保留父类行为

## 抽象类与抽象函数
- 抽象类