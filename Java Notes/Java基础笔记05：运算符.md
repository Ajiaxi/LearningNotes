### 1. 加、减、乘、除、取模（%）

### 2. 自增（++）、自减（--）

- 自增自减符号位于左边：先自增自减，再参与运算
- 自增自减符号位于右边：先进行运算，后自增自减
- 改变的是它本身

## 3. 三目运算

```java
int A = 10
int B = 20
int max = A > B ? A : B //如果A大于B，那么把A给max，否则把B给max
```

## 4. 逻辑运算

### 4.1 与操作（&、&&）

- 用&的时候，&之后的语法有错误会提示错误
- 用&&的时候，&之后的语法有错误，但之前的语法无错误，不会提示错误。短路与性能高

### 4.2 或操作（|、 ||）

- 用法和与操作是一样的

## 5. 位运算

位运算主要用于二进制数据操作的，可以使用&、|、>>、<<（向左移位）、^、~

- 与：相同为1，不同为0
- 或：有一个1，结果就是1 
- 移位不会改变原始数据

面试题：&、&&的区别；|、||区别：

分两种情况去讨论，逻辑运算符，位运算符

