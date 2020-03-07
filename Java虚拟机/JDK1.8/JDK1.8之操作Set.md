## 1. foreach()
例如：
```
/**获取Redis中所有key*/
        Set<String> keys =  redisTemplate.keys("*");
        /**文章点赞的key*/
        List<String> likeKeyList = new ArrayList<>();
        keys.forEach(key->{
            if(key.startsWith("BLOG:LIKE:")){
                likeKeyList.add(key);
            }
        });
```