# ORN 1
```sql
select p.ProductId,
p.Color,
case
	when p.Color = 'Red' then 'Kirmizi'
	when p.Color = 'Blue' then 'Mavi'
	when p.Color = 'Black' then 'Siyah'
	when p.Color = 'Silver' then 'Gümüs'
	when p.Color = 'Green' then 'Yesil'
	when p.Color = 'Yellow' then 'Sari'
	else 'Renksiz'
end as Renk,
SUM(sod.UnitPrice)
from Production.Product p
	INNER JOIN Sales.SalesOrderDetail sod
			ON sod.ProductID = p.ProductID
group by p.ProductID, p.Color
having SUM(sod.UnitPrice) > 2000
```
