# DBUtils的通用增删方法：

## 1，通用方法1，通过传入函数的参数个数来确定函数

> 当一个函数要传入的参数个数不确定的时候，可以用(Object ... args)来概括，表示要传入的参数个数和参数类型都不确定并且args相当于数组
>
> 可以将对数据的增删改操作封装再一个函数里面，统一用update描述，然后给函数传入一个sql语句字符串和一个参数数组，用Object类型，例如：

```Java
public static void update(String sql,Object... args){
```

其中 Object ...args表示又多个参数，由于每个参数的类型不一定相同，所以都用Object表示，函数体里面的参数设置，可以用数组来实现。

```Java
for(int i = 0 ;i < args.length;i++)
{
    preparedStatement.setObject(i+1,args[i]);

}
```

## 2通过参数SQL语句里面的？个数来确定

元数据    Meta data

> 描述数据的数据  

可以先获取SQL语句里面的问号，以问号的个数为准

```java
preparedStatement = connection.prepareStatement(sql);
ParameterMetaData parameterMetaData=preparedStatement.getParameterMetaData();//  获取到占位符的个数
int count = parameterMetaData.getParameterCount();
for(int i = 0 ; i < count ; i++)
{
    preparedStatement.setObject(i+1,args[i]);
}
```

# DBUtils的通用查询方法：

## //TODO





# MVC设计模式：

### JSP的开发模式：

