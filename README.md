
# Famous Paintings SQL Queries

This project contains a set of SQL queries for analyzing a database of famous paintings. Below are the details about the tables used in the analysis and the corresponding SQL queries.

## SQL Queries

### 1. Fetch all the paintings which are not displayed in any museums

```sql
SELECT name AS painting_name, museum_id 
FROM work
WHERE museum_id = 0;
```

### 2. Count the number of paintings that have an asking price higher than their regular price

```sql
SELECT COUNT(*) AS count_of_paintings
FROM famouspaintings.product_size
WHERE sale_price > regular_price;
```

### 3. Identify the paintings whose asking price is less than 50% of its regular price

```sql
SELECT w.name, ps.sale_price, ps.regular_price
FROM famouspaintings.product_size ps
JOIN famouspaintings.work w
ON ps.work_id = w.work_id
WHERE ps.sale_price < 0.5 * ps.regular_price;
```

### 4. Which canva size costs the most?

```sql
SELECT cs.width, cs.height, cs.label, ps.sale_price
FROM famouspaintings.canvas_size cs
JOIN famouspaintings.product_size ps
ON cs.size_id = ps.size_id
ORDER BY ps.sale_price DESC
limit 1;
```

### 5. Identify the museums which are both open on Sunday and Monday

```sql
SELECT m.name, m.country, m.city
FROM famouspaintings.museum m
JOIN famouspaintings.museum_hours mh
ON m.museum_id = mh.museum_id
WHERE mh.day IN ('Sunday', 'Monday')
GROUP BY m.name, m.country, m.city
HAVING COUNT(DISTINCT mh.day) = 2;
```

### 6. Identify the museums which are open all seven days

```sql
SELECT m.name, m.country, m.city
FROM famouspaintings.museum m
JOIN famouspaintings.museum_hours mh ON m.museum_id = mh.museum_id
WHERE mh.day IN ('Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday')
GROUP BY m.name, m.country, m.city
HAVING COUNT(DISTINCT mh.day) = 7;
```

### 7. Fetch the top 10 most famous painting subject.

```sql
SELECT subject, COUNT(*) AS subject_count
FROM subject
GROUP BY subject
ORDER BY subject_count DESC
LIMIT 10;
```

### 8. Identify the museums which are open on both Sunday and Monday.

```sql
SELECT m.city, m.name
FROM famouspaintings.museum m
JOIN famouspaintings.museum_hours mh ON m.museum_id = mh.museum_id
WHERE mh.day IN('Sunday', 'Monday')
GROUP BY m.museum_id, m.city, m.name
HAVING COUNT(DISTINCT mh.day) = 2
LIMIT 0, 1000;
```

### 9. Which are the top 5 most popular museum? (Popularity is defined based on most no of paintings in a museum)

```sql
SELECT m.name, w.name, COUNT(*) AS work_count
FROM famouspaintings.museum m 
JOIN famouspaintings.work w ON m.museum_id = w.museum_id
GROUP BY w.name, m.name
ORDER BY work_count DESC
LIMIT 5;
```

### 10. Which artists have the most paintings?

```sql
SELECT a.full_name, COUNT(*) AS painting_count
FROM famouspaintings.artist a
JOIN famouspaintings.work w ON a.artist_id = w.artist_id
GROUP BY a.full_name
ORDER BY painting_count DESC
LIMIT 10;
```

### 11. What is the average price of paintings per artist?

```sql
SELECT a.full_name, AVG(ps.sale_price) AS average_price
FROM famouspaintings.artist a
JOIN famouspaintings.work w ON a.artist_id = w.artist_id
JOIN famouspaintings.product_size ps ON w.work_id = ps.work_id
GROUP BY a.full_name
ORDER BY average_price DESC
LIMIT 10;
```

### 12. Number of paintings by style

```sql
SELECT style, COUNT(*) AS painting_count
FROM famouspaintings.work
GROUP BY style
ORDER BY painting_count DESC;
```

### 13. Museum with the highest average painting price

```sql
SELECT m.name, AVG(ps.sale_price) AS average_price
FROM famouspaintings.museum m
JOIN famouspaintings.work w ON m.museum_id = w.museum_id
JOIN famouspaintings.product_size ps ON w.work_id = ps.work_id
GROUP BY m.name
ORDER BY average_price DESC
LIMIT 10;
```

### 14. Top 5 artists with the highest total sales

```sql
SELECT a.full_name, SUM(ps.sale_price) AS total_sales
FROM famouspaintings.artist a
JOIN famouspaintings.work w ON a.artist_id = w.artist_id
JOIN famouspaintings.product_size ps ON w.work_id = ps.work_id
GROUP BY a.full_name
ORDER BY total_sales DESC
LIMIT 5;
```

### 15. Percentage of paintings for each nationality of the artists

```sql
SELECT a.nationality, COUNT(*) * 100.0 / (SELECT COUNT(*) FROM famouspaintings.work) AS percentage
FROM famouspaintings.artist a
JOIN famouspaintings.work w ON a.artist_id = w.artist_id
GROUP BY a.nationality
ORDER BY percentage DESC;
```
## Tables

### artist
Contains information about artists with the following columns:
- `artist_id`: Unique identifier for the artist
- `full_name`: Full name of the artist
- `first_name`: First name of the artist
- `middle_name`: Middle name of the artist
- `last_name`: Last name of the artist
- `nationality`: Nationality of the artist
- `style`: Artistic style of the artist
- `birth`: Birth year of the artist
- `death`: Death year of the artist

Size: 421 rows

### canvas_size
Records data about the canvases used by artists with the following columns:
- `size_id`: Unique identifier for the canvas size
- `width`: Width of the canvas
- `height`: Height of the canvas
- `label`: Label describing the canvas size

Size: 200 rows

### image_link
Consists of links to art pieces with the following columns:
- `work_id`: Unique identifier for the artwork
- `image_id`: Unique identifier for the image
- `url`: URL of the image
- `description`: Description of the image

Size: 14,775 rows (URLs are non-functional and the table is not essential for the analysis)

### museum
Provides information about museums with the following columns:
- `museum_id`: Unique identifier for the museum
- `name`: Name of the museum
- `address`: Address of the museum
- `city`: City where the museum is located
- `state`: State where the museum is located
- `postal`: Postal code of the museum
- `country`: Country where the museum is located
- `phone`: Phone number of the museum
- `url`: Website URL of the museum

Size: 57 rows

### museum_hours
Contains details about museum operating hours with the following columns:
- `museum_id`: Unique identifier for the museum
- `day`: Day of the week
- `open`: Opening time
- `close`: Closing time

Size: 351 rows

### product_size
Holds data regarding art pricing and dimensions with the following columns:
- `work_id`: Unique identifier for the artwork
- `size_id`: Unique identifier for the canvas size
- `sale_price`: Sale price of the artwork
- `regular_price`: Regular price of the artwork

Size: 110,347 rows

### subject
Records information about the subjects of art with the following columns:
- `work_id`: Unique identifier for the artwork
- `subject`: Subject of the artwork

Size: 6,771 rows

### work
Provides details about artworks with the following columns:
- `work_id`: Unique identifier for the artwork
- `name`: Name of the artwork
- `artist_id`: Unique identifier for the artist
- `style`: Artistic style of the artwork
- `museum_id`: Unique identifier for the museum where the artwork is displayed

Size: 14,776 rows

