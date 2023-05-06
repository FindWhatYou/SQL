# Case study Eniac

You have to answer two main **questions**:

1. Eniac’s catalogue is 100% tech products and heavily based on Apple-compatible accessories. **It is not clear that the marketplaces Magist works with are a good place for these high-end tech products**.

2. Among Eniac’s efforts to have happy customers, fast deliveries are key. The delivery fees resulting from Magist’s deal with the public Post Office might be cheap, but at what cost? **Are deliveries fast enough?**

   

## **Answer business questions**

1. In relation to the product: 
   1. What categories of tech products does Magist have?

      ```sql
      USE magist;
      
      SELECT 
          *
      FROM
          product_category_name_translation
      WHERE
          product_category_name_english IN (
      				'computers_accessories' , 
              'telephony',
              'electronics',
              'consoles_games',
              'fixed_telephony',
              'audio',
              'computers',
              'cine_photo');
      ```
   
      ```
      'audio'
      'cine_photo'
      'computers'
      'computers_accessories'
      'consoles_games'
      'electronics'
      'fixed_telephony'
      'telephony'
      ```
   
   2. How many products of these tech categories have been sold (within the time window of the database snapshot)? What percentage does that represent from the overall number of products sold?
   
      ```sql
      SELECT 
          product_category_name_english,
          COUNT(order_items.product_id) AS prd_count_sold
      FROM
          order_items
              LEFT JOIN
          products ON order_items.product_id = products.product_id
              LEFT JOIN
          product_category_name_translation ON products.product_category_name = product_category_name_translation.product_category_name
      WHERE
          product_category_name_english IN ('computers_accessories' , 'telephony',
              'electronics',
              'consoles_games',
              'fixed_telephony',
              'audio',
              'computers',
              'cine_photo')
      GROUP BY product_category_name_english
      ORDER BY prd_count_sold DESC;
      ```
   
      ```c
      'computers_accessories','1639'
      'telephony','1134'
      'electronics','517'
      'consoles_games','317'
      'fixed_telephony','116'
      'audio','58'
      'computers','30'
      'cine_photo','28'
      ```
      
      All product sold:
      
      ```sql
      USE magist;
      SELECT 
          COUNT(distinct product_id)
      FROM
          order_items;
      ```
      
      ```c
      '32951'
      ```
      
      Products from the tech categories:
      
      ```sql
      SELECT 
          count(product_id)
      FROM
          product_category_name_translation
              LEFT JOIN
          products ON products.product_category_name = product_category_name_translation.product_category_name
      WHERE
          product_category_name_english IN ('computers_accessories' , 'telephony',
              'electronics',
              'consoles_games',
              'fixed_telephony',
              'audio',
              'computers',
              'cine_photo');
       
      ```
      
      ```c
      '3839'
      ```
      Result:
      
      ```sql
      SELECT ROUND(3839 / 32951 * 100)
      ```
      
      ```c
      '12'
      ```
      
   3. What’s the average price of the products being sold?
   
      ```sql
      SELECT 
         round(avg(price))
      FROM
          order_items;
      ```
   
      ```c
      '121'
      ```
   
   4. Are expensive tech products popular? 
   
      ```sql
      SELECT 
          product_category_name_english, count(products.product_id) as count_number_prod, round(avg(price))
      FROM
          product_category_name_translation
              LEFT JOIN products 
              ON products.product_category_name = product_category_name_translation.product_category_name
              LEFT JOIN order_items
      		ON products.product_id = order_items.product_id
      WHERE
          product_category_name_english IN ('computers_accessories' , 'telephony',
              'electronics',
              'consoles_games',
              'fixed_telephony',
              'audio',
              'computers',
              'cine_photo')
      GROUP BY product_category_name_english
      ORDER BY count_number_prod desc;
      ```
   
      ```c
      'product_category_name_english','count_number_prod','Average_price_categories '
      
      'computers_accessories','7827','117'
      'telephony','4545','71'
      'electronics','2767','58'
      'consoles_games','1137','138'
      'audio','364','139'
      'fixed_telephony','264','226'
      'computers','203','1098'
      'cine_photo','72','96'
      ```
   
      
   
