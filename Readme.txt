#BookMyShow Database Design

This project models a simplified BookMyShow-like ticketing platform where users can browse movies running in a theatre for the next 7 days and see show timings.

---

## Entities & Attributes

### Theaters
- id (PK)
- name
- city
- state
- address

### Screens
- id (PK)
- theater_id (FK → theaters.id)
- name (e.g., Screen 1, IMAX)
- total_seats

### Movies
- id (PK)
- title
- language
- genre
- duration_mins
- certification (UA, U, A)

### Shows
- id (PK)
- movie_id (FK → movies.id)
- theater_id (FK → theaters.id)
- screen_id (FK → screens.id)
- show_date
- start_time
- end_time
- price
- available_seats

---

## Normalization

- **1NF**: All attributes atomic.  
- **2NF**: Non-key attributes fully dependent on primary key.  
- **3NF**: No transitive dependency.  
- **BCNF**: All determinants are candidate keys.  

---

## Example Query (P2)

List all shows for a given date and theatre:

```sql
SELECT
    m.title AS movie_title,
    sc.name AS screen_name,
    s.show_date,
    TIME_FORMAT(s.start_time, '%h:%i %p') AS show_time
FROM shows s
JOIN movies m ON s.movie_id = m.id
JOIN screens sc ON s.screen_id = sc.id
WHERE s.theater_id = 1
  AND s.show_date = '2025-04-25'
ORDER BY s.start_time;