---
title: 判断微分方程的类型（Java伪代码表示）
date: 2020-05-07 11:38:37
tags: [数学二,高数,微分方程]
categories: 考研
---

由于微分方程有很多种类，因此在做题时需要快速判别。我整理了一下分类，有助于自己理解。

只是伪代码表示，如果直接在电脑上运行，肯定是没法运行的。

```java
interface 常数值 {
    //定义常用的常数值
	public static final int ERROR = Integer.MIN_VALUE;
    //省略代码
}

public class Expression implements 常数值{
    //省略代码
}

public class 微分方程 implements 常数值 {
	public Expression 表达式;
    public int 阶数;
    public Expression a(x) = ERROR; //a(x)不一定存在（只有线性的方程存在），因此预先定义为ERROR
	public Expression b(x) = ERROR;	//b(x)不一定存在（只有线性的方程存在），因此预先定义为ERROR
    public Expression c(x) = ERROR;	//c(x)不一定存在（只有线性的方程存在），因此预先定义为ERROR
    
    public 微分方程(Expression 表达式) {
        this.表达式 = 表达式;
        this.阶数 = 表达式.最高阶导数的阶数;
        this.a(x) = 表达式.a(x);
        this.b(x) = 表达式.b(x);
        this.c(x) = 表达式.c(x);
    }
	/**
	 * 判断微分方程是否满足特定要求
	 * @param s: 判断条件
	 */
	public boolean is(String s) {
		//省略代码
	}
}

public class 微分方程Util {
	private 微分方程Util() {}
	/**
	 * 判断微分方程类型的方法，返回微分方程的类型
	 * @param 方程: 待判断的微分方程
	 * @return 微分方程的种类
	 */
	public static String 判断微分方程类型(微分方程 方程) {
        //对于一阶方程：
		if (方程.阶数 <= 1) {
             //第一种
			if(方程.is("可因式分解")) {
				return "可分离变量型，分离变量后两边积分";
			}
             //第二种
			if (方程.is("可化为φ(y/x)形式")) {
				return "齐次型，带公式";
			}
             //剩下的就都是线性的了；y' + b(x)*y = c(x)，包括齐次与非齐次两种
             //第三种
			if (方程.c(x).equals("0")) {
				return "一阶线性齐次型，带公式";
			}
             //第四种
             //去掉线性齐次后，剩下的都是线性非齐次。即Q(x) != 0
             System.out.println("非齐次型方程的通解 = 对应的齐次的通解 + 非齐次的特解");
             //上述是线性微分方程的解的“叠加原理”。不仅一阶，高阶的也具有此特点
             return "一阶线性非齐次型，带公式";
		}
        //除了一阶方程，剩下的都是高阶方程
        
        //对于二阶线性方程，由于更常见，因此单独研究。但是二阶往上的线性方程，原理是类似的
        if(方程.阶数 == 2 && 方程.is("线性")) {	//二阶线性形如：y'' + a(x)*y' + b(x)*y = c(x)
            //如果方程是线性的：分为线性齐次和线性非齐次
            //齐次：
            if(方程.c(x).equals("0")) {
                return String.format(
                    "高阶线性齐次方程，找到%d个特解，带公式得到通解", 方程.阶数);
            }
        }
        //对于可降阶的高阶，有三种情况：
        //情况1
        if(方程.is("f(x, y^(n)) = 0 类型")) {
            System.out.println("只有y^(n)，没有y'和y");
            return "第一种可降阶，连续积分";
        }
        //情况2
        if(方程.is("f(x, y', y'') = 0 类型")) {
            System.out.println("有x, y'和y''，但是缺y");
            return "第二种可降阶，令y' = p 降阶，" + 
                "会变成一阶线性齐次方程。带公式。如果有必要的话，再降阶一次";
        }
        //情况3
        if(方程.is("f(y, y', y'') = 0 类型")) {
            System.out.println("有y, y'和y''，但是缺x（没有单独出现x）");
            return "第三种可降阶，令y' = p, y'' = p*(dp/dy) 降阶，" + 
                "会变成一阶线性齐次方程。带公式。如果有必要的话，再降阶一次";
        }

	}
}
```

