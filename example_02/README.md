# ORN 1

```sql
  select  
  case
    when p.Title = 'Mr.' then 'Erkek'
    when p.Title in ('Ms', 'Ms.') then 'Kadın'
    when p.Title = 'Mrs.' then 'Evli Kadın'
    when p.Title = 'Miss' then 'Bekar Kadın'
    else 'belirsiz'
  end
  from Person.Person p 
```

# ORN 2

```sql
select	p.Name, 
		(sod.UnitPrice * sod.OrderQty) as toplam
from Production.Product p
		INNER JOIN Sales.SalesOrderDetail sod
				ON sod.ProductID = p.ProductID
where p.ProductID > 100 
		and p.Color is null
		and (sod.UnitPrice * sod.OrderQty) > 5000
		and sod.SalesOrderID IN (select	soh.SalesOrderID
								from	Sales.SalesOrderHeader soh
										INNER JOIN Sales.SalesTerritory st
												ON st.TerritoryID = soh.TerritoryID
								where soh.SubTotal < 3000
										and not st.CountryRegionCode = 'US')
```

# ORN 3

```sql
select	soh.*
from	Sales.SalesOrderHeader soh
where	soh.SalesOrderID In (select	sod.SalesOrderID 
							from	Sales.SalesOrderDetail sod 
							where	sod.ProductID = (select MAX(p.ProductID)
													from	Production.Product p
													where	p.Color = 'Red')
							and sod.UnitPrice > 1000)
```

# ORN 4

```sql
select	sod.ProductID, 
		avg(UnitPrice) 
from	Sales.SalesOrderDetail sod 
where	not exists(select	*
					from	Production.Product p
					where	p.ListPrice < 100 
							and sod.ProductID = p.ProductID)
group by sod.ProductID
having AVG(UnitPrice) > 2000
order by AVG(UnitPrice) desc;
```
