# 1. 介绍
Lucene是一个基于**Java的全文搜索引擎**(full-text search engine)。Lucene本身不是一个完整的应用，它只是一个**类库**，提供了一些API让开发者更加简单的在在应用中集成搜索功能，因此它并不像www.baidu.com 或者www.google.com那么拿来就能用，它只是提供了一种工具让你能实现这些产品

关键词：全文搜索引擎  API
## 1.1. 与数据库like关键字搜索的区别
- like 关键字效率是很低的(需要遍历每一条记录)，并且提供的搜索功能很低级
- 表中的记录，必须完全包含某个检索关键字。

例如我们在数据库中，存储的内容是“北京是中国的首都”，那么我们在搜索的时候，如果搜索的是“中国的首都”，那么我们是可以检索出来这条记录的，但是如果我们搜索的是“中国的首都在哪里”，就无法搜索出来这条记录。究其原因，我们使用的是 like "%keyword%"这种方式检索，数据库记录中必须完全包含检索的内容才能搜索到

- 搜索结果按照相似度排序

所谓相似度，指的是搜索关键字和记录的匹配程度。例如我们在数据库有两条记录，“北京欢迎你”,"中国北京欢迎你"。当我们的检索关键字是"北京欢迎你"的时候，第一条记录时完全匹配上的，而第二条记录时包含搜索关键字。因此第一条记录和搜索关键字的相似度应该高于第二条。但是我们使用like关键字搜索出来的结果，并不会按照相似度进行排序

- 搜索关键字无法高亮显示
如果匹配的话，Lucene能显示，但是like关键字不行
![](_v_images/20191112101029523_24619.png)

## 1.2. 索引与数据库的对应关系
在实际开发中，索引库中的每一条记录都对应着数据库中一条记录。每当向数据库中新增、修改或删除一条记录的时候，我们在索引库中也进行同样的操作。即索引库的增删改操作与数据库的增删改是保持同步的。

如果用伪代码表示的话，就是这样：

```
注：DB表示数据库操作，Index表示索引库操作
       public void add(record){
            DB.add(record);
            Index.add(record);
      }
      
       public void update(record){
            DB.update(record);
            Index.update(record);
      }
      
       public void delete(record){
            DB.delete(record);
            Index.delete(record);
      }
```

# 2. Lucene的执行流程
索引、分词、搜索三部分组成 
## 2.1. 创建索引过程
- 原始数据
        可以是用户在web控制台界面上添加的一条记录；也可以是网络上的任何资源
        
- 建立文档，转化为Document对象
        注意：Build Document过程实际上是提取出我们建立索引的数据的必要的文本信息，Lucene只能针对纯文本内容建立索引，意味着针对PDF、Word等格式的文档，我们必须提取出其文本内容
        
 - Document中的每个字段进行分词(Analyze)处理
        实际上是一种倒叙索引。例如，我们创建的Article实体中content为：“中国的首都是北京....”，Lucene会将内容进行分词，那么这段话可能会被分为:"中国"、"首都"、"北京"等。Lucene建立索引实际上是对分词后的结果建立索引
## 2.2. 搜索过程
- 输入关键字

- 关键字构建成一个Query对象（对关键字进行分词）

- 搜索结果进行一些修饰(Render)，如高亮
# 3. Lucene重要类
## 3.1. 建立索引过程
- Document 
        Document 是用来描述文档的，这里的文档可以指一个 HTML 页面，一封电子邮件，或者是一个文本文件。一个 Document 对象由多个 Field 对象组成的。可以把一个 Document 对象想象成数据库中的一个记录，而每个 Field 对象就是记录的一个字段,**Document则是Lucene建立索引的最小单位**
        
- Field
        Field 对象是用来描述一个文档的某个属性的，比如一封电子邮件的标题和内容可以用两个 Field 对象分别描述
- IndexWriter
        IndexWriter 是 Lucene 用来创建索引的一个核心的类，他的作用是把一个个的 Document 对象加到索引中来
        
- Analyzer
        在一个文档被索引之前，首先需要对文档内容进行分词处理，这部分工作就是由 Analyzer 来做的。Analyzer 类是一个抽象类，它有多个实现。针对不同的语言和应用需要选择适合的 Analyzer。Analyzer 把分词后的内容交给 IndexWriter 来建立索引
        
- Directory
        这个类代表了 Lucene 的索引的存储的位置，这是一个抽象类，它目前有两个实现，第一个是 FSDirectory，它表示一个存储在文件系统中的索引的位置。第二个是 RAMDirectory，它表示一个存储在内存当中的索引的位置

## 3.2. 搜索过程
- IndexSearcher
        IndexSearcher 是用来在建立好的索引上进行搜索的。它只能以只读的方式打开一个索引，所以可以有多个 IndexSearcher 的实例在一个索引上进行操作
        
- Term
        Term 是搜索的基本单位，一个 Term 对象有两个 String 类型的域组成。生成一个 Term 对象可以有如下一条语句来完成：Term term = new Term(“fieldName”,”queryWord”); 其中第一个参数代表了要在文档的哪一个 Field 上进行查找，第二个参数代表了要查询的关键词
        
- Query
        这是一个抽象类，他有多个实现，比如 TermQuery, BooleanQuery, PrefixQuery. 这个类的目的是把用户输入的查询字符串封装成 Lucene 能够识别的 Query
        
- TermQuery
        TermQuery 是抽象类 Query 的一个子类，它同时也是 Lucene 支持的最为基本的一个查询类。生成一个 TermQuery 对象由如下语句完成： TermQuery termQuery = new TermQuery(new Term(“fieldName”,”queryWord”)); 它的构造函数只接受一个参数，那就是一个 Term 对象
        
- Hits
        Hits 是用来保存搜索的结果的
