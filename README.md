# Query Works MIST 4610 Project 2



# Team Members

1. Bridger Putzer [@bputzer](https://github.com/bputzer?tab=overview&from=2024-10-01&to=2024-10-13)
2. Colby Love [@ColbyMIST](https://github.com/ColbyMIST)
3. Dev Gajera [@devg0804](https://github.com/devg0804)
4. Grayson Duckworth [@grayson-duck](https://github.com/grayson-duck)
5. Brooke Soto [@3rooke15](https://github.com/3rooke15)

# Problem Description
For project #2, Query Works decided to iterate on our previous project and make it better than it was before. With a focus on increased functionality and more use cases, Feature Film Facts and Figures is more helpful than ever.

The problem we tackled was to build a data model to store and sort movie data better than conventional options available on the web. The core of the model is the movie class, which represents each unique film within the database that a customer may watch. 
The movie class is connected to the greater movie industry through the awards, box_office, movie_screening, director, and studio entities. The consumer side is represented by reviews, genre, and movie_screening. Rather than rely on ad-cluttered sites like IMDB, we sought to build a more useful way for customers and directors to get the movie data they need exactly how they want it.

# Data Model

Explanation of Data Model
This data model represents a movie database with several interrelated entities, each storing specific information about movies, actors, reviews, and other relevant details. Here's a breakdown of the main components:
Movie: This is the central entity, which stores basic information about movies, including the title, release year, rating, duration, and links to related entities like director, studio, and genre.
Actor: This entity holds information about actors, including their name, gender, and date of birth. It is linked to the movie table through the movie_Actor table, establishing a many-to-many relationship (i.e., an actor can appear in multiple movies, and a movie can have multiple actors).
Genre: This table defines different genres (e.g., Sci-Fi, Drama). Each movie can be associated with one genre through the idgenre foreign key in the movie table, establishing a one-to-many relationship (i.e., a genre can have multiple movies, but each movie has only one genre).
Director: This table stores information about directors, including their name, nationality, birthdate, and the number of awards won. Each movie is linked to one director, establishing a one-to-many relationship (i.e., a director can direct multiple movies).
Studio: This table represents the production studio that produced the movie. It contains details like the studio's name, location, and the number of movies produced. Each movie is linked to one studio.
Movie_Screening: This table stores details about individual movie screenings, including the theater location, date, time, and ticket sales. Each screening is linked to a specific movie, creating a one-to-many relationship between movies and screenings.
Box Office: This table tracks box office performance, including revenue, region, and tickets sold. Each record is linked to a specific movie, reflecting how a movie performed in different regions.
Review: This table stores user reviews, including the review score, the reviewer’s name, and their comments. Each review is linked to both a movie and a customer (the reviewer).
Customer: This table stores customer information such as their name, email address, and membership status. Customers are linked to the review table, meaning each customer can submit reviews for multiple movies.
Awards: This table tracks awards that movies or actors have won, including the award name, the role of the person (e.g., Best Director), and the quantity. It is linked to both the movie and actor tables.

![Project 2 Data Model (1)](https://github.com/user-attachments/assets/125f21cd-23da-4bda-9b3c-1e83de2ba454)

# Data Dictionary
![Screenshot 2024-10-13 164219](https://github.com/user-attachments/assets/63df644c-b358-47cf-9137-8aad183492b3)
![Screenshot 2024-10-13 164230](https://github.com/user-attachments/assets/77205054-6424-4e4b-afa3-3ad75d68dee8)
![Screenshot 2024-10-13 164242](https://github.com/user-attachments/assets/10c5e8ed-db87-4b92-94f4-a8dd40c6015b)
![Screenshot 2024-10-13 164253](https://github.com/user-attachments/assets/d01cc9d2-d233-44e5-bacc-4f6996b7f6aa)
![Screenshot 2024-10-13 164303](https://github.com/user-attachments/assets/67009a5c-795c-4e91-960c-2721aab7d678)
![Screenshot 2024-10-13 164312](https://github.com/user-attachments/assets/fc2ea5aa-02ed-4015-b699-722aa3ff9d61)
![Screenshot 2024-10-13 164323](https://github.com/user-attachments/assets/5cbb4387-7b70-478d-81a2-cddb442fc62d)
![Screenshot 2024-10-13 164332](https://github.com/user-attachments/assets/f6547b62-fc2e-4466-8858-173f28b1afd5)
![Screenshot 2024-10-13 164341](https://github.com/user-attachments/assets/e3e707b0-9212-4b18-9efc-6783c165ed51)
![Screenshot 2024-10-13 164352](https://github.com/user-attachments/assets/41789c9f-6eb4-4ad8-8afa-ba7468932308)
![Screenshot 2024-10-13 164400](https://github.com/user-attachments/assets/e1be792b-82b7-406f-9aae-86999a29fc24)



# Queries

![Query Chart](https://github.com/user-attachments/assets/c3d9b774-4d8e-4a99-8f89-792606804393)
![Screenshot 2024-12-02 105259](https://github.com/user-attachments/assets/c57aa6ca-d4a7-46bb-94e1-1e2ea6ac0768)

1. Which movies have an average rating above 7? 

- SELECT movie.genre, AVG(reviewScore)   
- FROM review   
- JOIN movie ON movie.idmovie = review.idmovie   
- GROUP BY movie.genre   
- HAVING AVG(reviewScore) > 7   
- ORDER BY AVG(reviewScore) DESC; 

![Screenshot 2024-10-13 174226](https://github.com/user-attachments/assets/8f2be987-229e-46fd-8984-0edf79e93639)


Justification:  

This query helps managers identify high-performing genres that consistently receive favorable audience reviews. It can inform decisions on what types of movies studios should produce more frequently. 

2. Select the names of movies where the number of tickets sold is higher than the average of movies in North America.

- SELECT title, TicketsSold   
- FROM box_office   
- JOIN movie ON movie.idmovie = box_office.idmovie   
- WHERE TicketsSold >   
- (SELECT AVG(TicketsSold) FROM box_office WHERE region = "North America")   
- AND region = "North America";  

![Screenshot 2024-10-13 174349](https://github.com/user-attachments/assets/e57bdbfa-3e77-47ca-bfa5-c0c1d3969ee0)


Justification: 

This query provides insights into which movies are outperforming in a key market (North America), helping managers focus marketing and distribution efforts. This information helps movie producers know what kind of movies they should produce if they want to appeal to this specific market.

3. Which studios have won at least 3 Academy Awards for their movies?

- SELECT COUNT(awardName) AS `number of awards`, studio.name   
- FROM studio   
- JOIN movie ON studio.idstudio = movie.idstudio   
- JOIN awards ON awards.idmovie = movie.idmovie   
- WHERE awardName REGEXP "Academy Award"   
- GROUP BY studio.idstudio   
- HAVING COUNT(awardName) >= 3;  

![Screenshot 2024-10-13 174455](https://github.com/user-attachments/assets/e6f2cac9-8ec9-4726-be8e-24a649b7f53a)

Justification: 

Tracking which studios have consistently won prestigious awards is valuable for partnerships and investment decisions. Only including movies that have won at least 3 times filters results to only show studios that have a proven track record of success. 

4. List movies, their release year, and their director if the movie has no awards, ordered by release year.

- SELECT title, name, releaseYear   
- FROM movie   
- JOIN director ON director.iddirector = movie.iddirector   
- WHERE NOT EXISTS (SELECT * FROM awards WHERE movie.idmovie = awards.idmovie)   
- ORDER BY releaseYear DESC;  

![Screenshot 2024-10-13 174942](https://github.com/user-attachments/assets/670500bd-2c3a-4235-8b0b-319ac5411ccc)

Justification: 

Identifying movies without awards can help managers evaluate underappreciated films for potential re-releases or marketing opportunities. These results can also lead them to understand what customers don’t like to see in their movies. 

5. Which actor has appeared in the most award-winning movies? 

- SELECT Actor.idActor, name, COUNT(DISTINCT awards.idmovie) AS `Number of Award-Winning Movies`   
- FROM Actor   
- JOIN awards ON Actor.idActor = awards.idActor   
- JOIN Movie_Actor ON Actor.idActor = Movie_Actor.idActor   
- GROUP BY Actor.idActor, name   
- ORDER BY COUNT(DISTINCT awards.idmovie) DESC; 

![Screenshot 2024-10-13 174923](https://github.com/user-attachments/assets/d81dc896-a22d-40ef-90bf-fd9f1ccf0877)

Justification: 

This query helps managers identify actors who consistently contribute to award-winning films, which can inform casting decisions. 

6. Which directors are the most successful based on revenue and number of movies directed?

- SELECT director.name, COUNT(title) AS `number of movies directed`, SUM(box_office.revenue) AS `total revenue`   
- FROM director   
- JOIN movie ON movie.iddirector = director.iddirector   
- JOIN box_office ON box_office.idmovie = movie.idmovie   
- GROUP BY director.iddirector   
- ORDER BY `total revenue` DESC, `number of movies directed`;  

![Screenshot 2024-10-13 174907](https://github.com/user-attachments/assets/af2c08e8-25e8-4fd5-bceb-e9157d8742fd)


Justification: 

Tracking the most financially successful directors helps in strategic planning for future projects and collaborations. 

7. Select the release year, number of movies produced that year, and total revenue for those movies (only for movies over 2 hours, rated above 7, released post-2009). 

- SELECT releaseYear, COUNT(*), SUM(revenue)   
- FROM movie   
- JOIN box_office ON movie.idmovie = box_office.idmovie   
- WHERE rating > 7   
- AND duration > 120   
- GROUP BY releaseYear   
- HAVING releaseYear > 2009   
- ORDER BY releaseYear;

![Screenshot 2024-10-13 174827](https://github.com/user-attachments/assets/259b5cc6-6ebf-4788-a305-efe33ea58378)

Justification: 

This query helps managers analyze the performance of high-quality, long-duration movies in the past decade, allowing for data-driven future investments.

8. Which actors appear in movies with average box office revenue higher than the overall average?

- SELECT Actor.idActor, name, AVG(revenue) AS `Average Box Office Revenue`   
- FROM Actor   
- JOIN Movie_Actor ON Actor.idActor = Movie_Actor.idActor   
- JOIN box_office ON Movie_Actor.idmovie = box_office.idmovie   
- GROUP BY Actor.idActor, name   
- HAVING AVG(revenue) > (SELECT AVG(revenue) FROM box_office)   
- ORDER BY AVG(revenue) DESC;  

![Screenshot 2024-10-13 174811](https://github.com/user-attachments/assets/eff40b86-f438-463d-aa53-6a655b44497e)

Justification: 

Identifying actors associated with high-revenue films helps managers make more strategic casting choices for profitable outcomes. This query also allows management to see the exact relationship between the actors of movies and the amount of revenue the movie generates.

9. At which times do movies sell the most tickets? 

- SELECT time, theaterLocation, title, date, ticketSales   
- FROM movie_screening   
- JOIN movie ON movie.idmovie = movie_screening.idmovie   
- ORDER BY ticketSales DESC; 

![Screenshot 2024-10-13 174748](https://github.com/user-attachments/assets/3a5dd2cb-2bd4-40dc-a1ec-8bd253d2460a)

Justification: 

Knowing when movies perform best can guide theaters in scheduling showtimes to maximize sales and profit. 

10. Which genre has the most movies with a box office revenue above the average?

- select genreName, count(*) as `number of movies` 
- from genre 
- join movie on genre.idgenre = movie.idgenre 
- join box_office on movie.idmovie = box_office.idmovie 
- where revenue >  
- (select avg(revenue) from box_office) 
- group by genreName 
- order by count(*) desc; 

![Screenshot 2024-10-13 174728](https://github.com/user-attachments/assets/9eed38a3-7f05-4e66-bd1b-c02255a0d6cc)

Justification: 

Understanding which genres consistently outperform in revenue helps guide genre focus in future productions. 

11. Identify underperforming movies based on total tickets sold and the average rating

- Select movie.Title,
- AVG(movie.rating),
- SUM(TicketsSold)
- FROMmovie
- JOIN Box_Office ON movie.idmovie = Box_Office.idmovie
- Group BY movie.idmovie
- Having Avg(movie.rating) < 8 AND SUM(TicketsSold) < 5000;

12. Which movies from a specific genre released, between a range of years have generated revenue above a certain threshold?

- CREATE Procedure moviesWithGenreRevenueYear(IN genreInput VARCHAR(45),
- begYearInput INT, endYearInput INT, revenueInput INT)
- Select movie.Title, Box_Office.revenue, Genre.genreName
- FROMmovie
- Join Box_Office ON movie.idmovie = Box_Office.idmovie
- Join Genre on Genre.idgenre = movie.idgenre
- Where Genre.genreName = genreInput AND movie.releaseYear BETWEEN begYearInput AND
- endYearInput AND revenue > revenueInput;

13. Of movies released in the last 15 years, which genres have a percentage of movies
    
- produced greater than 10%?
- Select genreName, count(genreName), (count(genreName) / (select count(*) From movie where
- releaseYear between 2009 and 2024)) * 100 As `percentage` From Genre
- Join movie on movie.idgenre = Genre.idgenre
- Where releaseYear between 2009 AND 2024
- Group By genreName
- Having `percentage` > 10

 14. For each actor, list the number of movies they have performed in for each genre, if the number is > 3
     
 -  Select Actor.name, genreName, COUNT(*) From Actor
 - Join Movie_has_Actor ON Actor.idActor = Movie_has_Actor.Actor_idActor
 - Join movie ON movie.idmovie = Movie_has_Actor.movie_idmovie
 - Join Genre ON Genre.idgenre = movie.idgenre
 - Group by Actor.name, genreName
 - Having COUNT(*) > 3
 - ORDERBYCOUNT(*);

15.  Identify customers who have reviewed movies but not TV shows

- select Customer.idcustomer, name, count(distinct Movie_Review.idmovie) as `Movies reviewed`, count(distinct TV_Show_Review.idShow) as `Shows reviewed` from Customer
left join Movie_Review on Customer.idcustomer = Movie_Review.idcustomer left
join TV_Show_Review on Customer.idcustomer = TV_Show_Review.idcustomer
group by Customer.idcustomer, name HAVING `Movies reviewed` > 0
AND `Shows reviewed` = 0;

# Tableau Visualizations

1. ![Sheet 1](https://github.com/user-attachments/assets/b83f1d8f-6ebe-45b2-bf17-255621e338ba)
2. ![Sheet 2](https://github.com/user-attachments/assets/d22d242a-fb94-48d2-940c-958c2fd685e5)
3. ![Sheet 3](https://github.com/user-attachments/assets/2793227b-5cf6-4a07-a670-3ac0c5b36846)





 # Database Description
 Database name: cs_bpp41344
