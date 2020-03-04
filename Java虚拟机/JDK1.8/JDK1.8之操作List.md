# 1. stream().forEach()
- 语法：list.stream().forEach()
例如：
```
List<Like> list = getLikeListFromRedis(blogId);
        list.stream().forEach(like -> {
            System.out.println(like.likeBlogId);
        });
```
# 2. stream().map()处理List，并给另外一个List赋值
- 语法：stream().map()
例子：
```
list.stream().map(like -> {
            System.out.println(like.likeBlogId);
            return "";
        }).collect(Collectors.toList());

        list.stream().forEach(like -> {
            System.out.println(like.likeBlogId);
        });
```