## ANTLR4编程的基本流程
+ 基于需求按照ANTLR4的规则编写自定义语法的语义规则, 保存成以g4为后缀的文件。
+ 使用ANTLR4工具处理g4文件，生成词法分析器、句法分析器代码、词典文件。
+ 编写代码继承Visitor类或实现Listener接口，开发自己的业务逻辑代码。

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
所有被标签标明的选项在生成的Visitor接口中都定义了一个visit方法：
```
public interface CalcVisitor<T> extends ParseTreeVisitor<T> {
    T visitProg(CalcParser.ProgContext ctx);
    T visitPrintExpr(CalcParser.PrintExprContext ctx);
    T visitAssign(CalcParser.AssignContext ctx);
    ...
}
```
默认实现：
```java
public class EvalVisitor extends CalcBaseVisitor<Integer> {
    /** "memory" for our calculator; variable/value pairs go here */
    Map<String, Integer> memory = new HashMap<String, Integer>();

    /** ID '=' expr */
    @Override
    public Integer visitAssign(CalcParser.AssignContext ctx) {
        String id = ctx.ID().getText();  // id is left-hand side of '='
        int value = visit(ctx.expr());   // compute value of expression on right
        memory.put(id, value);           // store it in our memory
        return value;
    }

    /** expr */
    @Override
    public Integer visitPrintExpr(CalcParser.PrintExprContext ctx) {
        Integer value = visit(ctx.expr()); // evaluate the expr child
        System.out.println(value);         // print the result
        return 0;                          // return dummy value
    }

    /** INT */
    @Override
    public Integer visitInt(CalcParser.IntContext ctx) {
        return Integer.valueOf(ctx.INT().getText());
    }

    /** ID */
    @Override
    public Integer visitId(CalcParser.IdContext ctx) {
        String id = ctx.ID().getText();
        if ( memory.containsKey(id) ) return memory.get(id);
        return 0;
    }

    /** expr op=('*'|'/') expr */
    @Override
    public Integer visitMulDiv(CalcParser.MulDivContext ctx) {
        int left = visit(ctx.expr(0));  // get value of left subexpression
        int right = visit(ctx.expr(1)); // get value of right subexpression
        if ( ctx.op.getType() == CalcParser.MUL ) return left * right;
        return left / right; // must be DIV
    }

    /** expr op=('+'|'-') expr */
    @Override
    public Integer visitAddSub(CalcParser.AddSubContext ctx) {
        int left = visit(ctx.expr(0));  // get value of left subexpression
        int right = visit(ctx.expr(1)); // get value of right subexpression
        if ( ctx.op.getType() == CalcParser.ADD ) return left + right;
        return left - right; // must be SUB
    }

    /** '(' expr ')' */
    @Override
    public Integer visitParens(CalcParser.ParensContext ctx) {
        return visit(ctx.expr()); // return child expr's value
    }
}
```

## prog
规则prog 表示prog是一个或多个stat。
## stat
规则stat 适配三种子规则：空行、表达式expr、赋值表达式 ID’=’expr。
## expr
表达式expr适配五种子规则：乘除法、加减法、整型、ID、括号表达式。很显然，这是一个递归的定义。
## 基础元素定义
最后定义的是组成复合规则的基础元素，比如：规则`ID: [a-zA-Z]+`表示ID限于大小写英文字符串；INT: [0-9]+; 表示INT这个规则是0-9之间的一个或多个数字，当然这个定义其实并不严格。再严格一点，应该限制其长度。



