# sql-server

* [example_01](example_01)
* [example_02](example_02)

# View

Viewleri belirli tablolardan istediğimiz columnlarını alarak kendi oluşturduğumuz sanal tablolara denir

Viewlere sanal tablolar denilebilir. Bundan dolayı viewlerde aynı isme ait column bulunamaz ve isimsiz column olamaz.

Viewlere 'v' harfi ile başlaması tavsiye edilir. Böylece sql ile uğraşan diğer takım arkdaşlarımız bunu anlayabilir.

Viewlere gösterme amaçlı olduklarından update, delete, insert islemleri yapılamamaktadır.

Şu Şekilde oluşturulur.

```sql
create view view_name as select col_name, .... from table_name where col_name = your_condition ...
```

# Stored Procedure

Türkçesi saklı yordamlar olarak çevrilebilir.

Fonksiyon gibi dışardan değer alınarak veya alınmayarak çalışırlar. Ve çalıştırıldıklarında 1 kere derlenir ve çalıştırılır.

Fonksiyonlardan farkı değer döndürmemesidir.

Insert, update, delete yapılabilmektedir.

Saklı yordam isimleri usp, sp başlanması tavsiye edilir. Böylelikle diğer takım arkadaşlarımız bunun saklı yordam olduğunu kolaylıkla anlar.

Şu şekilde oluştururlurlar

```sql
create procedure procedure_name(@param valtype, ...)
as
begin
  --işlemler
end
```

Ve şu şekilde çalıştırırlar

```sql
 exec procedure_name
```


# Func

Saklı yordamlardan farkı değer döndürebiliyor olmasıdır.

Bunları çağırırken schema_adi.function_name olarak çağrılması tavsiye edilir.

Fonksiyon isimleri ufn, fn başlanması tavsiye edilir.

3 Tip Func tipi oluşturma şekli vardır. Bunlar deger donduren, sorgu donduren ve tablo donduren şeklindedir.

Deger donduren şu şekilde tanımlanır.

```sql
create function function_name(@param valtype, ....)
returns valtype
begin
  -- işlemler
  return val
end
```

Sorgu döndürenler ise şu şekilde tanımlanır

```sql
create function func_name(@param valtype, ....)
returns table
as 
return select col_name, ... from table_name where col_name = condition ...
```

Tablo döndürenler ise su şekilde tanımlanır

```sql
CREATE FUNCTION fonksiyon_adi
(
  -- Parametrelerin eklendiği yer
  @param1 veritürü,
  @param2 veritürü
)
RETURNS
@tablo TABLE
(
  -- colonları buraya yazıyoruz
  colon1 veritürü,
  colon2 veritürü
)
AS
BEGIN
  -- Tablo değişkenin içini girilen parametreler ile doldur.
  RETURN
END
```

