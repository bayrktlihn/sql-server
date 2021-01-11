# ORN 1 Soru

person.person tablosunda title kolonuna göre MR için Erkek, Miss için Bekar Kadın, Mrs için Evli kadın ve Ms için Kadın yazsın bunlar haricindeki değerlere belirsiz yazılsın

# ORN 2 Soru

CountryRegionCode US olmayan SubTotal 30000'den küçük olan ProductID 100den büyük  rengi null olan  ve UnitPrice * OrderQty(Toplam) toplamı 5000den büyük olan ürünlerin adını ve satış toplamını

# ORN 3 Soru

Kırmızı renkli en büyük idli ürünün UnitPrice değeri 1000den büyük olan satışlara ait  SalesOrderHeader bilgileri gelsin

# ORN 4 Soru

Not Exists-Product tablosunda ListPrice değeri 100den küçük olan ürünlerin içinde olmayan ürünlerin SalesOrderDetail tablosunda UnitPrice ortalamasının 2000den büyük olduğu bilgileri büyükten küçüğe sıralanarak gelsin


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
		sum(sod.UnitPrice * sod.OrderQty) as toplam
from Production.Product p
		INNER JOIN Sales.SalesOrderDetail sod
				ON sod.ProductID = p.ProductID
where p.ProductID > 100 
		and p.Color is null
		and sod.SalesOrderID IN (select	soh.SalesOrderID
								from	Sales.SalesOrderHeader soh
										INNER JOIN Sales.SalesTerritory st
												ON st.TerritoryID = soh.TerritoryID
								where soh.SubTotal < 30000
										and not st.CountryRegionCode = 'US')
group by p.Name
having SUM(sod.UnitPrice * sod.OrderQty) > 5000
order by toplam desc
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
		avg(UnitPrice) Average
from	Sales.SalesOrderDetail sod 
where	not exists(select	*
					from	Production.Product p
					where	p.ListPrice < 100 
							and sod.ProductID = p.ProductID)
group by sod.ProductID
having AVG(UnitPrice) > 2000
order by AVG(UnitPrice) desc;
```
