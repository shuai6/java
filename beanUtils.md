## BeanUtils，不同 jar 包所埋的坑
### 背景
  线上代码，两个地方使用了相同的copy 方式，但是执行的效果却完全不一样。

    BeanUtils.copyProperties(invoiceMonth, invoiceMonthDTO);
### 问题描述
1、我们的项目使用了Spring 包和 Apache 包两种，而这两个包都提供了 BeanUtils 工具方法

2、看一下源码：

   1）、Apache 源码是，第一参数：dest；第二参数：source。

    
    public static void copyProperties(Object dest, Object orig)
        throws IllegalAccessException, InvocationTargetException {
    
        BeanUtilsBean.getInstance().copyProperties(dest, orig);
    }
   2）、Spring 源码是，第一参数：source；第二参数是 target。

public static void copyProperties(Object source, Object target) throws BeansException {
   copyProperties(source, target, null, (String[]) null);
}
 

综上可以看出，两个开源包提供的工具类是有差异的，一不小心就会因为 jar 包的错误引入导致程序错误。