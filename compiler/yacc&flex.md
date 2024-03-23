### yacc flex

flex 词法分析

yacc 语法分析


- 终结符   %token
- 非终结符 %type

**%union 和 union**

在yacc和c++中都可以实现共享内存：

在任何给定时间，%union中只能有一个字段包含有效数据。


#### 实现一个计算器

`1 + 2 * 3 - 1 + (-2 * -3) = 21`

```
S->P
P->P+T | P-T | T
T->T*F | F
F->int | -F | (P)
```
