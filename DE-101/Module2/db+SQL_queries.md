# Создание БД на основе Superstore.xls ([module 1](https://github.com/papchukev/datalearn/tree/main/DE-101/Module1))

<details> 
  <summary> Устанавливаем СУБД PostgreSQL </summary>
  
  ![изображение](https://github.com/papchukev/datalearn/assets/149643273/d0202b89-a25a-4b30-8b6f-aa50ce3ad9da)     
</details>

<details>
  <summary> Подключаемся к созданной бд с помощью DBeaver </summary>
  
  ![изображение](https://github.com/papchukev/datalearn/assets/149643273/76e4f54e-396a-4742-89b4-842325732099)
  ![изображение](https://github.com/papchukev/datalearn/assets/149643273/72539fc2-66a7-496f-a76b-d35f13b4d37d)
  ![изображение](https://github.com/papchukev/datalearn/assets/149643273/0fefce70-f396-4c2d-baa4-b036c77220a0)

</details>

Теперь необходимо создать 3 таблицы для данных о заказах - *Orders*, о региональных менеджерах - *People* и о возвратах - *Returns*.

orders
![изображение](https://github.com/papchukev/datalearn/assets/149643273/d2c9c991-e62a-4af6-ad13-24d1647815c0)

people
![изображение](https://github.com/papchukev/datalearn/assets/149643273/593e3e4a-4232-420e-a7cf-9da16d45a731)

returns
![изображение](https://github.com/papchukev/datalearn/assets/149643273/7ef4a1bf-6474-415d-84ce-db50e06535bb)

Далее заполняем таблицы данными

*orders.sql*

![изображение](https://github.com/papchukev/datalearn/assets/149643273/601e8e3a-366a-4cc0-96c7-dd4e3a45fde5)

*people.sql*

![изображение](https://github.com/papchukev/datalearn/assets/149643273/76f2bfe7-092d-4ce3-b165-3c327dc2ff51)

*returns.sql*

![изображение](https://github.com/papchukev/datalearn/assets/149643273/9a92a9b3-de49-42b8-b933-f0135a54768a)

# Выборка данных из БД с помощью SQL запросов.

База данных на основе информации из Superstore.xls готова, а значит можно осуществлять выборку необходимых данных. Используя SQL запросы построим отчеты подобные тем, что были созданы в Excel в [модуле 1](https://github.com/papchukev/datalearn/tree/main/DE-101/Module1#%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BE%D1%82%D1%87%D0%B5%D1%82%D0%BE%D0%B2-%D0%B8-%D0%B2%D0%B8%D0%B7%D1%83%D0%B0%D0%BB%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D0%B9)

- продажи всего/за опр.период, продажи сгруппированные по годам

      select round(sum(sales), 2) as total_sales
      from orders o
      /*where extract('year' from order_date) = '2019'*/
      ;  
  
      select
          extract('year' from order_date) as year,
          round(sum(sales), 2) as total_sales_per_year
      from orders o
      group by extract('year' from order_date)
      ;
  <details> 
    <summary> показать </summary>
      
  ![изображение](https://github.com/papchukev/datalearn/assets/149643273/232c326c-06da-48c7-badf-66b1f5932176)
  ![изображение](https://github.com/papchukev/datalearn/assets/149643273/703472b0-6249-4e96-832b-6c62c8b00d91)
  </details>

- прибыль всего/за опр.период, прибыль сгруппированная по годам

      select round(sum(profit), 2) as total_profit
      from orders o
      /*where extract('year' from order_date) = '2019'*/
      ;

      select 
	      extract('year' from order_date) as year, 
	      round(sum(profit), 2) as total_profit_per_year
      from orders o 
      group by extract('year' from order_date)
      ;

  <details> 
    <summary> показать </summary>
      
  ![изображение](https://github.com/papchukev/datalearn/assets/149643273/76675236-3da3-4cc6-b8ee-935f60bf82aa)
  ![изображение](https://github.com/papchukev/datalearn/assets/149643273/d0870ed1-ef77-4361-92f8-ddd8339d8877)
  </details>

- норма прибыли(profit ratio) за все время/за опр.период, норма прибыли сгруппированная по годам

      select round(sum(profit)/sum(sales), 4)*100 as profit_ratio
      from orders o
      /*where extract('year' from order_date) = '2019'*/
      ;

      select 
	      extract('year' from order_date) as year, 
	      round(sum(profit)/sum(sales), 4)*100 as profit_ratio_per_year
      from orders o 
      group by extract('year' from order_date)
      ;

  <details> 
    <summary> показать </summary>
      
  ![изображение](https://github.com/papchukev/datalearn/assets/149643273/2b8639e0-3a9b-4533-80ba-9de8ae8d9e9e)
  ![изображение](https://github.com/papchukev/datalearn/assets/149643273/79b79028-02bb-4891-b8f8-5c4f8b2bd90e) 
  </details>

- средний размер скидки(average discount) за все время/за опр.период, средний размер скидки сгруппированный по годам

      select round(avg(discount), 4)*100 as avg_discount
      from orders o
      /*where extract('year' from order_date) = '2019'*/
      ;

      select 
	      extract('year' from order_date) as year, 
	      round(avg(discount), 4)*100 as avg_discount_per_year
      from orders o 
      group by extract('year' from order_date)
      ;

  <details> 
    <summary> показать </summary>
      
  ![изображение](https://github.com/papchukev/datalearn/assets/149643273/f15a143b-d628-4c9a-990b-02bdab05f33a)
  ![изображение](https://github.com/papchukev/datalearn/assets/149643273/88768746-ead0-4a48-8a38-a5b7286682c1) 
  </details>

