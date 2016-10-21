---
title: 最近做的一些Java入门题
comments: true
date: 2010-02-06 18:08:05
updated: 2010-02-06 18:08:05
tags:
  - java
  - 算法
categories:
  - program
  - java
  - 算法
---

## 题目一
> 给定等式，其中每个字母代表一个数字，且不同数字对应不同字母。编程求出这些数字并且打出这个数字的算术计算竖式。
> **定式**
> ```
>   A B C D E
>       D F G
> +     D F G
> ————————————
>   X Y Z D E
> ```

<!-- more -->


代码如下：

```java
import java.util.*; //Date类所需
public class text4
{
//此方法是用来判断A、B、C、D、E、F、G、X、Y、Z之间是否相等的，返回boolean值，相等就返回true，否则返回false
public static boolean b(int A,int B,int C,int D,int E,int F,int G,int X,int Y,int Z)
{
  int i[] = {A,B,C,D,E,F,G,X,Y,Z};
  for (int m = 0;m < 9;m++)
  {
   for (int n = m;n < 9;n++)
   {
    if (i[m] == i[n+1]) return true;
   }
  }
  return false;
}
//构造计算方法
public static void count()
{
  int A,B,C,D,E,F = 5,G = 0,X,Y,Z,DFG,XYZDE;  // 事先推算得F=5，G=0
  boolean ex;
  Date d1 = new Date();  // 仅作计时用，代码运行前的时间，可去掉
  for (int i = 11111;i <= 89999;i++)  // 因为A、B、C、D、E都不等于0，A不等于9，所以从for循环从11111开始，89999结束
  {
   for (int j = 1;j < 10;j++)
   {
    if (j == F) continue;// j其实就是D的值，D不等于5，所以可以节省一次循环次数
    DFG = j*100+F*10+G;// 给DFG赋值，下同，不作说明
    A = i/10000;
    B = (i/1000)%10;
    C = (i/100)%10;
    D = (i/10)%10;
    E = i%10;
    X = A+1;  // 从竖式中不难推算出X = A+1
    Y = (B + (C+2*D+1)/10)%10;  // 同上
    Z = (C+2*D+1)%10;  //同上
    ex = b(A,B,C,D,E,F,G,X,Y,Z);  // 调用b方法判断A,B,C,D,E,F,G,X,Y,Z相互之间是否相等
    if (ex) continue;  // 如果A,B,C,D,E,F,G,X,Y,Z当中有相等的，则继续循环
    XYZDE = X*10000+Y*1000+Z*100+D*10+E;
    if ((i+(DFG*2)) == XYZDE)
    {
     System.out.println("A= "+A+" B= "+B+" C= "+C+" D= "+D+" E= "+E+" F= "+F+" G= "+G+" X= "+X+" Y= "+Y+" Z= "+Z);
     System.out.println(" ");
     System.out.println(A+" "+B+" "+C+" "+D+" "+E);
     System.out.println("    "+D+" "+F+" "+G+" "+"     "+D+" "+F+" "+G+" ");
     System.out.println("---------");
     System.out.println(X+" "+Y+" "+Z+" "+D+" "+E);
    }
   }
  }
  Date d2 = new Date();  // 代码运行后的时间，可去掉
  System.out.println(d2.getTime()-d1.getTime());  // 计算代码运行后和运行前的时间差，得出代码运行时间，可去掉
}
public static void main(String args[])
{
  count();  // 调用count()方法
}
}
```

## 题目二
> A、B、C、D、E五名学生有可能参加计算机竞赛，根据下列条件判断哪些人参加了竞赛：
> 1. A参加时，B也参加；
> 2. B和C只有一个人参加；
> 3. C和D或者都参加，或者都不参加；
> 4. D和E中至少有一个人参加；
> 5. 如果E参加，那么A和D也都参加。

代码如下：