2. In relation to the seller:

   1. How many months of data are included in the magist database?

      ```sql
      SELECT 
          COUNT(DISTINCT YEAR(order_purchase_timestamp),
              MONTH(order_purchase_timestamp)) AS num_rows
      FROM
          orders
      ```

      ```c
      '25'
      ```
   
   2. How many sellers are there? How many Tech sellers are there? What percentage of overall sellers are Tech sellers?

      ```sql
      SELECT 
          COUNT(seller_id)
      FROM
          sellers
      ```
   
      All sellers:

      ```c
      '3095'
      ```
   
      ```sql
      SELECT 
          COUNT(DISTINCT sellers.seller_id)
      FROM
          sellers
              LEFT JOIN
          order_items ON order_items.seller_id = sellers.seller_id
              LEFT JOIN
          products ON order_items.product_id = products.product_id
              LEFT JOIN
          product_category_name_translation ON product_category_name_translation.product_category_name = products.product_category_name
      WHERE
          product_category_name_english IN ('computers_accessories' , 'telephony',
              'electronics',
              'consoles_games',
              'fixed_telephony',
              'audio',
              'computers',
              'cine_photo');
      ```
   
      Seller with Tech:

      ```c
      '492'
      ```
   
      ```sql
      SELECT ROUND((492 / 3095) * 100)  
      ```
   
      Percentage of overall sellers are Tech sellers:

      ```sql
      '16'
      ```
   
   3. What is the total amount earned(?) by all sellers? What is the total amount earned by all Tech sellers?

      ```sql
      SELECT 
          ROUND(SUM(price)) AS sum_price
      FROM
          sellers
              LEFT JOIN
          order_items ON order_items.seller_id = sellers.seller_id
              LEFT JOIN
          products ON order_items.product_id = products.product_id
              LEFT JOIN
          product_category_name_translation ON product_category_name_translation.product_category_name = products.product_category_name
      ```
   
      ```c
      '13591644'
      ```
   
      Price and Delivery:

      ```sql
      SELECT 
          ROUND(SUM(payment_value))
      FROM
          order_payments
      ```
   
      ```c
      '16008872'
      ```
   
      ```sql
      SELECT 
          ROUND(SUM(price)) AS sum_price
      FROM
          sellers
              LEFT JOIN
          order_items ON order_items.seller_id = sellers.seller_id
              LEFT JOIN
          products ON order_items.product_id = products.product_id
              LEFT JOIN
          product_category_name_translation ON product_category_name_translation.product_category_name = products.product_category_name
          WHERE
          product_category_name_english IN ('computers_accessories' , 'telephony',
              'electronics',
              'consoles_games',
              'fixed_telephony',
              'audio',
              'computers',
              'cine_photo');
      ```
   
      ```c
      '1893502'
      ```
   
   4. Can you work out the average monthly income(?) of all sellers? Can you work out the average monthly income of Tech sellers?
   
      Version 1:
   
      ```sql
      SELECT
      year(order_purchase_timestamp) as years, round(sum(price)/12) as avr_month_price
      FROM
          sellers
              LEFT JOIN
          order_items ON order_items.seller_id = sellers.seller_id
              LEFT JOIN
          products ON order_items.product_id = products.product_id
              LEFT JOIN
          product_category_name_translation ON product_category_name_translation.product_category_name = products.product_category_name
      		left join
      	orders on orders.order_id = order_items.order_id
      group by years
      order by years;
      
      ```
   
      ```c
      '2016','4149'
      '2017','512972'
      '2018','615516'
      
      ```
   
      ```sql
      SELECT 
          YEAR(order_purchase_timestamp) AS years,
          ROUND(SUM(price) / 12) AS avr_month_price
      FROM
          sellers
              LEFT JOIN
          order_items ON order_items.seller_id = sellers.seller_id
              LEFT JOIN
          products ON order_items.product_id = products.product_id
              LEFT JOIN
          product_category_name_translation ON product_category_name_translation.product_category_name = products.product_category_name
              LEFT JOIN
          orders ON orders.order_id = order_items.order_id
      WHERE
          product_category_name_english IN ('computers_accessories' , 'telephony',
              'electronics',
              'consoles_games',
              'fixed_telephony',
              'audio',
              'computers',
              'cine_photo')
      GROUP BY years
      ORDER BY years;
      ```
   
      ```c
      '2016','668'
      '2017','75014'
      '2018','82110'
      ```
   
      Version 2:
   
      ```sql
      Use magist;
      
      select round(avg(avr_month_price)) from (
      SELECT
      extract(year_month from order_purchase_timestamp) as years, round(sum(price)) as avr_month_price
      FROM
          sellers
              LEFT JOIN
          order_items ON order_items.seller_id = sellers.seller_id
              LEFT JOIN
          products ON order_items.product_id = products.product_id
              LEFT JOIN
          product_category_name_translation ON product_category_name_translation.product_category_name = products.product_category_name
      		left join
      	orders on orders.order_id = order_items.order_id
      group by years
      order by years) as tm1 ;
      ```
   
      ```c
      '566319'
      ```
   
      ```sql
      Use magist;
      
      SELECT 
          ROUND(AVG(avr_month_price))
      FROM
          (SELECT 
              EXTRACT(YEAR_MONTH FROM order_purchase_timestamp) AS years,
                  ROUND(SUM(price)) AS avr_month_price
          FROM
              sellers
          LEFT JOIN order_items ON order_items.seller_id = sellers.seller_id
          LEFT JOIN products ON order_items.product_id = products.product_id
          LEFT JOIN product_category_name_translation ON product_category_name_translation.product_category_name = products.product_category_name
          LEFT JOIN orders ON orders.order_id = order_items.order_id
          WHERE
              product_category_name_english IN ('computers_accessories' , 'telephony', 'electronics', 'consoles_games', 'fixed_telephony', 'audio', 'computers', 'cine_photo')
          GROUP BY years
          ORDER BY years) AS tm1;
      ```
   
      ```c
      '86068'
      ```
   
