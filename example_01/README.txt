#ORN 1
select * from Person.Person p
	INNER JOIN Person.PersonPhone pp
			ON pp.BusinessEntityID = p.BusinessEntityID
WHERE p.FirstName like '%a%' and pp.PhoneNumber like '212-555%'

#ORN 2

##Çözüm 1
SELECT st.countryregioncode, 
       Count(*) as TOPLAM
FROM   sales.salesorderheader soh 
       INNER JOIN sales.salesterritory st 
               ON st.territoryid = soh.territoryid 
WHERE  EXISTS (SELECT sod.salesorderid 
                            FROM   sales.salesorderdetail sod 
                                   INNER JOIN production.product p 
                                           ON p.productid = sod.productid 
                            WHERE soh.SalesOrderID = sod.SalesOrderID and  p.NAME LIKE '%z%') 
GROUP  BY st.countryregioncode;


##Çözüm 2

SELECT st.countryregioncode, 
       Count(*) as TOPLAM
FROM   sales.salesorderheader soh 
       INNER JOIN sales.salesterritory st 
               ON st.territoryid = soh.territoryid 
WHERE  soh.salesorderid IN (SELECT sod.salesorderid 
                            FROM   sales.salesorderdetail sod 
                                   INNER JOIN production.product p 
                                           ON p.productid = sod.productid 
                            WHERE  p.NAME LIKE '%z%') 
GROUP  BY st.countryregioncode;