```java
import java.util.*;
public class text1
{
 public static void print(boolean A,boolean B,boolean C,boolean D,boolean E) //根据A、B、C、D、E的逻辑值输出结果
 {
  System.out.println("A:"+A);
  System.out.println("B:"+B);
  System.out.println("C:"+C);
  System.out.println("D:"+D);
  System.out.println("E:"+E);
 }

 public static void judge()
 {
  boolean A = true,B,C,D,E;//先假设A是参加的
  Date d1 = new Date();
  for (int i = 0;i < 2;i++)
  {
   A = !A;
   B = A;
   C = !B;
   D = C;
   if (D == false)
   {
    E = true;
    if (A == E && D == E) print(A,B,C,D,E);
   }
   else
   {
    E = true;
    if (A == E && D == E) print(A,B,C,D,E);
    else E = false;
    print(A,B,C,D,E);
   }
  }
  Date d2 = new Date();
  System.out.println(d2.getTime()-d1.getTime());
 }
 public static void main(String args[])
 {
  judge();
 }
}
```

## 题目三
> 打印一个 N*N 的方阵，N为每边字符的个数（3`<`N`<`20）, 要求最外一层为"T", 第二层为"J", 从第三层起每层依次打印数字 1,2,3,...
> （下图以N为15为例）
```
   TTTTTTTTTTTTTTT
   TJJJJJJJJJJJJJT
   TJ11111111111JT
   TJ12222222221JT
   TJ12333333321JT
   TJ12344444321JT
   TJ12345554321JT
   TJ12345654321JT
   TJ12345554321JT
   TJ12344444321JT
   TJ12333333321JT
   TJ12222222221JT
   TJ11111111111JT
   TJJJJJJJJJJJJJT
   TTTTTTTTTTTTTTT
```

代码如下：

//分析思路：把数组拆分来看，先赋值左上角部分，再将左上角部分水平翻转赋值给右上角部分，再将上半部分垂直翻转赋值给下半部分

```java
import java.util.*;
public class text3
{

 public static int inputN() //从键盘录入N值，并判断如果N不是整数或者N不是>3和<20的，打印错误信息
 {
  int N;
  while (true)
  {
   Scanner sc = new Scanner(System.in);
   if (!sc.hasNextInt())
   {
    System.out.println("Input Error!");
    continue;
   }
   N = sc.nextInt();
   if (N > 19 || N < 4)
   {
    System.out.println("Input Error!");
    continue;
   }
   break;
  }
  return N;
 }

 public static void matrix()
 {
  int N = inputN();
  int l;
  String temp[] = {"T","J","1","2","3","4","5","6","7","8"};
  String M[][] = new String[N][N];
  l = (int) (N/2.0+0.5);//循环次数，奇数时为(N+1)/2，偶数时为N/2
  Date d1 = new Date();
  for (int i = 0 ;i < l;i++)//左上角部分的赋值
  {
   for (int j = i;j < l;j++)
   {
    M[i][j] = M[j][i] = temp[i];
   }
  }
  for (int i = 0;i < l;i++)//右上角部分的赋值（左上角的水平翻转）
  {
   for (int j = 0;j < l;j++)
   {
    M[i][N-1-j] = M[i][j];
   }
  }
  for (int i = 0;i < N;i++)//下半部分的赋值（上半部分的垂直翻转）
  {
   for (int j = 0;j < N;j++)
   {
    M[N-1-i][j] = M[i][j];
   }
  }
  for (int i = 0;i < M.length;i++)//打印整个数组
  {
   for (int j = 0;j < M.length;j++)
   {
    System.out.print(M[i][j]+" ");
   }
   System.out.println();
  }
  Date d2 = new Date();
  System.out.println(d2.getTime()-d1.getTime());
 }

 public static void main(String args[])
 {
  matrix();
 }
}
```

## 题目四
> 在N行N列的数阵中, 数K(1<=K<=N)在每行和每列中出现且仅出现一次，这样的数阵叫N阶拉丁方阵。例如下图就是一个五阶拉丁方阵。编一程序，从键盘输入N值后，打印出所有不同的N阶拉丁方阵，并统计个数。

