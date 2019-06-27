# 1. fragment加载语法


templatename::selector：”::”前面是模板文件名，后面是选择器
::selector：只写选择器，这里指fragment名称，则加载本页面对应的fragment
templatename：只写模板文件名，则加载整个页面
```
  ================== fragment语法 ============================= 
    <!--  语法说明  "::"前面是模板文件名，后面是选择器 -->
    <div th:include="template/footer::copy"></div>
    <!-- 只写选择器，这里指fragment名称，则加载本页面对应的fragment -->
    <div th:include="::#thispage"></div>
    <!-- 只写模板文件名，则加载整个页面 -->
    <div th:include="template/footer"></div>
================= 加载块 ============================
 
```
```
<span id="thispage">
    div in this page.
</span>
```
th:include 和 th:replace都是加载代码块内容，但是还是有所不同

th:include：加载模板的内容： 读取加载节点的内容（不含节点名称），替换div内容
th:replace：替换当前标签为模板中的标签，加载的节点会整个替换掉加载他的div 
公共部分如下：

```
<!-- th:fragment 定义用于加载的块 -->
<span th:fragment="pagination"> 
the public pagination
</span>
```
引用时如下：

```
================= th:include 和 th:replace============================
<!-- 加载模板的内容： 读取加载节点的内容（不含节点名称），替换<div>的内容 -->
<div th:include="pagination::pagination">1</div>
<!-- 替换当前标签为模板中的标签： 加载的节点会整个替换掉加载他的<div>  -->
<div th:replace="pagination::pagination">2</div>
结果如下：

<!-- 加载模板的内容： 读取加载节点的内容（不含节点名称），替换<div>的内容 -->
<div> the public pagination</div>
<!-- 替换当前标签为模板中的标签： 加载的节点会整个替换掉加载他的<div>  -->
<span> the public pagination</span>
```