3. In relation to the delivery time:

   1. What’s the average time between the order being placed and the product being delivered?

      ```sql
      SELECT 
          TIMESTAMPDIFF(order_delivered_customer_date, order_purchase_timestamp)
      FROM
          orders;
      ```
      
      ```c
      '13' //days
      ```
      
   2. How many orders are delivered on time vs orders delivered with a delay?
   
      ```sql
      SELECT 
          CASE
              WHEN
                  DATEDIFF(order_estimated_delivery_date,
                          order_purchase_timestamp) - DATEDIFF(order_delivered_customer_date,
                          order_purchase_timestamp) > 0
              THEN
                  'The Delivery is on time'
              ELSE 'Not on Time'
          END AS delay_in_delivery,
      	COUNT(*) AS how_many
      FROM
          orders
      GROUP BY delay_in_delivery
      ```
   
      ```c
      'The Delivery is on time','88476'
      'Not on Time','10965'
      ```
   
      ```sql
      select round(((10965/88479))*100)
      ```
   
      ```c
      '12'
      ```
   
   3. Is there any pattern for delayed orders, e.g. big products being delayed more often?
   
      ```sql
      SELECT 
          product_weight_g,
          product_length_cm,
          CASE
              WHEN
                  DATEDIFF(order_estimated_delivery_date,
                          order_purchase_timestamp) - DATEDIFF(order_delivered_customer_date,
                          order_purchase_timestamp) > 0
              THEN
                  'The Delivery is on time'
              ELSE 'Not on Time'
          END AS delay_in_delivery,
          DATEDIFF(order_estimated_delivery_date,
                  order_purchase_timestamp) - DATEDIFF(order_delivered_customer_date,
                  order_purchase_timestamp) AS Diff
      FROM
          sellers
              LEFT JOIN
          order_items ON order_items.seller_id = sellers.seller_id
              LEFT JOIN
          products ON order_items.product_id = products.product_id
              LEFT JOIN
          product_category_name_translation ON product_category_name_translation.product_category_name = products.product_category_name
              LEFT JOIN
          orders ON orders.order_id = order_items.order_id
      ORDER BY product_weight_g DESC;
      ```
   
      ```c
      'not lenght not weight_g'
      ```
   
   
   

