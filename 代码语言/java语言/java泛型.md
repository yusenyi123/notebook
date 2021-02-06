

# java泛型



本质:数据类型的参数化

我们可以把“泛型”理解为数据类型的一个占位符(形式参数)，即告诉编译器，在调用泛型时必须传入实际类型。





## 泛型类

```
1.泛型类可以有多个泛型参数

public class Generictest<E,A,B>{

}

2.泛型类的构造器如下：
public   Generictest(){

}
而下面是错误的： 
public   Generictest<E,A,B>(){

}


3.实例化后，操作原来泛型位置的结构必须与指定的泛型类型一致。

4.泛型不同的引用不能相互赋值。
尽管在编译时 ArrayList<String>和 ArrayList<Integer>是两种类型，但是在运行时只有
一个ArrayList类型被加载到jvm
5.泛型如果不指定，将被擦除，泛型对应的类型均按照 Object/处理，但不等价
于 Object。经验：泛型要使用一路都用。要不用，一路都不要用

6.泛型类静态方法不能使用类的泛型

7.异常类不能是泛型类

8.不能使用new E()。但是可以： E elements=(E)new Object();


9.子类继承泛型类的情况,如下图
```



![image-20210111211450868](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20210111211718.png)



```
java7的泛型类型推断 产生
在以前的版本中使用泛型类型，需要在声明并赋值的时候，两侧都加上泛型类型。

Map<String, String> myMap = new HashMap<String, String>();

你可能觉得：在声明变量的的时候已经指明了参数类型，为什么还要在初始化对象时再指定？

幸好，在Java SE 7中，这种方式得以改进，现在你可以使用如下语句进行声明并赋值：

Map<String, String> myMap = new HashMap<>();  //注意后面的"<>" 

在这条语句中，编译器会根据变量声明时的泛型类型自动推断出实例化HashMap时的泛型类型。
再次提醒一定要注意new HashMap后面的“<>”，只有加上这个“<>”才表示是自动类型推断，否则就是非泛型类型的HashMap，并且在使用编译器编译源代码时会给出一个警告提示。
但是：Java SE 7在创建泛型实例时的类型推断是有限制的；只有构造器的参数化类型在上下文中被显著的声明了，才可以使用类型推断，否则不行。
```



## 泛型接口





## 泛型方法

``` 
泛型方法：在方法中出现了泛型的结构，泛型参数与类的泛型参数没有在何关系
换句话说，泛型方法所属的类是不是泛型类都没有关系
泛型方法，可以声明为静态方法(使用static修饰)。原因：泛型参数是在调用方法时确定的。并非在实例化类时定

//如果泛型方法中参数中传入了T，则传入的参数是什么数据类型，当前方法就是什么数据类型

//如果泛型方法中参数没有传入了T，则使用什么类型的参数去接受方法的返回值，方法内的T就是什么数据类型


泛型方法的结构<E> 表示声明方法中使用到了泛型,用E代表泛型
对象方法
public <E> copyArraytoList(E [] arrs){

}
静态方法
public static <E> copyArraytoList(E [] arrs){

}
```

```java
//如果泛型方法中参数中传入了T，则传入的参数是什么数据类型，当前方法就是什么数据类型

//如果泛型方法中参数没有传入了T，则使用什么类型的参数去接受方法的返回值，方法内的T就是什么数据类型
	@SuppressWarnings("unchecked")
	public <T> T insertReturnKey(String sql, Object... params) {
		Connection conn = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			conn = JDBCUtil.getConn();
			// 开启主键返回功能
			ps = conn.prepareStatement(sql, PreparedStatement.RETURN_GENERATED_KEYS);
			if(null != params){
				// 有参数, 设置sql参数
				for (int i = 0; i < params.length; i++) {
					ps.setObject(i+1, params[i]);
				}
			}
			ps.executeUpdate();
			
			// 获取结果集中主键, 此时的结果集中就放了一个主键数据
			rs = ps.getGeneratedKeys();
			if(rs.next()){
				// 一个主键列, 所以通常使用序号获取
				return (T) rs.getObject(1);
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtil.close(conn, ps, rs);
		}
		return null;
	}
	@Override
	public void insertOrder(Order order) {
		String sql = new StringBuffer()
		.append(" insert into ")
		.append("   t_order ")
		.append(" (user_id,no,num,total_price,create_date) ")
		.append("   values ")
		.append(" (?, ?, ?, ?, ?) ")
		.toString();
        
        //使用Long类型去接受泛型方法的返回值
		Long key = jt.insertReturnKey(sql, order.getUserId(),
				order.getNo(), order.getNum(),
				order.getTotalPrice(), order.getCreateDate());
		// 主键返回, 把获得的主键值, 设置到参数对象中
		order.setId(key.intValue());
	}
```

