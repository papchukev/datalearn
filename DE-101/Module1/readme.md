# Аналитика в Excel
## Исходные данные.
Исходные данные - файл *Sample-Superstore.xls* содержащий 3 листа: *Orders* - данные о заказах, *People* - данные о региональных менеджерах, *Returns* - данные о возвратах.
<details>
  <summary>Значения атрибутов; данные</summary>
  
|Название столбца|	Значение   |
|-----------     |-----------  |
|Row ID	         |Идентификатор строки (уникальный)|
|Order ID	       |Идентификатор заказа|
|Order Date	     |Дата заказа|
|Ship Date	     |Дата доставки|
|Ship Mode       |Класс доставки|
|Customer ID	   |Идентификатор покупателя|
|Customer Name	 |Имя и фамилия покупателя|
|Segment	       |Сегмент покупателя|
|Country	       |Страна|
|City	           |Город|
|State           |Штат|
|Postal Code     |Почтовый индекс|
|Region	         |Регион|
|Product ID	     |Идентификатор товара|
|Category	       |Категория|
|Sub-Category	   |Подкатегория|
|Product Name	   |Название товара|
|Sales           |Продажи (Доход)|
|Quantity	       |Количество|
|Discount	       |Скидка в %|
|Profit	         |Прибыль|
|Person          |Региональный менеджер|
|Returned        |Возвраты товара|

![image](https://github.com/papchukev/datalearn/assets/149643273/a709167a-f736-4a5d-a5bc-4e5714b06ee4)
![image](https://github.com/papchukev/datalearn/assets/149643273/3ad67b12-f91a-4948-8b03-c123a68ab7b7)
![image](https://github.com/papchukev/datalearn/assets/149643273/160d7fbc-56bc-46e6-9012-9c1404769b95)
</details>

**Используя данные *Sample-Superstore.xls* необходимо:**

- **создать примеры отчетов**(продажи и прибыль, динамика отдельных показателей, сравнение показателей по регионам и т.п.);
- **построить подходящую визуализацию для каждого отчета**;
- **скомпоновать полученные данные в единый дашборд**.

## Объединение данных.
Данные с 3-х листов объединим в 1 таблицу на листе *Orders*, т.к. он содержит больше всего информации и имеет общие поля с двумя другими листами - поле "Region" с листом *People* , поле "Order ID" с листом *Returns*.
1. Оформляем данные на листе *Orders* как таблицу используя команду `"Форматировать как таблицу"`. 

   <details> 
     <summary> показать </summary>
     
     ![image](https://github.com/papchukev/datalearn/assets/149643273/ce7e6830-af0d-46a0-85a2-24bc081f9f21)

   </details>

2. Добавляем в полученную таблицу столбцы "Person" и "Returned".

   <details> 
     <summary> показать </summary>
     
     ![image](https://github.com/papchukev/datalearn/assets/149643273/e0b398ad-7430-46ef-8be1-627ae1dc4e46)

   </details>
   
3. Заполняем новые столбцы "Person" и "Returned" данными из соответствующих листов:
  -  для "Person" используем функцию `ЕСЛИ()`, которая сравнивает значения полей "Region" в таблице *Orders* и *People*, и возвращает соответствующего региону менеджера из *People* (4 региона -> 4 менеджера -> 4 условия):
`=ЕСЛИ(M2=People!$B$5;People!$A$5;ЕСЛИ(M2=People!$B$4;People!$A$4;ЕСЛИ(M2=People!$B$3;People!$A$3;ЕСЛИ(M2=People!$B$2;People!$A$2;0))))`
  - для "Returned" используем комбинацию функции `ЕСЛИОШИБКА()` + `ВПР()`, в результате работы которых в случае если значение поля "OrderID" таблицы *Orders* будет найдено в том же поле из листа с данными о возвратах (*Returns*) вернется значение "Yes", иначе "No" :    
`=ЕСЛИОШИБКА(ВПР(Orders!$B2;Returns!$B:$C;2;ЛОЖЬ);"No")`
  
      <details> 
         <summary> показать </summary>
        для корректной работы функции ВПР() столбец с возвращаемым значением должен находится справа от столбца с искомым значением, поэтому меняем столбцы местами на листе *Returns*.
        
    ![image](https://github.com/papchukev/datalearn/assets/149643273/874c4957-6c58-43c7-aa5e-f530f8107108)   
    ![image](https://github.com/papchukev/datalearn/assets/149643273/5ea3b41f-ab20-4eae-ad4f-99a92eeb6edd)
    ![image](https://github.com/papchukev/datalearn/assets/149643273/2716f396-f59f-422f-a467-53d4e3a6ccf8)
    
      </details>  

