## 标签
如果选项上没有标签，ANTLR只会为每个规则生成一个visit方法。
在新的语法中，标签出现在选项的右边缘，且以“#”符号开头：
```
stat
    : expr                   # printExpr
    | ID '=' expr            # assign
    ;

expr
    : expr op=(MUL|DIV) expr # MulDiv
    | expr op=(ADD|SUB) expr # AddSub
    | INT                    # int
    | ID                     # id
    | '(' expr ')'           # parens
    ;
```
## 生成Visitor接口和默认实现
```
antlr -no-listener -visitor Calc.g
```
