# ORN 1
```sql
select p.Color,
case
	when p.Color = 'Red' then 'Kirmizi'
	when p.Color = 'Blue' then 'Mavi'
	when p.Color = 'Black' then 'Siyah'
	when p.Color = 'Silver' then 'Gümüs'
	when p.Color = 'Green' then 'Yesil'
	when p.Color = 'Yellow' then 'Sari'
	when p.Color = 'Silver/Black' then 'Gumus/Siyah'
	when p.Color = 'White' then 'Beyaz'
	when p.Color = 'Multi' then 'Cok'
	else 'Renksiz'
end as Renk,
SUM(sod.UnitPrice)
from Production.Product p
	INNER JOIN Sales.SalesOrderDetail sod
			ON sod.ProductID = p.ProductID
group by p.Color
having SUM(sod.UnitPrice) > 2000
```

# ORN 2

```sql
select sod.ProductID, 
	sum(sod.UnitPrice)  Total
from Sales.SalesOrderDetail sod
where sod.ProductID = (select max(p.ProductID) - 23
						from Production.Product p)
group by sod.ProductID
```

# ORN 3
```sql
select * 
from Sales.SalesOrderHeader soh
where soh.SalesOrderID = (select max(sod.SalesOrderID)
							from Sales.SalesOrderDetail sod
							where sod.ProductID IN (select p.ProductID
													from Production.Product p
													where p.ProductID > 500))
```
