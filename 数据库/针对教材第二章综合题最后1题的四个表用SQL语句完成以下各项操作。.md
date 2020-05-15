# 针对教材第二章综合题最后1题的四个表用SQL语句完成以下各项操作。

##  查询订货数量在500～800之间的订单情况。

```mssql
select		*
from		orders
where		Qty between 500 and	800;
```

## 查询产品名称中含“水”字的产品名称与单价。

```mssql
select		Pname, Price
from		Products
where		Pname like ('%水%');
```

## 查询每个月的订单数、总订货数量以及总金额，要求赋予别名，并按月份降序排列。

```mssql
select		count(Ord_no) as '订单数',
			count(Qty) as '总订货数量', 
			count(Amount) as '总金额'
from		orders
group by	Month
order by	Month, desc;
```

## 查询姓王且名字为两个字的客户在1月份的订单情况，并按订货数量降序排列。

```mssql
select		T.Ord_no, T.Month, T.Aid, T.Pid, T.Qty, T.Amount
from		(Orders full outer join Customers on Orders.Cid = Customers.Cid)T
where		T.Cname
like		('王_')
order by	desc;
```



## 查询上海客户总订货数量超过2000的订货月份。

```mssql
select		T.Month
from		(Orders full outer join Customers on Orders.Aid = Customers.Aid) T
group by	city
having		city = '上海' and count(T.Qty) > 2000;
```



## 查询每个产品的产品编号、产品名称、总订货数量以及总金额。

```mssql
select		Products.Pid as '产品编号', 
			Products.Pname as '产品名称', 
			Products.Quantity as '总订货数量', 
			Products.Quantity*Products.price as '总金额'
from		Products;
```



## 查询没有通过北京的代理订购笔袋的客户编号与客户名称。

```mssql
select		T2.Cid, T2.Cname
from		(Orders right outer join 
             	(select Aid from Agents where City not in ('上海'))T1,
             on T1.Aid =  Orders.Aid)T2;
```



## 查询这样的订单号，该订单的订货数量大于3月份所有订单的订货数量。

```mssql
select		*
from		Orders
where		Orders.Amount > (select sum(Amount) from Orders where Month = 3);
```



## 向产品表中增加一个产品，名称为粉笔，编号为P20，单价为1.50，销售数量为25000。

```mssql
insert into		Products
values			('P20', '粉笔', '25000', '1.50')
```



## 将所有单价大于1.00的产品单价增加10%。

```mssql
update		Product
set			Price = Price * 1.1
where		Price > 1.00
```



## 将所有由上海代理商代理的笔袋的订货数量改为2000。 

```mssql
update		Orders
set			Qty = 2000
where		Aid in (select Aid from Agents where City = '上海')
and			Pid = (select Pid from Products where Pname = '笔袋');
```



## 将由A06供给C006的产品P01改为由A05供应，请做必要的修改。 

```mssql
update		Orders
set			Aid = 'A05'
where		Aid = 'A06' and Cid = 'C006' and Pid = 'P01';
```



## 从客户关系中删除C006记录，并从供应情况关系中删除相应的记录。

```mssql
delete
from		Orders
where		Cid = 'C006';

delete
from		Customers
where		Cid = 'C006';
```



## 删除3月份订购尺子的所有订单情况。

```mssql
delete
from		Orders
where		Pid = (select Pid from Products where Pname = '尺子') and Month = 3;
```