```
  1  2  3  4  5
  2  3  4  5  1
  3  4  5  1  2
  4  5  1  2  3
  5  1  2  3  4
```
这题不会做。。。。

## 题目五
> 输入一个十进数，将其转换成 N 进制数（0<N<=16)。

代码如下：

```java
//程序说明：正整数版本，不包括负数和小数
import java.util.*;
public class text7
{
 public static long inputN()//从键盘录入N，并判断N的范围和数据类型
 {
  long N;
  while (true)
  {
   Scanner sc = new Scanner(System.in);
   if (!sc.hasNextLong())
   {
    System.out.println("Input Error!");
    continue;
   }
   N = sc.nextLong();
   if (N < 2 || N > 16)
   {
    System.out.println("Input Error!");
    continue;
   }
   break;
  }
  return N;
 }

 public static long inputD()//从键盘录入一个十进制数，并判断数据类型
 {
  long D;
  while (true)
  {
   Scanner sc = new Scanner(System.in);
   if (!sc.hasNextLong())
   {
    System.out.println("Input Error!");
    continue;
   }
   D = sc.nextLong();
   break;
  }
  return D;
 }

 public static void converter()
 {
  System.out.print("请输入一个十进制数：");
  long D = inputD();
  System.out.print("请输入想转换的进制数N(0<N<=16)：");
  long N = inputN();
  char c[] = {'0','1','2','3','4','5','6','7','8','9','A','B','C','D','E','F'};
  ArrayList al = new ArrayList();
  if (D == 0) System.out.println("\n10进制数转为"+N+"进制数为：0");
  else System.out.print("\n10进制数转为"+N+"进制数为：");
  while (D != 0)
  {
   al.add(c[(int)(D%N)]);//N进制转换，D%N赋值给动态数组
   D /= N;
  }
  Object temp[] = new Object[al.size()];
  int k = al.size();
  for (int i = 0;i < k;i++)
  {
   temp[i] = al.get(k-i-1);//将动态数组逆序赋值，从而获得该十进制数的N进制数
   System.out.print(temp[i]);
  }
  System.out.println("\n");
 }
 public static void main(String args[])
 {
  converter();
 }
}
```

## 题目六
> 矩阵中填数. 当给出 N*N 的矩阵，要求用程序填入下列形式的数：
```
① 倒填，例如N=5      ② 蛇形填数           ③ 回转填数
┌──┬──┬──┬──┬──┐   ┌──┬──┬──┬──┬──┐   ┌──┬──┬──┬──┬──┐
│25│24│23│22│21│   │ 1│ 3│ 4│10│11│   │ 1│16│15│14│13│
├──┼──┼──┼──┼──┤   ├──┼──┼──┼──┼──┤   ├──┼──┼──┼──┼──┤
│20│19│18│17│16│   │ 2│ 5│ 9│12│19│   │ 2│17│24│23│12│
├──┼──┼──┼──┼──┤   ├──┼──┼──┼──┼──┤   ├──┼──┼──┼──┼──┤
│15│14│13│12│11│   │ 6│ 8│13│18│20│   │ 3│18│25│22│11│
├──┼──┼──┼──┼──┤   ├──┼──┼──┼──┼──┤   ├──┼──┼──┼──┼──┤
│10│ 9│ 8│ 7│ 6│   │ 7│14│17│21│24│   │ 4│19│20│21│10│
├──┼──┼──┼──┼──┤   ├──┼──┼──┼──┼──┤   ├──┼──┼──┼──┼──┤
│ 5│ 4│ 3│ 2│ 1│   │15│16│22│23│25│   │ 5│ 6│ 7│ 8│ 9│
└──┴──┴──┴──┴──┘   └──┴──┴──┴──┴──┘   └──┴──┴──┴──┴──┘
```

代码如下：

