---
title: DesignPattern-Builder
comments: true
date: 2017-10-07 17:45:46
tags:
categories:  DesignPatterns
---

# Builder模式

##　描述　

既能保证像重叠构造器模式那样的安全性，也能保证像JavaBeans模式那么好的可读性

不直接生成想要的对象，而是让客户端利用所有必要的参数调用构造器（或者静态工厂），得到一个builder对象，然后客户端在builder对象上调用类似于setter的方法，来设置每个相关的可选参数


```

public class NutritionFacts_03_Builder {
    private  final  int servingSize;
    private  final  int servings;
    private  final  int calories;
    private  final  int fat;
    private  final  int sodium;
    private  final  int carbohydrate;
    public  static  class  Builder{
        //Required parameters
        private  final  int servingSize;
        private  final  int servings;

        //Optional parameters = initialized to default values
        private  int calories     = 0;
        private  int fat          = 0;
        private  int carbohydrate = 0;
        private  int sodium       = 0;

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings = servings;
        }

        public  Builder calories(int val){
            calories = val;
            return this;
        }
        public  Builder fat(int val){
            this.fat = fat;
            return  this;
        }
        public  Builder carbohydrate(int val){
            carbohydrate = val;
            return  this;
        }
        public  Builder sodium(int val){
            sodium = val;
            return  this;
        }
        public  NutritionFacts_03_Builder build(){
            return  new NutritionFacts_03_Builder(this);
        }
    }

    public NutritionFacts_03_Builder(Builder builder) {
        this.servingSize = builder.servingSize;
        this.servings = builder.servings;
        this.calories = builder.calories;
        this.fat = builder.fat;
        this.sodium = builder.sodium;
        this.carbohydrate = builder.carbohydrate;
    }

    public static void main(String[] args) {
        NutritionFacts_03_Builder cocaCola = new NutritionFacts_03_Builder.Builder(240,9)
                .calories(100).sodium(35).carbohydrate(27).build();
    }
}

```

## 疑惑 

 返回的this时怎么替代对象的
