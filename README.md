
# Famous Paintings SQL Queries

This project contains a set of SQL queries for analyzing a database of famous paintings. Below are the details about the tables used in the analysis and the corresponding SQL queries.

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

## SQL Queries

### 1. Fetch all the paintings which are not displayed in any museums

```sql
SELECT name AS painting_name, museum_id 
FROM work
WHERE museum_id = 0;