```java
import java.util.*;
public class text5
{

 public static int inputN() // 从键盘录入N
 {
  int N;
  while (true)
  {
   System.out.print("Please Input N:");
   Scanner sc = new Scanner(System.in);
   if (!sc.hasNextInt())
   {
    System.out.println("Input Error!");
    continue;
   }
   N = sc.nextInt();
   if (N <= 0)
   {
    System.out.println("Input Error!");
    continue;
   }
   break;
  }
  return N;
 }

 public static void print(int N,int sz[][]) // 构造打印数组的方法
 {
  for (int i = 0;i < N;i++)
  {
   for (int j = 0;j < N;j++)
   {
    System.out.print(sz[i][j]+"\t");
   }
   System.out.println();
  }
 }

 public static void dx() // 倒型填数
 {
  int N = inputN();
  int l = N*N;
  int dx[][] = new int[N][N];
  System.out.println("\n"+N+"阶倒型填数矩阵：");
  for (int i = 0 ;i < dx.length;i++)
  {
   for (int j = 0;j < dx.length;j++)
   {
    dx[i][j] = l;
    l--;
   }
  }
  print(N,dx);
 }

 public static void sx() // 蛇形填数
 {
  int N = inputN(),k = 0,m = 0,n = 0; //m为行，n为列
  int sx[][] = new int[N][N];
  System.out.println("\n"+N+"阶蛇型填数矩阵：");
  for (int i = 0;i < N;i++) //控制行数
  {
   for (int j = N-i;j <= N;j++) //控制每行的元素个数
   {
    k++;
    sx[m][n] = k;
    if (i%2 == 0 && i != 0 && j!= N) // 偶数行m+1,n-1，左下移
    {
     m++;
     n--;
    }
    if (i%2 != 0 && i != 0 && j!= N) // 奇数行m-1,n+1，右上移
    {
     m--;
     n++;
    }

    if (j == N && i%2 == 0) m++; //偶数行最后一个元素，下移
    if (j == N && i%2 != 0) n++; //奇数行最后一个元素，右移
   }
  }
  m = n = N-1; //把m,n值变为数组最后一个元素的下标，从后面填数
  k = N*N; //同上
  for (int i = 0;i < N-1;i++)// 翻转填数，原理同上
  {
   for (int j = N-i;j <= N;j++) //同上
   {
    sx[m][n] = k;
    k--;
    if (i%2 == 0 && i != 0 && j!= N) // 偶数行m-1,n+1，左下移
    {
     m--;
     n++;
    }
    if (i%2 != 0 && i != 0 && j!= N) // 奇数行m+1,n-1，右上移
    {
     m++;
     n--;
    }

    if (j == N && i%2 == 0) m--;//偶数行最后一个元素，下移
    if (j == N && i%2 != 0) n--;//奇数行最后一个元素，右移
   }
  }
  print(N,sx);
 }


 public static void hx() // 回型填数
 {
  int N = inputN(),k = 1,m = 0,n = 0,counter = 0,l = 1; // m为行，n为列，k为赋值数，counter为除第一数列外其他数列的标记数，终值为2，l为数列的个数，变化范围：[1,2N-1]
  int hx[][] = new int[N][N];
  System.out.println("\n"+N+"阶回型填数矩阵：");
  for (int i = 1;i <= N;i++)
  {
   while (counter < 2) // 除第一数列外，其余数列均进行此循环体2次
   {
    for (int j = i;j <= N;j++)
    {
     hx[m][n] = k;
     k++;
     if (l%4 == 1) // 靠左数列，下移，到达最后一个元素后，右移
     {
      if (j != N) m++;
      else n++;
     }
     if (l%4 == 2) // 靠下数列，右移，到达最后一个元素后，上移
     {
      if (j != N) n++;
      else m--;
     }
     if (l%4 == 3) // 靠右数列，上移，到达最后一个元素后，左移
     {
      if (j != N) m--;
      else n--;
     }
     if (l%4 == 0) // 靠上数列，左移，到达最后一个元素后，下移
     {
      if (j != N) n--;
      else m++;
     }
     if (j == N)
     {
      if (i != 1) counter++;
      l++;
     }
    }
    if (i == 1) counter = 2;
   }
   counter = 0;
  }
  print(N,hx);
 }
 public static void main(String args[])
 {
  dx();
  sx();
  hx();
 }
}
```

