# SQL_Music_Store_Analysis
![image](https://github.com/Bishalg27/SQL_Music_Store_Analysis/assets/107564589/639fc65c-9fd0-4003-a576-0c2fab8c9d43)


## Introduction
In their pursuit of business growth, the music store has raised essential questions. How can they expand their customer base and increase sales? What strategies can they employ to enhance their online presence? These are common concerns, and addressing them requires a blend of traditional wisdom and modern innovation. 

In the next part, I'll give straightforward answers to these questions. I'll keep it simple and practical, offering easy-to-implement solutions for the music store's growth and success. Let's dive in!

### Question Set : Easy



### Question 1 : 
  **Who is the senior most employee based on job title?**

  **SOLUTION**
  ```
  SELECT title, last_name, first_name FROM employee
ORDER BY levels DESC
LIMIT 1
```

### Question 2 : 
  **Which countries have the most Invoices?**

  **SOLUTION**
  
  ```
 SELECT COUNT(*) AS c, billing_country 
FROM invoice
GROUP BY billing_country
ORDER BY c DESC

```

### Question 3 : 
  **What are top 3 values of total invoice?**

  **SOLUTION**
  
  ```
SELECT total 
FROM invoice
ORDER BY total DESC
```

### Question 4 : 
  **Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. Write a query that returns one city that has the highest sum of invoice totals. Return both the city name & sum of all invoice totals**

  **SOLUTION**
  
  ```
SELECT billing_city,SUM(total) AS InvoiceTotal
FROM invoice
GROUP BY billing_city
ORDER BY InvoiceTotal DESC
LIMIT 1;

```

### Question 5 : 
  **Who is the best customer? The customer who has spent the most money will be declared the best customer. Write a query that returns the person who has spent the most money**

  **SOLUTION**
  
  ```
SELECT customer.customer_id, first_name, last_name, SUM(total) AS total_spending
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id
GROUP BY customer.customer_id
ORDER BY total_spending DESC
LIMIT 1;
```

### Question Set 2 : Moderate

### Question 6 : 

**Write query to return the email, first name, last name, & Genre of all Rock Music listeners. Return your list ordered alphabetically by email starting with A**

```
SELECT DISTINCT email,first_name, last_name
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id
JOIN invoiceline ON invoice.invoice_id = invoiceline.invoice_id
WHERE track_id IN
(SELECT track_id FROM track
	JOIN genre ON track.genre_id = genre.genre_id
	WHERE genre.name LIKE 'Rock')
ORDER BY email;
```

### Question 7 : 

**Let's invite the artists who have written the most rock music in our dataset. Write a query that returns the Artist name and total track count of the top 10 rock bands**

```
select distinct art.name,count(tr.track_id) x from artist art join album2 alb
on art.artist_id = alb.artist_id
join track tr on alb.album_id = tr.album_id
join genre on genre.genre_id = tr.genre_id
where genre.name = "Rock"
group by art.name
order by x desc;

```


### Question 8 : 

**Return all the track names with a song length longer than the average. Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first.**

```
select name,milliseconds from track
where milliseconds >
(select avg(milliseconds) from track)
order by milliseconds desc

```



### Question Set : Advance 
### Question 9 : 


**Find how much amount spent by each customer on artists? Write a query to return customer name, artist name and total spent**

```
WITH best_selling_artist AS (
	SELECT artist.artist_id AS artist_id, artist.name AS artist_name, SUM(invoice_line.unit_price*invoice_line.quantity) AS total_sales
	FROM invoice_line
	JOIN track ON track.track_id = invoice_line.track_id
	JOIN album2 ON album2.album_id = track.album_id
	JOIN artist ON artist.artist_id = album2.artist_id
	GROUP BY 1,2
	ORDER BY 3 DESC
	LIMIT 1
)
SELECT c.customer_id, c.first_name, c.last_name, bsa.artist_name, SUM(il.unit_price*il.quantity) AS amount_spent
FROM invoice i
JOIN customer c ON c.customer_id = i.customer_id
JOIN invoice_line il ON il.invoice_id = i.invoice_id
JOIN track t ON t.track_id = il.track_id
JOIN album2 alb ON alb.album_id = t.album_id
JOIN best_selling_artist bsa ON bsa.artist_id = alb.artist_id
GROUP BY 1,2,3,4
ORDER BY 5 DESC;


```

### Question 10 : 

**: We want to find out the most popular music Genre for each country. We determine the most popular genre as the genre with the highest amount of purchases. Write a query that returns each country along with the top Genre. For countries where the maximum number of purchases is shared return all Genres.**

```
select * FROM 
(select customer.country country ,g.name Genre, count(inl.quantity) purchase,
row_number() over (partition by customer.country order by count(inl.quantity) DESC) Ranks
  from invoice inv 
join invoice_line inl on inv.invoice_id = inl.invoice_id
JOIN customer ON customer.customer_id = inv.customer_id
join track tr on inl.track_id = tr.track_id
join genre g on tr.genre_id = g.genre_id
group by 1,2 
order by 3 desc) z
where Ranks = 1
order by 1 asc,3 desc


```


### Question 11 : 

****Write a query that determines the customer that has spent the most on music for each country. Write a query that returns the country along with the top customer and how much they spent. For countries where the top amount spent is shared, provide all customers who spent this amount****

```
select * FROM 
(SELECT c.country Country ,c.customer_id ID ,first_name Name ,round(sum(quantity*unit_price),0) spent,
row_number () over (partition by c.country order by sum(quantity*unit_price) desc) rw
 FROM customer c
join invoice inv on c.customer_id = inv.customer_id
join invoice_line inl on inv.invoice_id = inl.invoice_id
group by c.country,c.customer_id,first_name
order by country ) z
where rw = 1 
order by 1,4 desc


```


## Learnings : 

``` Data Analyzing ```


This project helped me learn how to analyze data and answer questions using it. I gained important skills in understanding information, finding patterns, and solving problems. It boosted my confidence in dealing with data-related challenges, making me more skilled in making informed decisions.


## Files Information : 
- Database : It contains the excel files of the Music Shop.
- Music Store Analysis-Questions.docx : It contains the question that I tried to solved.
- Schema : It shows the relation among the tables in the *Database*

## License : 
For questions or feedback, please contact: vishugoswami27@gmail.com

Enjoy exploring the project!












