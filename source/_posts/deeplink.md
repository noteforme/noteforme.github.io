---
title: deeplink
date: 2022-02-25 13:51:15
tags:
---



deeplink same as arouer, 维护了一个id(一个deeplink)和microApp的映射表，app启动后

1. 读取映射表,id和microapp.
   
   ```
    private void parseApps(Context context, InputStream is, String routerScheme, String routerDomain) {
       
               JSONArray jsonArray = new JSONArray(read(is));
               int length = jsonArray.length();
               for(int i = 0; i < length; ++i) {
                   MetaData metaData = (MetaData)JSON.parseObject(jsonArray.getString(i), MetaData.class);
                   String scheme = String.format("%s://%s/app/%s", routerScheme, routerDomain, metaData.id);
                   this.parseMicroApp(context, metaData, metaData.entry.android, scheme);                             
               }
           } 
   
       }
   ```
   
   

2. 解析id和app，通过反射拿到microapp,把映射表的id和app进行映射。
   
   ```
       private boolean parseMicroApp(Context context, MetaData metaData, String clazzName, String scheme) {
           Resources resources = context.getResources();
               Class clazz = Class.forName(clazzName);
               Constructor constructor = clazz.getDeclaredConstructor();
               Object object = constructor.newInstance();
               BaseMicroApp microApp = (BaseMicroApp)object;
               microApp.setMetaData(metaData);
               microApp.setApplicationContext(context);
               microApp.setIcon(metaData.icon);
               microApp.setScheme(scheme);
               microApp.setAppDisplayName(metaData.getDisplayName(resources.getConfiguration().locale, "Unknown"));
               MicroAppManagerImpl.getInstance().registerMicroApp(metaData.id, microApp);
           return true;
       }
   ```
   
   

3. 事件发出后，通过启动id,link拿到microapp,启动还有一个判断启动web还是uriHandler,acitivty,这里启动activity,来通过调用microapp的方法，来触发activity.
   
   ```
   
       public boolean dispatchURI(@Nullable Activity activity, String uriString) {
               Uri uri = Uri.parse(uriString);
                   URIHandler uriHandler = this.findDispatchHandler(uri);
                   List<String> pathComponents = uri.getPathSegments();
                   if (pathComponents.size() > 0) {
                    Map<String, String> parameters = URIDispatcherUtils.calcParameters(uri);
                    boolean shouldIntercept = this.mInterceptor.shouldIntercept(activity, pathComponents, parameters, uriString);
                    if (shouldIntercept) {
                        return false;
                    }
                   }
                boolean result = uriHandler.handleURI(activity, uriString);
               }
       }
   ```
   
   