## 题目七
> 读入一行文本，包含若干个单词（以空格间隔，％结尾）。将其中以`A`开头的单词与以`N`结尾的单词，用头尾交换的办法予以置换。

代码如下：

```java
import java.util.*;
public class text9
{
 public static String input()[]//该方法存储录入的字符串，并存入str[]<String>数组里
 {
  String In;
  int j = 0;
  Scanner sc = new Scanner(System.in);
  ArrayList<String> al = new ArrayList<String>();
  while (true)
  {
   al.add(sc.nextLine());//将录入的每一行字符串存入动态数组里
   In = al.get(j);//将每一行的字符串赋值给In
   j++;
   if (In.contains("%")) break;//判断In是否包含“%”，如果是，退出while循环
  }
  sc.close();//关闭Scanner流
  String str[] = new String[al.size()];
  for (int i = 0;i < al.size();i++)
  {
   str[i] = al.get(i);//将动态数组赋值给str数组
  }
  return str;
 }

 public static void deal()
 {
  String In[] = input();
  String str[] = new String[1];
  ArrayList<String> al = new ArrayList<String> ();
  for (int i = 0;i < In.length;i++)
  {
   str = In[i].split(" ");//将数组里每一个数据（相当于输入的每一行）分割，分隔符为“ ”(空格)【相当于为了获得每一句的单词】
   for (int j = 0;j < str.length;j++)
   {
    al.add(str[j]);
    if (str[j].contains("%"))//如果单词包含有“%”字符，则只获取“%”前（不包括“%”）的子字符串
    {
     int l = str[j].indexOf("%");
     al.set(al.size()-1,str[j].substring(0,l));
     break;
    }
   }
  }
  al.trimToSize();
  for (int i = 0;i < al.size();i++)
  {
   al.remove("");//去掉动态数组中的空字符串
  }
  String S[] = new String[al.size()];
  char temp;
  for (int i = 0;i < S.length;i++)
  {
   S[i] = al.get(i);
   if (S[i].startsWith("A"))//以“A”开头的，首尾交换
   {
    temp = S[i].charAt(S[i].length()-1);
    S[i] = String.valueOf(temp).concat(S[i].substring(1,S[i].length()-1)).concat("A");
   }
   else if (S[i].endsWith("N"))//以“N”结尾的，首尾交换
   {
    temp = S[i].charAt(0);
    S[i] = "N".concat(S[i].substring(1,S[i].length()-1)).concat(String.valueOf(temp));
   }
   System.out.println(S[i]);
  }
 }

 public static void main(String args[])
 {
  deal();
 }
}
```

## 题目八
> 输入两个正整数X,Y，将X,Y化为二进制数，然后将这两个二进制数作二进制加法运算，再将结果化为十进制数输出。

代码如下：

