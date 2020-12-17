# day4
## Summary
运算符及控制语句
## 运算符
### 算数运算符

| 运算符 | 描述 | 实例               |
| :----: | :--: | :----------------- |
|   +    | 相加 | A + B 输出结果 30  |
|   -    | 相减 | A - B 输出结果 -10 |
|   *    | 相乘 | A * B 输出结果 200 |
|   /    | 相除 | B / A 输出结果 2   |
|   %    | 求余 | B % A 输出结果 0   |
|   ++   | 自增 | A++ 输出结果 11    |
|   --   | 自减 | A-- 输出结果 9     |

### 关系运算符

| 运算符 | 描述                                                         |
| :----: | :----------------------------------------------------------- |
|   ==   | 检查两个值是否相等，如果相等返回 True 否则返回 False。       |
|   !=   | 检查两个值是否不相等，如果不相等返回 True 否则返回 False。   |
|   >    | 检查左边值是否大于右边值，如果是返回 True 否则返回 False。   |
|   <    | 检查左边值是否小于右边值，如果是返回 True 否则返回 False。   |
|   >=   | 检查左边值是否大于等于右边值，如果是返回 True 否则返回 False。 |
|   <=   | 检查左边值是否小于等于右边值，如果是返回 True 否则返回 False。 |

### 逻辑运算符

|      运算符       | 描述                                                         |
| :---------------: | :----------------------------------------------------------- |
|        &&         | 逻辑 AND 运算符。 如果两边的操作数都是 True，则条件 True，否则为 False。 |
| &#x007c; &#x007c; | 逻辑 OR 运算符。 如果两边的操作数有一个 True，则条件 True，否则为 False。 |
|         !         | 逻辑 NOT 运算符。 如果条件为 True，则逻辑 NOT 条件 False，否则为 True。 |

### 位运算符，假定 A 为60，B 为13

|  运算符  | 描述                                                         |
| :------: | :----------------------------------------------------------- |
|    &     | 按位与运算符"&"是双目运算符。 其功能是参与运算的两数各对应的二进位相与。 |
| &#x007c; | 按位或运算符"&#x007c;"是双目运算符。 其功能是参与运算的两数各对应的二进位相或 |
|    ^     | 按位异或运算符"^"是双目运算符。 其功能是参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1。 |
|    <<    | 左移运算符"<<"是双目运算符。左移n位就是乘以2的n次方。 其功能把"<<"左边的运算数的各二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补0。 |
|    >>    | 右移运算符">>"是双目运算符。右移n位就是除以2的n次方。 其功能是把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数。 |

### 赋值运算符

| 运算符 | 描述                                           | 实例                                  |
| :----: | :--------------------------------------------- | :------------------------------------ |
|   =    | 简单的赋值运算符，将一个表达式的值赋给一个左值 | C = A + B 将 A + B 表达式结果赋值给 C |
|   +=   | 相加后再赋值                                   | C += A 等于 C = C + A                 |
|   -=   | 相减后再赋值                                   | C -= A 等于 C = C - A                 |
|   *=   | 相乘后再赋值                                   | C *= A 等于 C = C * A                 |
|   /=   | 相除后再赋值                                   | C /= A 等于 C = C / A                 |
|   %=   | 求余后再赋值                                   | C %= A 等于 C = C % A                 |
|  <<=   | 左移后赋值                                     | C <<= 2 等于 C = C << 2               |
|  >>=   | 右移后赋值                                     | C >>= 2 等于 C = C >> 2               |
|   &=   | 按位与后赋值                                   | C &= 2 等于 C = C & 2                 |
|   ^=   | 按位异或后赋值                                 | C ^= 2 等于 C = C ^ 2                 |
|        | =                                              | 按位或后赋值 C                        |

### 其他运算符

| 运算符 |       描述       |            实例            |
| :----: | :--------------: | :------------------------: |
|   &    | 返回变量存储地址 | &a; 将给出变量的实际地址。 |
|   *    |     指针变量     |     *a; 是一个指针变量     |

### 优先级

| 优先级 |      运算符      |
| :----: | :--------------: |
|   5    | * / % << >> & &^ |
|   4    |  + - &#x007c; ^  |
|   3    | == != < <= > >=  |
|   2    |        &&        |
|   1    | &#x007c;&#x007c; |

## 控制语句
### 条件语句
#### if
```
package main

import "fmt"

func main() {
   /* 局部变量定义 */
   var a int = 100;
 
   /* 判断布尔表达式 */
   if a < 20 {
       /* 如果条件为 true 则执行以下语句 */
       fmt.Printf("a 小于 20\n" );
   } else {
       /* 如果条件为 false 则执行以下语句 */
       fmt.Printf("a 不小于 20\n" );
   }
   fmt.Printf("a 的值为 : %d\n", a);

}
```
#### switch
```
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var grade string = "B"
   var marks int = 90

   switch marks {
      case 90: grade = "A"
      case 80: grade = "B"
      case 50,60,70 : grade = "C"
      default: grade = "D"  
   }

   switch {
      case grade == "A" :
         fmt.Printf("优秀!\n" )    
      case grade == "B", grade == "C" :
         fmt.Printf("良好\n" )      
      case grade == "D" :
         fmt.Printf("及格\n" )      
      case grade == "F":
         fmt.Printf("不及格\n" )
      default:
         fmt.Printf("差\n" );
   }
   fmt.Printf("你的等级是 %s\n", grade );      
}
```
#### select
```
package main

import "fmt"

func main() {
   var c1, c2, c3 chan int
   var i1, i2 int
   select {
      case i1 = <-c1:
         fmt.Printf("received ", i1, " from c1\n")
      case c2 <- i2:
         fmt.Printf("sent ", i2, " to c2\n")
      case i3, ok := (<-c3):  // same as: i3, ok := <-c3
         if ok {
            fmt.Printf("received ", i3, " from c3\n")
         } else {
            fmt.Printf("c3 is closed\n")
         }
      default:
         fmt.Printf("no communication\n")
   }    
}
```
### 循环语句
#### for
```
package main

import "fmt"

func main() {
        sum := 0
        for i := 0; i <= 10; i++ {
                sum += i
        }
        fmt.Println(sum)
}
```
#### break
```
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 10

   /* for 循环 */
   for a < 20 {
      fmt.Printf("a 的值为 : %d\n", a);
      a++;
      if a > 15 {
         /* 使用 break 语句跳出循环 */
         break;
      }
   }
}
```
#### continue
```
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 10

   /* for 循环 */
   for a < 20 {
      if a == 15 {
         /* 跳过此次循环 */
         a = a + 1;
         continue;
      }
      fmt.Printf("a 的值为 : %d\n", a);
      a++;    
   }  
}
```
#### goto
```
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 10

   /* 循环 */
   LOOP: for a < 20 {
      if a == 15 {
         /* 跳过迭代 */
         a = a + 1
         goto LOOP
      }
      fmt.Printf("a的值为 : %d\n", a)
      a++    
   }  
}
```
## Reference
1. https://raw.githubusercontent.com/datawhalechina/go-talent/master/3.%E8%BF%90%E7%AE%97%E7%AC%A6%E3%80%81%E6%8E%A7%E5%88%B6%E8%AF%AD%E5%8F%A5.md
2. https://www.runoob.com/go/go-tutorial.html