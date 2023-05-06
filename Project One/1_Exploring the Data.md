# Case study Eniac

You have to answer two main **questions**:

1. Eniac’s catalogue is 100% tech products and heavily based on Apple-compatible accessories. **It is not clear that the marketplaces Magist works with are a good place for these high-end tech products**.
2. Among Eniac’s efforts to have happy customers, fast deliveries are key. The delivery fees resulting from Magist’s deal with the public Post Office might be cheap, but at what cost? **Are deliveries fast enough?**

###  

## Explore the tables

1. How many orders are there in the dataset? 

   ```SQL
   USE magist;
   SELECT 
       COUNT(DISTINCT order_id)
   FROM
       orders; 
   ```

   ```c
   '99441'
   ```

   

2. Are orders actually delivered? 

   ```SQL
   SELECT DISTINCT
       order_status, COUNT(order_status)
   FROM
       orders
   GROUP BY order_status
   ```

   ```c
   'delivered'  	,'96478'
   'unavailable'	,'609'
   'shipped'			,'1107'
   'canceled'		,'625'
   'invoiced'		,'314'
   'processing'	,'301'
   'approved'		,'2'
   'created'			,'5'
   ```

   

3. Is Magist having user growth? 

   ```sql
   SELECT 
       YEAR(order_purchase_timestamp) AS years,
       MONTH(order_purchase_timestamp) AS months,
         COUNT(order_id)
   FROM
       orders
   GROUP BY years , months
   ORDER BY years , months
   ```

   ```c
   '2016','9','4'
   '2016','10','324'
   '2016','12','1'
   '2017','1','798'
   '2017','2','1780'
   '2017','3','2678'
   '2017','4','2402'
   '2017','5','3695'
   '2017','6','3256'
   '2017','7','4013'
   '2017','8','4337'
   '2017','9','4285'
   '2017','10','4636'
   '2017','11','7538'
   '2017','12','5681'
   '2018','1','7254'
   '2018','2','6735'
   '2018','3','7212'
   '2018','4','6930'
   '2018','5','6872'
   '2018','6','6176'
   '2018','7','6265'
   '2018','8','6549'
   '2018','9','16'
   '2018','10','4'
   
   ```

4. How many products are there on the products table?

   ```sql
   SELECT 
       COUNT(DISTINCT product_id)
   FROM
       products
   ```

   ```c
   '32951'
   ```

5. Which are the categories with the most `products`? 

   ```sql
   SELECT 
       product_category_name_english, COUNT(product_id)
   FROM
       products
           LEFT JOIN
       product_category_name_translation ON product_category_name_translation.product_category_name = products.product_category_name
   GROUP BY product_category_name_english
   ORDER BY COUNT(product_id) DESC
   ```

   ```c
   'bed_bath_table','3029'
   'sports_leisure','2867'
   'furniture_decor','2657'
   'health_beauty','2444'
   'housewares','2335'
   'auto','1900'
   'computers_accessories','1639'
   'toys','1411'
   'watches_gifts','1329'
   'telephony','1134'
   'baby','919'
   'perfumery','868'
   'fashion_bags_accessories','849'
   'stationery','849'
   'cool_stuff','789'
   'garden_tools','753'
   'pet_shop','719'
   'others','610'
   'electronics','517'
   'construction_tools_construction','400'
   'home_appliances','370'
   'luggage_accessories','349'
   'consoles_games','317'
   'office_furniture','309'
   'musical_instruments','289'
   'small_appliances','231'
   'home_construction','225'
   'books_general_interest','216'
   'fashion_shoes','173'
   'furniture_living_room','156'
   'air_conditioning','124'
   'books_technical','123'
   'fixed_telephony','116'
   'home_confort','111'
   'food_drink','104'
   'market_place','104'
   'fashion_male_clothing','95'
   'kitchen_dining_laundry_garden_furniture','94'
   'signaling_and_security','93'
   'construction_tools_safety','91'
   'home_appliances_2','90'
   'costruction_tools_garden','88'
   'food','82'
   'drinks','81'
   'construction_tools_lights','78'
   'agro_industry_and_commerce','74'
   'industry_commerce_and_business','68'
   'christmas_supplies','65'
   'audio','58'
   'art','55'
   'fashion_underwear_beach','53'
   'dvds_blu_ray','48'
   'furniture_bedroom','45'
   'costruction_tools_tools','39'
   'books_imported','31'
   'small_appliances_home_oven_and_coffee','31'
   'computers','30'
   'cine_photo','28'
   'fashio_female_clothing','27'
   'music','27'
   'party_supplies','26'
   'fashion_sport','19'
   'arts_and_craftmanship','19'
   'flowers','14'
   'diapers_and_hygiene','12'
   'la_cuisine','10'
   'furniture_mattress_and_upholstery','10'
   'portable_kitchen_food_processors','10'
   'tablets_printing_image','9'
   'home_comfort_2','5'
   'fashion_childrens_clothes','5'
   'pc_gamer','3'
   'security_and_services','2'
   'cds_dvds_musicals','1'
   ```

6. How many of those products were present in actual transactions? The `products` table is a “reference” of all the available products. Have all these products been involved in orders? Check out the `order_items` table to find out!

   ```sql
   Use magist;
   SELECT 
       MAX(order_items.price), MIN(order_items.price)
   FROM
       order_items
           INNER JOIN
       products ON order_items.product_id = products.product_id
   
   ```

   ```c
   '32951'
   ```

7. What’s the price for the most expensive and cheapest products? Sometimes, having a basing range of prices is informative. Looking for the maximum and minimum values is also a good way to detect extreme outliers.

   ```SQL
   Use magist;
   SELECT 
       MAX(order_items.price), MIN(order_items.price)
   FROM
       order_items
           INNER JOIN
       products ON order_items.product_id = products.product_id
   ```

   ```c
   '6735', '0.85'
   ```

8. What are the highest and lowest payment values? Some orders contain multiple products. What’s the highest someone has paid for an order? Look at the `order_payments table` and try to find it out.

   ```sql
   SELECT 
       MAX(payment_value), MIN(payment_value)
   FROM
       order_payments
   ```

   ```c
   '13664.1', '0'
   ```

   