```java
import java.util.*;
public class text8
{
 public static long input(char i)//构造输入方法，从键盘录入一个非负的long值
 {
  long N;
  while (true)
  {
   System.out.print("Please Input "+i+":");
   Scanner sc = new Scanner(System.in);
   if (!sc.hasNextLong())
   {
    System.out.println("Input Error!");
    continue;
   }
   N = sc.nextLong();
   if (N < 0)
   {
    System.out.println("Input Error!");
    continue;
   }
   break;
  }
  return N;
 }

 public static void count()
 {
  long X = input('X');//X
  long Y = input('Y');//Y
  long Bin_X = Long.parseLong(Long.toBinaryString(X));//将X转换为二进制
  long Bin_Y = Long.parseLong(Long.toBinaryString(Y));//将Y转换为二进制
  long Bin_XY = Bin_X + Bin_Y;//将二进制的X和Y相加
  ArrayList<Long> al = new ArrayList<Long> ();
  long T = Bin_XY % 10;//二进制(X+Y)相加后的个位
  while (Bin_XY != 0)
  {
   if (T == 2)//如果为2，则该位为0，且进位+1
   {
    al.add((long)0);
    Bin_XY /= 10;
    T = Bin_XY % 10 + 1;
    continue;
   }
   if (T == 3)//如果为3，则该位为1，且进位+1
   {
    al.add((long)1);
    Bin_XY /= 10;
    T = Bin_XY % 10 + 1;
    continue;
   }
   else
   {
    al.add(T);//如果为0或1，则该位为原值，进位无变化
    Bin_XY /= 10;
    T = Bin_XY % 10;
    continue;
   }
  }
  if (T == 2) al.add((long)0);//因D=0，而最高位未处理，所以再执行多一次，下同
  if (T == 3) al.add((long)1);
  else al.add(T);
  long Bin_Result = 0;
  for (int i = 0;i < al.size();i++)
  {
   Bin_Result += al.get(al.size()-1-i) * (long)Math.pow(10,al.size()-1-i);//将转换后的结果倒序*10^N再相加，得到转换后的二进制之和
  }
  System.out.println("Binary: "+Bin_X+"+"+Bin_Y+"="+Bin_Result);
  System.out.println("Decimal: "+X+"+"+Y+"="+Long.parseLong(Long.toString(Bin_Result),2));
 }

 public static void main(String args[])
 {
  count();
 }
}
```

## 题目九
> 四人玩火柴棍游戏，每一次都是三个人赢，一个人输。输的人要按赢者手中的火柴数进行赔偿，即赢者手中有多少根火柴棍，输者就赔偿多少根。现知道玩过四次后，每人恰好输过一次，而且每人手中都正好有16根火柴。问此四人做游戏前手中各有多少根火柴? 编程解决此问题。

代码如下：

```java
/**分析结果
       A   B  C  D
原来  33  17  9  5
ALose  2  34 18 10
BLose  4   4 36 20
CLose  8   8  8 40
DLose 16  16 16 16
逆向反推，其中一个最小值=其余所有值/2相加和+自身值；
正推，每轮最大值为输家，除最大值外，其余所有值*2，最大值-其余所有值*2；
*/

public class text10
{

 public static int findMin(int a[]) // 返回数组内最小值的下标，如果多个值相等并同为最小值，则返回最后一个最小值的下标
 {
  int min_ = 0,min = a[0];
  for (int i = 1;i < a.length;i++)
  {
   if (a[i] <= min)
   {
    min_ = i;
    min = a[i];
   }
  }
  return min_;
 }

 public static void count()
 {
  int R[] = new int[4];
  int l = 0;
  for (int i = 0;i < 4;i++)
   R[i] = 16;
  for (int i = 0;i < 4;i++) //共进行4轮游戏
  {
   l = findMin(R); //每轮的最后一个最小值
   for (int j = 0;j < 4;j++)
   {
    if (j != l) //数组内除最小值外的其他元素
    {
     R[j] /= 2;
     R[l] += R[j];
    }
   }
  }
  for (int i = 0;i < 4;i++)
  {
   System.out.print(R[i]);
  }
 }

 public static void main(String args[])
 {
  count();
 }
}
```

## 题目十
> 如图所示，编写程序计算大大小小正方形共有多少？当最小正方行边长为1时，它们的总面积共为多少？
> 10*10
```
  ┎┰┰┰┰┰┰┰┰┰┒
  ┠╂╂╂╂╂╂╂╂╂┨
  ┠╂╂╂╂╂╂╂╂╂┨
  ┠╂╂╂╂╂╂╂╂╂┨
  ┠╂╂╂╂╂╂╂╂╂┨
  ┠╂╂╂╂╂╂╂╂╂┨
  ┠╂╂╂╂╂╂╂╂╂┨
  ┠╂╂╂╂╂╂╂╂╂┨
  ┠╂╂╂╂╂╂╂╂╂┨
  ┠╂╂╂╂╂╂╂╂╂┨
  ┖┸┸┸┸┸┸┸┸┸┚
```

代码如下：

