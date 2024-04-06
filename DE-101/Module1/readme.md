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
- **скомпоновать полученные отчеты и визуализации в единый дашборд**.

## Объединение данных.
Данные с 3-х листов объединим в 1 таблицу на листе *Orders*, т.к. он содержит больше всего информации и имеет общие поля с двумя другими листами - поле "Region" с листом *People* , поле "Order ID" с листом *Returns*.
1. Оформляем данные на листе *Orders* как таблицу используя команду `"Форматировать как таблицу"`, в меню на вкладке `"Конструктор таблиц"` задаем имя таблицы - *Data*. 

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

## Создание отчетов и визуализаций.
Для создания отчета(в виде сводной таблице) на пустом листе используем вкладку `Вставка` -> `Сводная таблица`, в качестве диапазона указываем созданную ранее таблицу *Data*

![image](https://github.com/papchukev/datalearn/assets/149643273/0a1ae589-5de9-43fe-8e8e-66d632146965)

Далее в меню `Поля сводной таблицы` добавляем в список `Строки` поля,по которым будет происходить агрегация, а в список `Значения` - поля с числовыми значениями, которые будут агрегироваться

![image](https://github.com/papchukev/datalearn/assets/149643273/82cde7d6-caf2-41f8-9465-c71cd8c6ff2a)

Выделяем полученную сводную таблицу и используя в меню команду `Сводная диаграмма` строим подходящую визуализацию

![image](https://github.com/papchukev/datalearn/assets/149643273/05c454af-67bb-4cb5-9612-ac8bcd2235b7)

Получаем сводную таблицу с данными о продажах и прибыли по месяцам/годам + график динамики продаж и прибыли

![image](https://github.com/papchukev/datalearn/assets/149643273/a007add3-5e16-4891-9283-5b7bb51ebe4c)


Повторяем эти же шаги, выбирая поля, по которым агрегируются значения и поля с числовыми значениями, для создания необходимых отчетов

<details> 
  <summary> прибыль/продажи по категориям и подкатегориям </summary>
  
  ![image](https://github.com/papchukev/datalearn/assets/149643273/95f81d48-aa6c-4307-89cc-dec964d5ed64)  
</details>

<details> 
  <summary> прибыль/продажи по региональным менеджерам </summary>

  ![image](https://github.com/papchukev/datalearn/assets/149643273/d8257dd0-d16e-44b7-bccb-0d591982d07d)
</details>

<details> 
  <summary> динамика продаж по сегментам </summary>

   ![image](https://github.com/papchukev/datalearn/assets/149643273/4524da17-ce2c-448b-a1e3-99e955c2040d) 
</details>

<details> 
  <summary> прибыль/продажи по сегментам </summary>

  ![image](https://github.com/papchukev/datalearn/assets/149643273/f86a8911-0f42-4b8f-9a45-912bd74bedef)  
</details>

<details> 
  <summary> продажи по регионам </summary>

  ![image](https://github.com/papchukev/datalearn/assets/149643273/88bb207b-b3a6-4392-b715-163285051406)  
</details>

<details> 
  <summary> возвраты </summary>

  ![image](https://github.com/papchukev/datalearn/assets/149643273/235e95bc-8df1-4f8b-881e-d95522060799)  
</details>

<details> 
  <summary> основные метрики </summary>

  ![image](https://github.com/papchukev/datalearn/assets/149643273/d68e3fd2-9d05-4439-a14f-fd195398883c)  
</details>

## Дашборд
На пустой лист копируем созданные ранее графики и диаграммы, добавляя срезы по интересующим нас полям

![image](https://github.com/papchukev/datalearn/assets/149643273/6f5c9140-5986-44f5-90ad-51cc8a513d2b)

Для основных метрик добавляем спарклайны (меню `Вставка` -> раздел Спарклайны -> `График`), в качестве источника данных выбираем созданный ранее отчет с основными метриками

![image](https://github.com/papchukev/datalearn/assets/149643273/a6b2ab91-f7b1-4533-8c49-dc9dd0166527)

![image](https://github.com/papchukev/datalearn/assets/149643273/3de41d87-d642-4ba5-b36d-7f369787ecd5)

Подключаем созданные срезы ко всем диаграммам на дашборде

![image](https://github.com/papchukev/datalearn/assets/149643273/9dc0d506-90aa-49b3-a65e-706e79c87d88)

Финальный дашборд

![image](https://github.com/papchukev/datalearn/assets/149643273/0cfa515b-d5de-44e0-a43e-227a797445f0)
