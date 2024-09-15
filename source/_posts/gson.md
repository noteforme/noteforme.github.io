---
id: 383
title: GSON
date: '2024-06-07T03:40:36+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=383'
permalink: /2024/06/07/gson/
categories:
    - Library
---



# Running Gson source Codew

注释

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/gson/20240608090817.jpg)

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/gson/20240608091843.jpg)

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/gson/20240608091832.jpg)

给GitHub PR

https://www.bilibili.com/video/BV1JJ41197UK?spm_id_from=333.337.search-card.all.click

json格式

数组 [1,2,3]

对象格式 {"key":"value"}

## $Gson$Types理解

官方教程 [https://github.com/google/gson/blob/master/UserGuide.md](https://github.com/google/gson/blob/master/UserGuide.md "官方教程")

源码分析
http://www.jianshu.com/p/89c314ae8c0b

## Gson转map

Gson可以直接把json转为map,但是在转成map时，默认將int long型的数字,转换成doublel类型

```java
  String sms = "{\"Code\":200,\"Data\":{\"Code\":\"\"},\"Message\":\"发送成功\"}";
  Map<String, Object> map = new Gson().fromJson(sms, HashMap.class);
```

这里装成map后，Code值尾 200.0 ,解析方法

```
  Gson gson = new GsonBuilder()
            .registerTypeAdapter(new TypeToken<TreeMap<String, Object>>() {
            }.getType(), new JsonDeserializer<TreeMap<String, Object>>() {

                @Override
                public TreeMap<String, Object> deserialize(JsonElement json, Type typeOfT, JsonDeserializationContext context) throws JsonParseException {
                    TreeMap<String, Object> treeMap = new TreeMap<>();
                    JsonObject jsonObject = json.getAsJsonObject();
                    Set<Map.Entry<String, JsonElement>> entrySet = jsonObject.entrySet();
                    for (Map.Entry<String, JsonElement> entry : entrySet) {
                        treeMap.put(entry.getKey(), entry.getValue());
                    }
                    return treeMap;
                }
            }).create();
```

[参考](http://blog.csdn.net/liangrui_cust/article/details/51197974) 

* map to json(生成json数据)
  
        HashMap<String, Object> map = new HashMap<>();
        map.put("key", "real_name");
        map.put("value", userName);
        list.add(map);
        map = new HashMap<>();
        map.put("key", "mobile_phone");
        map.put("value", userPhone);
        map.put("hidden", false);
        list.add(map);
        userInfo.data = new Gson().toJson(list);

* kotlin  json to object

```
val type = object : TypeToken<BaseCount<List<Site>>>() {}.type
var response = gson.fromJson<BaseCount<List<Site>>>(json, type)
```

https://ask.helplib.com/java/post_498433

## java parse

```
 public static List<RecipeBean> analysisChild(String receps) throws JSONException {
        List<RecipeBean> list = new ArrayList<>();
//        JSONObject jsonObject = new JSONObject();
        JSONArray jsonArray = new JSONArray(receps);
        for (int i = 0; i < jsonArray.length(); i++) {
            JSONObject object = jsonArray.getJSONObject(i);
            RecipeBean mPatient = new RecipeBean(TYPE_ITEM);
            if (object.has("drug_name")){
                mPatient.setDrug_name(object.getString("drug_name"));
            }
            if (object.has("drug_num")){
               mPatient.setDrug_num(object.getString("drug_num"));
            }
            if (object.has("frequency")){
             mPatient.setFrequency(object.getString("frequency"));
            }
            if (object.has("num")){
               mPatient.setNum(object.getString("num"));
            }
            if (object.has("drug_num")){
               mPatient.setDrug_num(object.getString("drug_num"));
            }
            if (object.has("pharmacy_type")){
               mPatient.setPharmacy_type(object.getString("pharmacy_type"));
            }
            if (object.has("medicine_num")){
             mPatient.setMedicine_num(object.getString("medicine_num"));
            }
            if (object.has("medicine_unit")){
              mPatient.setMedicine_unit(object.getString("medicine_unit"));
            }
            list.add(mPatient);
        }
        return list;
    }
```