```java
/**

分析结果：

递归算法
个数：
1*1 = 1
2*2 = 4+1*1 = 5
3*3 = 9+2*2+1*1 = 14
4*4 = 16+3*3+2*2+1*1 = 30
…………
10*10 = 100+81+64+49+……+9+4+1 = 385

面积：
1*1 = 1*(1*1) = 1
2*2 = 4*(1*1)+1*(2*2) = 8
3*3 = 9*(1*1)+4*(2*2)+1*(3*3) = 34
4*4 = 16*(1*1)+9*(2*2)+4*(3*3)+1*(4*4) = 104
…………
10*10 = 100*1+81*4+64*9+49*16+36*25+25*36+16*49+9*64+4*81+1*100
*/

public class text11
{
 public static int count(int num)//正方形个数的计算方法
 {
  int sum;
  if (num == 1)
  {
   return 1;
  }
  else
  {
   sum = num*num + count(num-1);//sum = N*N+(N-1)*(N-1)+(N-2)*(N-2)+……+2*2+1*1
  }
  return sum;
 }

 public static int area(int num)//正方形总面积的计算方法
 {
  int sum = 0,i = 1;
  while (num != 1)
  {
   sum += num*num*i*i;//sum = N*N*1*1+(N-1)*(N-1)*2*2+……+2*2*(N-1)*(N-1)+1*1*N*N
   i++;
   num--;
  }
  return sum;
 }
 public static void main(String args[])
 {
  System.out.println("┎┰┰┰┰┰┰┰┰┰┒");
  for (int i = 0;i < 9;i++)
  {
   System.out.println("┠╂╂╂╂╂╂╂╂╂┨");
  }
  System.out.println("┖┸┸┸┸┸┸┸┸┸┚"+"\n"+"10*10"+"\n");
  System.out.println("正方形个数："+count(10));
  System.out.println("总面积："+area(10));
 }
}
```

## 题目十一
> 小明和小强都是张老师的学生，张老师的生日是M月N日，2人都知道张老师的生日是下列10组中的一天，张老师把M值告诉了小明，把N值告诉了小强，张老师问他们知道他的生日是那一天吗？
>　　　　3月4日 3月5日 3月8日
>　　　　6月4日 6月7日
>　　　　9月1日 9月5日
>　　　　12月1日 12月2日 12月8日
>　　　　小明说：如果我不知道的话，小强肯定也不知道
>　　　　小强说：本来我也不知道，但是现在我知道了
>　　　　小明说：哦，那我也知道了
>　　　　请根据以上对话推断出张老师的生日是哪一天

解题思路：由小明第一句话可知，此月可能为3月或9月。因为6月有个7日，12月有个2日，如果M为6或12的话，小强有可能知道，而小明肯定不知道。
由小强的话可知，此日可能为1日、4日或8日。因为如果N为5的话，小强无法判断是3月5日还是9月5日，而不会说现在我知道了。
由小明第二句话可知，此月必为9月。因为如果是3月，小明无法判断是3月4日还是3月8日，而他说那我也知道了。

代码如下：

```java
public class text6
{
 public static void birthday()
 {
  int month[] = {3,3,3,6,6,9,9,12,12,12};
  int day[] =   {4,5,8,4,7,1,5,1,2,8};
  int counter = 0,temp;
  for (int i = 0;i < day.length;i++)
  {
   counter = 0;
   for (int j = 0;j < day.length;j++)
   {
    if (day[i] == day[j] && i != j) counter++;
   }
   if (counter == 0)
   {
    day[i] = 0;
    temp = month[i];
    for (int k = 0;k < month.length;k++)
    {
     if (month[k] == temp) month[k] = 0;
    }
   }
  }
  for (int k = 0;k < day.length;k++)
  {
   if (month[k] == 0) day[k] = 0;
  }

  for (int i = 0;i < day.length;i++)
  {
   counter = 0;
   for (int j = 0;j < day.length;j++)
   {
    if (day[i] == day[j] && i != j)
    {
     day[i] = 0;
     day[j] = 0;
    }
   }
   for (int k = 0;k < month.length;k++)
   {
    if (day[k] == 0) month[k] = 0;
   }
  }

  for (int i = 0;i < month.length;i++)
  {
   counter = 0;
   for (int j = 0;j < month.length;j++)
   {
    if (month[i] == month[j] && i != j)
    {
     month[i] = 0;
     month[j] = 0;
    }
   }
   for (int k = 0;k < day.length;k++)
   {
    if (month[k] == 0) day[k] = 0;
   }
  }
  for (int i = 0;i < month.length;i++)
  {
   for (int j = 0;j < day.length;j++)
   {
    if (month[i] !=0 && day[i] != 0) System.out.println("Birthday: "+month[i]+"月"+day[i]+"日");
    break;
   }
  }
 }
 public static void main(String args[])
 {
  birthday();
 }
}
```

