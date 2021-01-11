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
