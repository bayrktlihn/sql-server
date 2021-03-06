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

## Çözüm 1
```sql
select * 
from Sales.SalesOrderHeader soh
where soh.SalesOrderID = (select	max(sod.SalesOrderID)
							from Sales.SalesOrderDetail sod
							where sod.ProductID IN (select p.ProductID
													from Production.Product p
													where p.ProductID > 500))
```

## Çözüm 2

```sql
select	*
from	Sales.SalesOrderHeader soh
where soh.SalesOrderID = (select	max(sod.SalesOrderID)
						from	Sales.SalesOrderDetail sod
							INNER JOIN Production.Product p
									ON p.ProductID = sod.ProductID
						where p.ProductID > 500)
```

# ORN 4

```sql
create procedure uspTotalAmountByYearAndMonth(@year int, @month int)
as
begin
select	soh.SalesOrderID,
		YEAR(soh.ShipDate) as Yil,
		MONTH(soh.ShipDate) as Ay,
		SUM(sod.UnitPrice * sod.OrderQty) as ToplamTutar
from Sales.SalesOrderHeader soh
	INNER JOIN Sales.SalesOrderDetail sod
			ON sod.SalesOrderID = soh.SalesOrderID
where YEAR(soh.ShipDate) = @year and MONTH(soh.ShipDate) = @month
group by	soh.SalesOrderID, 
			YEAR(soh.ShipDate),
			MONTH(soh.ShipDate)
end

exec uspTotalAmountByYearAndMonth 2011, 9
```

# ORN 5

```sql
create procedure insertCulture(@givenPassword varchar(30), @culterId varchar(30), @Name varchar(50))
as
begin

	if 'alihan' = @givenPassword
	begin

		if exists(select * from Production.Culture where CultureID = @culterId)
		begin
			print 'ayni ididen eklenemez' 
		end
		else
		begin
			insert into Production.Culture(CultureID, Name, ModifiedDate) values(@culterId, @Name, GETDATE())
			print 'Eklendi' 
		end
	
	end
	else
	begin
		print 'Sifre Yanlis'
	end
	
end
```