## 题目十二
> 双倍超立方数是指一个正整数可以正好被拆分为两种不同的a^3+b^3的方式，其中a,b均为整数且0<a<=b。对于任何一个指定的 int n, 返回所有的小于等于n的双倍超立方数的个数。

代码如下：

```java
import java.util.*;
public class text2
{
 public static int inputN()
 {
  int N;
  while (true)
  {
   Scanner sc = new Scanner(System.in);
   if (!sc.hasNextInt())
   {
    System.out.println("Input Error!");
    continue;
   }
   N = sc.nextInt();
   if (N <= 0)
   {
    System.out.println("Input Error!");
    continue;
   }
   break;
  }
  return N;
 }

 public static void count()
 {
  System.out.print("N:");
  int N = inputN(),counter = 0;
  for (int i = 1;i <= N;i++)
  {
   for (int b = 1;b <= N;b++)
   {
    for (int a = 1;a <= b;a++)
    {
     if (i == a*a*a+b*b*b)
     {
      System.out.print(i+"="+a+"^3+"+b+"^3"+"\n");
      counter++;
     }
    }
   }
  }
  System.out.println("\n双倍超立方数的个数: "+counter+"\n");
 }
 public static void count2()
 {
  System.out.print("N:");
  int N = inputN(),counter = 0;
  for (int b = 1;b <= N;b++)
  {
   for (int a = 1;a <= b;a++)
   {
    if (N == a*a*a+b*b*b)
    {
     System.out.print(N+"="+a+"^3+"+b+"^3"+"\n");
     counter++;
    }
   }
  }
  System.out.println("\n双倍超立方数"+N+"的个数: "+counter+"\n");
 }
 public static void main(String args[])
 {
//  count();
//  count2()
 }
}
```

```java
/**
另一种算法：
import java.util.Hashtable;
import java.util.Map;
import java.util.Set;
import java.util.Map.Entry;

public class text3 {
        private static Map<Integer, Integer> cache = new Hashtable<Integer, Integer>();// 键为立方和，值为次数

        public static int count(int n) {
                if (n < 1 || n > 1000000000)
                        return 0;
                int count = 0;
                int max = (int) java.lang.Math.pow(n, 1.0f / 3.0f);
                for (int a = 1; a <= max; a++) {
                        for (int b = a; b <= max; b++) {
                                int tmp = a * a * a + b * b * b;
                                if (cache.get(tmp) == null) {
                                        cache.put(tmp, 1);
                                } else {
                                        int num = cache.get(tmp);
                                        cache.put(tmp, num + 1);
                                }
                        }
                }

                Set<Entry<Integer, Integer>> entrys = cache.entrySet();

                for (Entry<Integer, Integer> entry : entrys) {
                        int key = entry.getKey();
                        int value = entry.getValue();
                        if (key <= n && value == 2)
                                ++count;
                }
                return count;
        }

        public static void main(String[] args) {
                long t1 = System.currentTimeMillis();
                int n = 1000000000;
                System.out.println(count(n));
                long t2 = System.currentTimeMillis();
                System.out.println(t2 - t1);

        }

}
*/
```
