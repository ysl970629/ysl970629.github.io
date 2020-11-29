---
title: 判断微分方程的类型（Java伪代码表示）
tags:
  - 数学二
  - 高数
  - 微分方程
categories: 考研
abbrlink: d6882305
date: 2020-05-07 11:38:37
---

由于微分方程有很多种类，在做题时需要快速判别。因此我整理了一下分类，有助于自己理解。

只是伪代码表示，如果直接在电脑上运行，肯定是没法运行的。

<!--more-->

```java
public class 计算微分方程的解 {
	private 计算微分方程的解() {}
	/**
	 * 判断微分方程类型的方法，返回微分方程的类型
	 * 
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
        
        //剩下的就是线性的高阶方程
        //对于二阶线性方程，由于更常见，因此单独研究
        //二阶往上的线性方程，原理是类似的，因此没有在算法中列出（三阶常系数线性）
        //二阶线性形如：y'' + a(x)*y' + b(x)*y = c(x)，分为常系数和变系数
        if(方程.阶数 == 2) {
            //判断是否是线性常系数的
            if(方程.a(x).equals("0") && 方程.b(x).equals("0")) {	//是线性常系数的
                //判断是否是线性常系数齐次
                if(方程.c(x).equals("0")) {	//是齐次的
                    //对于齐次线性，只要找到两个线性无关的特解，即可通过公式得到通解
                    Expression 二阶特征方程 = 方程.获取对应的特征方程();
                    switch(二阶特征方程.根的情况) {	//可通过求△delta判断
                        case "λ1 != λ2":	//两个不相等的实根
                            return "带二阶公式：两个不相等的根的通解公式";
                        case "λ1 == λ2":	//两个相等的实根
                            return "带二阶公式：两个相等的根的通解公式";
                        case "λ1, λ2共轭":	//两个共轭的根
                            return "带二阶公式：两个共轭的根的通解公式";
                    }
                }
                //剩下的就是非齐次的
                //非齐次的c(x)被人为规定了两种情况:（如果不人为规定，特解非常难找）
                //c(x) = P_n(x)*e^(k*x)或者c(x) = (e^(α*x))*(P(x)*cos(β*x) + Q(x)*sin(β*x))
                //第一种情况
                if(方程.c(x).equals("P_n(x)*e^(k*x)")) {
                    System.out.println("非齐次型方程的通解 = 对应的齐次的通解 + 非齐次的特解");
                    StringBuilder s = new StringBuilder("先求对应的齐次的通解，并记录方程的根λ1, λ2");
                    //根据多项式P_n(x)的阶数构建新的多项式exp
                    //比如，
                    //P_n(x)的阶数是0，构建的多项式就是a
                    //P_n(x)的阶数是1，构建的多项式就是a*x + b
                    //P_n(x)的阶数是2，构建的多项式就是a*(x^2) + b*x + c
                    //...
                    Expression exp = Expression.构建多项式(P_n(x).阶数);
                    //如果两个根都不等于k
                    if (c(x).k != λ1 && c(x).k != λ2) {
                        String y_0 = "(exp)*e^(k*x)";
                        s.append(String.format(
                            "然后令y_0(x) = %s带回，解出a和b，得到非齐次特解，合起来就是通解", y_0));
                        return s.toString();
                    }
                    //如果两个根都等于k
                    if (c(x).k == λ1 && c(x).k == λ2) {
                        String y_0 = "(x^2)*(exp)*e^(k*x)"; //注意最前面多了一个x^2
                        s.append(String.format(
                            "然后令y_0(x) = %s带回，解出a和b，得到非齐次特解，合起来就是通解", y_0));
                        return s.toString();
                    }
                    //剩下的就是两个根只有一个等于k
                    String y_0 = "x*(exp)*e^(k*x)";	//注意最前面多了一个x
                    s.append(String.format(
                        "然后令y_0(x) = %s带回，解出a和b，得到非齐次特解，合起来就是通解", y_0));
                    return s.toString();
                }
                //第二种情况
                if(方程.c(x).equals("(e^(α*x))*(P(x)*cos(β*x) + Q(x)*sin(β*x))")) {
                    System.out.println("非齐次型方程的通解 = 对应的齐次的通解 + 非齐次的特解");
                    StringBuilder s = new StringBuilder("先求对应的齐次的通解，并记录方程的根λ1, λ2");
                    //根据多项式P(x)与Q(x)二者中的最高阶数构建新的多项式e1, e2
                    //注意：（1）取二者阶数较高的作为新多项式的阶数。（2）两个多项式的字母不同。
                    //比如，
                    //P(x)的阶数是0，构建的多项式e1就是a，e2就是b
                    //P(x)的阶数是1，构建的多项式e1就是a*x + b，e2就是c*x + d
                    //P(x)的阶数是2，构建的多项式e1就是a*(x^2) + b*x + c，e2就是d*(x^2) + e*x + f
                    //...
                    int 最高阶数 = Math.max(P(x).阶数, Q(x).阶数);
                    Expression e1 = Expression.构建多项式(最高阶数);	
                    Expression e2 = Expression.构建多项式(最高阶数);
                    //如果两个根都不等于α + βi
                    if(α + βi != λ1 && α + βi != λ2) {
                        String y_0 = "(e^(α*x))*((e1)*cos(β*x) + (e2)*sin(β*x))";
                        s.append(String.format(
                            "然后令y_0(x) = %s带回，解出abcd，得到非齐次特解，合起来就是通解", y_0));
                        return s.toString();
                    }
                    //如果两个根都等于α + βi
                    if(α + βi == λ1 && α + βi == λ2) {
                        String y_0 = "(x^2)*(e^(α*x))*((e1)*cos(β*x) + (e2)*sin(β*x))"; //注意前面多了x^2
                        s.append(String.format(
                            "然后令y_0(x) = %s带回，解出abcd，得到非齐次特解，合起来就是通解", y_0));
                        return s.toString();
                    }
                    //剩下的就是两个根只有一个等于α + βi
                    String y_0 = "x*(e^(α*x))*((e1)*cos(β*x) + (e2)*sin(β*x))";	//注意最前面多了一个x
                    s.append(String.format(
                        "然后令y_0(x) = %s带回，解出abcd，得到非齐次特解，合起来就是通解", y_0));
                    return s.toString();
                }
            }
            //剩下的就是线性变系数的：也分为齐次和非齐次
            //不需要会解，只要能明白解的结构即可
            //齐次：
            if(方程.c(x).equals("0")) {
                System.out.println("通解公式:y = c_1*φ_1(x) + c_2*φ_2(x)");
                return String.format(
                    "高阶线性变系数齐次方程，找到%d个线性无关的特解，带公式得到通解", 方程.阶数);
            }
			//非齐次：除了齐次就是非齐次
            System.out.println("非齐次型方程的通解 = 对应的齐次的通解 + 非齐次的特解");
            return "高阶线性变系数非齐次方程，找到对应的齐次的特解，通过通解公式得到齐次的通解" + 
                "然后加入非齐次的特解，合起来就是非齐次通解";
        }
        //二阶往上的方程，由于原理相同，且做题时比较少见，不再列出。
        //（仅列出三阶线性常系数）
        if(方程.阶数 == 3) {
            if(方程.a(x).equals("0") && 方程.b(x).equals("0")) {	//是线性常系数的
                //判断是否是齐次的
                if(方程.c(x).equals("0")) {
					Expression 三阶特征方程 = 方程.获取对应的特征方程();
                      switch(三阶特征方程.根的情况) {	
                          case "λ1, λ2, λ3都是实数且λ1 != λ2 != λ3":
                              return "带三阶公式：对应二阶的case 2";
                          case "λ1, λ2, λ3都是实数，且λ1 = λ2 != λ3":
                              return "带三阶公式。没有对应的二阶";
                          case "λ1, λ2, λ3都是实数，且λ1 = λ2 = λ3":
                              return "带三阶公式：对应二阶的case 1";
                          case "λ1是实数, λ2与λ3共轭":
                              return "带三阶公式，没有对应的二阶";
                     }
                }
                //剩下的就是非齐次的（老师没讲）
            }
        }
	}
}

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
    /**
     * 得到微分方程的特征方程
     */
    public Expression 获取对应的特征方程() {
        //省略代码
    }
}

public class Expression {
    //省略代码
}
```

