# 1. Json对象
## 1.1. Json对象转化为String
```
JSON.stringify(obj)将JSON转为字符串。
```

## 1.2. String转化为Json
```
JSON.parse(string)将字符串转为JSON格式
```
# 2. String
## 2.1. Json转化为String
- JSONArray转换string方法实例
```
public static void main(String[] args) throws JSONException {
    //创建JSONObject对象
    JSONObject json = new JSONObject();
    //向json中添加数据
    json.put("username", "wanglihong");
    json.put("height", 12.5);
    json.put("age", 24);
    //创建JSONArray数组，并将json添加到数组
    JSONArray array = new JSONArray();
    array.put(json);

    //转换为字符串
    String jsonStr = array.toString()
    System.out.println(jsonStr);
}
```

-JSONObject转化为String
```
public class Json {
    public static void main(String[] args) {
        Book book = new Book();
        book.setId("111");
        book.setName("语文");

        User u = new User();
        u.setId("002");
        u.setName("xim");
        u.setBook(book);

        JSONObject jo = JSONObject.fromObject(u);
        System.out.println(jo);
    }
}
```

## 2.2. String转化为Json
```
JSONArray jsonArray3 = JSONArray.fromObject("['json','is','easy']" );
```


# 3. Map
## 3.1. json转化为map
```
String json2 = "{'k1':{'age':10,'sex':'男','userName':'xiapi1'},'k2':{'age':12,'sex':'女','userName':'xiapi2'}}";
    JSONObject jsonObject2 = JSONObject.fromObject(json2);
    Map < String,Class < ?>>typeMap2 = new HashMap < String,Class < ?>>();
    Map < String,Student > output2 = (Map < String, Student > ) JSONObject.toBean(jsonObject2, Map.class, typeMap2);
    System.out.println("Map<String,Student>");
    System.out.println(output2.size());
    System.out.println(output2.get("k1"));
```
## 3.2. map转化为json
```
Map map = new HashMap();
map.put("name", "json");
map.put("bool", Boolean.TRUE);
map.put("int", new Integer(1));
map.put("arr", new String[] { "a", "b" });
map.put("func", "function(i){ return this.arr[i]; }");
JSONObject json = JSONObject.fromObject(map);
```
# 4. list
## 4.1. json转list
```
String json = "{\"address\":\"chian\",\"birthday\":{\"birthday\":\"2010-11-22\"}," + "\"email\":\"email@123.com\",\"id\":22,\"name\":\"tom\"}";
        json = "[" + json + "]";
        jsonArray = JSONArray.fromObject(json);
        List < Student > list = JSONArray.toList(jsonArray, Student.class);
        System.out.println(list.size());
        System.out.println(list.get(0));

        list = JSONArray.toList(jsonArray);
        System.out.println(list.size());
        System.out.println(list.get(0)); //MorphDynaBean
```

## 4.2. list转json
```
List list = new ArrayList();
list.add( "one" );
list.add( "two" );
JSONArray jsonArray2 = JSONArray.fromObject( list );
```