# 1. include指令：

```
<%@include file="目标文件" %>；此指令是“先包含，在运行”，即将目标文件全部包含

              进来，形成一个jsp文件，然后一起运行，转化成servlet；此种包含为静态包含；
```

 

# 2. include标签：
```

<jsp:include page="目标文件"/>、

             <jsp:include page="目标文件">

                 <jsp:param name="参数名" value="参数值"/>

                  ............

             </jsp:include>

             此标签的执行过程是：当执行到include标签的时候，页面转向目标文件，然后执行目标文

             件，将执行结果包含进来。
```

 

# 3. forward标签：

```
<jsp:forward page="目标文件"/>

             <jsp:forward page="目标文件">

                 <jsp:param name="参数名" value="参数值"/>

                  ............

             </jsp:forward >

             当执行到此标签的时候，会转向执行到目标文件，这个标签之后的文件不再执行。

```
            

# 4. include指令和include标签的区别：
include指令将目标文件包含进来，一起执行；生成一个servlet，include标签是将目标文件执行后，将结果包含进来，然后在执行，每个目标文件都生成一个servlet。

include标签和forward标签的区别：对于标签前的内容，include执行且显示，forward执行但不显示；对于标签后的内容，include执行且显示，forward不执行。