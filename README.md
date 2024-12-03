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

Explanation of Data Model:
This data model represents a comprehensive entertainment database that stores detailed information about movies, television shows, actors, reviews, and other relevant components. The central entity for movies is the Movie table, which holds basic details such as the title, release year, rating, duration, and links to related entities like Director, Studio, and Genre. The Actor table stores information about actors, including their name, gender, and date of birth, and is connected to movies through the Movie_has_Actor table, establishing a many-to-many relationship. The Genre table categorizes movies and TV shows into distinct genres, while the Director and Studio tables store details about the creators and production studios, each forming one-to-many relationships with movies.

The model also tracks movie screenings through the Movie_Screening table, capturing details like theater location, date, time, and ticket sales. Box office performance is stored in the Box_Office table, which records revenue, tickets sold, and regional performance for each movie. Reviews are managed through the Review table, where customers can provide ratings and comments for movies. Customer details, such as their name, email address, and membership status, are stored in the Customer table, linking them to their reviews. The Awards table tracks recognition received by movies and actors, including the award name, role of the recipient, and quantity.

With the addition of television-related components, the data model now includes the TV_Show table to store details about television shows, such as the title, release year, rating, number of seasons, and total episodes. Each TV show is linked to a genre, director, and studio, forming one-to-many relationships. The TV_Show_Season table tracks individual seasons, including the season number, release date, and number of episodes, while the TV_Show_Episodes table captures information about episodes, such as the episode number, title, and duration, with each episode linked to a specific season. The many-to-many relationship between TV shows and actors is managed through the TV_Show_has_Actor table, mirroring the structure used for movies. Reviews for TV shows are stored in the TV_Show_Review table, which links customers to their feedback on TV shows, allowing for optional anonymity.

The Awards table was updated to support awards for both movies and TV shows, adding flexibility to track recognition across different media. These enhancements integrate seamlessly with the original movie-focused design, enabling the database to manage both movies and TV shows with detailed information about their associated actors, reviews, seasons, and episodes. This unified structure provides a robust and scalable solution for managing entertainment data.

![Project 2 Data Model (1)](https://github.com/user-attachments/assets/125f21cd-23da-4bda-9b3c-1e83de2ba454)

# Data Dictionary
![Screenshot 2024-12-02 160354](https://github.com/user-attachments/assets/f34399ba-217d-4bf0-aedb-7dedee1217bb)
![Screenshot 2024-12-02 160539](https://github.com/user-attachments/assets/09b81bd1-9fc2-4079-b7f6-0db572b07c83)
![Screenshot 2024-12-02 160601](https://github.com/user-attachments/assets/f983bd33-3719-485a-b116-c4d30eb4656b)
![Screenshot 2024-12-02 160728](https://github.com/user-attachments/assets/8cb2db90-a0ea-457e-a0a1-bc36974190e4)
![Screenshot 2024-12-02 160750](https://github.com/user-attachments/assets/d2397a58-c877-42f2-9dd3-b8d279995027)




# Queries

![Query Chart](https://github.com/user-attachments/assets/c3d9b774-4d8e-4a99-8f89-792606804393)
![Screenshot 2024-12-02 105259](https://github.com/user-attachments/assets/c57aa6ca-d4a7-46bb-94e1-1e2ea6ac0768)

# Query 1
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

# Query 2

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

# Query 3

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

# Query 4

4. List movies, their release year, and their director if the movie has no awards, ordered by release year.

- SELECT title, name, releaseYear   
- FROM movie   
- JOIN director ON director.iddirector = movie.iddirector   
- WHERE NOT EXISTS (SELECT * FROM awards WHERE movie.idmovie = awards.idmovie)   
- ORDER BY releaseYear DESC;  

![Screenshot 2024-10-13 174942](https://github.com/user-attachments/assets/670500bd-2c3a-4235-8b0b-319ac5411ccc)

Justification: 

Identifying movies without awards can help managers evaluate underappreciated films for potential re-releases or marketing opportunities. These results can also lead them to understand what customers don’t like to see in their movies. 

# Query 5

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

# Query 6

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

# Query 7

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

# Query 8

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

# Query 9

9. At which times do movies sell the most tickets? 

- SELECT time, theaterLocation, title, date, ticketSales   
- FROM movie_screening   
- JOIN movie ON movie.idmovie = movie_screening.idmovie   
- ORDER BY ticketSales DESC; 

![Screenshot 2024-10-13 174748](https://github.com/user-attachments/assets/3a5dd2cb-2bd4-40dc-a1ec-8bd253d2460a)

Justification: 

Knowing when movies perform best can guide theaters in scheduling showtimes to maximize sales and profit. 

# Query 10

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

# Query 11

11. Identify underperforming movies based on total tickets sold and the average rating

- Select movie.Title,
- AVG(movie.rating),
- SUM(TicketsSold)
- FROMmovie
- JOIN Box_Office ON movie.idmovie = Box_Office.idmovie
- Group BY movie.idmovie
- Having Avg(movie.rating) < 8 AND SUM(TicketsSold) < 5000;

Justification: Identifying underperforming movies allows management to flag these movies for further investigation. This can allow management to make adjustments to where studios spend their money and how they market movies. This also allows them to see what kind of movies (ex. Romance) tend to do worse and avoid making those movies in the future.

# Query 12

12. Which movies from a specific genre released, between a range of years have generated revenue above a certain threshold?

- CREATE Procedure moviesWithGenreRevenueYear(IN genreInput VARCHAR(45),
- begYearInput INT, endYearInput INT, revenueInput INT)
- Select movie.Title, Box_Office.revenue, Genre.genreName
- FROMmovie
- Join Box_Office ON movie.idmovie = Box_Office.idmovie
- Join Genre on Genre.idgenre = movie.idgenre
- Where Genre.genreName = genreInput AND movie.releaseYear BETWEEN begYearInput AND
- endYearInput AND revenue > revenueInput;

Justification: This procedure enables management to focus on specific genres and time periods to assess the success of their investment strategies within those contexts. By specifying date ranges, management can analyze how movie performance in a genre has evolved over time and adapt to changing audience preferences. A procedure also allows more flexibility than a regular query.

# Query 13

13. Of movies released in the last 15 years, which genres have a percentage of movies produced greater than 10%?
- Select genreName, count(genreName), (count(genreName) / (select count(*) From movie where
- releaseYear between 2009 and 2024)) * 100 As `percentage` From Genre
- Join movie on movie.idgenre = Genre.idgenre
- Where releaseYear between 2009 AND 2024
- Group By genreName
- Having `percentage` > 10

Justification: Allows management to see the trend in movie production in the modern age. Management can use the query to see what genres are dominating the box office, and they can use this information to adjust their production decisions in the future. For example, if they see that fantasy movies are dominating the market, they can focus on producing these movies and adjust their marketing to appeal to younger audiences, like elementary school children. 

# Query 14

14. For each actor, list the number of movies they have performed in for each genre, if the number is > 3
     
 -  Select Actor.name, genreName, COUNT(*) From Actor
 - Join Movie_has_Actor ON Actor.idActor = Movie_has_Actor.Actor_idActor
 - Join movie ON movie.idmovie = Movie_has_Actor.movie_idmovie
 - Join Genre ON Genre.idgenre = movie.idgenre
 - Group by Actor.name, genreName
 - Having COUNT(*) > 3
 - ORDERBYCOUNT(*);

Justification: Management can use this data to make casting decisions. For example, if a studio wants to make an action movie and is looking for someone to play the main character, they can compare how many action movies Actor A (ex. 3) and Actor B (ex. 2) have performed in. If they want somene with more experience, they might hire Actor A. However, if they want someone with less experience who is more likely to be cheaper, they might hire Actor B. 

# Query 15

15.  Identify customers who have reviewed movies but not TV shows

- select Customer.idcustomer, name, count(distinct Movie_Review.idmovie) as `Movies reviewed`, count(distinct TV_Show_Review.idShow) as `Shows reviewed` from Customer
- left join Movie_Review on Customer.idcustomer = Movie_Review.idcustomer left
- join TV_Show_Review on Customer.idcustomer = TV_Show_Review.idcustomer
- group by Customer.idcustomer, name HAVING `Movies reviewed` > 0
- AND `Shows reviewed` = 0;

Justification: Identifying customers who are interested in a specific type of content helps management understand audience preferences and tailor content recommendations  accordingly. They can also do the opposite and try to offer discounts and incentives to try to get these customers to watch shows, like offering free trials on subscription services. It also helps identify opportunities to engage inactive customers (in the TV show department) by recommending specific TV shows based on the movies they reviewed.

# Tableau Visualizations

1. ![Sheet 1](https://github.com/user-attachments/assets/b83f1d8f-6ebe-45b2-bf17-255621e338ba)
   
 Justification: This line chart visualizes the total box office revenue for all movies within their respective years. Based on this chart, a film enthusiast could see some of the most successful box office years dating back to the 1930s. Throughout history, we can see that box office sales peaked in the mid-1970s before declining until the 1990s. After that, the movie industry had some good years and bad, with an overall upward trend. In recent years, we can see an dramatic increase in revenue in 2019, most likely due to several box office hits like "Avengers: Endgame", "The Lion King", and "Star Wars: The Rise of Skywalker." That revenue, however, quickly disappeared in 2020 with the onset of the COVID-19 pandemic. This graph is useful in that it shows the overall trend of movie revenue over time and that movie companies should not underestimate global events, which can drastically change the total revenue for a year. It also shows that the movie industry has still been recovering from Covid over the last few years, and if it hopes to emulate the success it had before COVID-19, it will need big releases from studios like Marvel, Disney, and Universal. 
 
Question answered: Over time, how has the total amount of revenue in the movie industry changed, and what years in particular are the most influential on the industry?

2. ![Sheet 4](https://github.com/user-attachments/assets/db04a098-c8a0-48b3-a551-ff088ab4053f)

 Justification: This histogram displays the amount of tickets sold by movies winning different awards. Based on this visualization, we can tell that movies that win the "Best Director" award sell more tickets on average than those winning an award for just an actor. A film analyst may be able to use this data to show that moviegoers often value the story and plot, more than they may value seeing a famous actor or actress. A movie analyst would also be able to determine that a famous actress will often sell more tickets than a famous actor, at least in the case of leading roles, which could be used to futher examine the gender breakdown of a particular film or TV show.

 Question answered: Out of all award-winning movies, which awards lead to the most success (based on average tickets sold)? What trends does this data uncover about the audience?
   
3. ![Sheet 3](https://github.com/user-attachments/assets/2793227b-5cf6-4a07-a670-3ac0c5b36846)
 
 Justification: This histogram displays the total revenue, on a studio basis, for the entirety of the time data is available. Based on the data we can see that Walt Disney and Pixar Animation have the largest total revenues. This is most likely due to each of the resepective studio's long history of success. In the second group of studios, we can see 20th Century Fox, Warner Bros, and Univsersal Pictures. These studios are just as large as Diney, but have a shorter history so their values are smaller. Finally, we can see some of the newest studios, Lionsgate, New Line, and Netflix. These studios are younger, meaning they have a smaller total revenue number. A manager looking at this data may see that Netflix has had immense success despite its short history, and may choose a more streaming-focused distribution model instead of the more tradtional one with a box-office release. On the flip side, it may question why Universal, which is still a large movie studio with its own theme park, is not able to come close to studios like Walt Disney and Pixar. Information like this can influence who production companies and actors choose to work with.

 Question answered: Out of studios that made at most 32 movies and at minimum 2 movies, which have generated the most and least revenue? How do streaming platforms compare with smaller movie studios?

4. ![Dashboard](https://github.com/user-attachments/assets/00aa0fa2-0148-4ebd-9994-b5ed52d18a99)

 Justification: This dasboard combines all of the previous visuals into one, seamless view. It displays the total revenue of all movie and TV shows for years dating back to 1930, total revenue broken down by studio in that same period, and finally the average tickets sold depending on what type of award a movie won. The entirety of this dashboard could be used for individuals looking to see the most sucessful years, movie studios, and type of awarded-movie to learn more about movie history. Our new website will incorporate many dasboards like this one in order to appeal to a wide range of cinema critics, fans, and casual moviegoers alike. 

[Download the Tableau Packaged Workbook](https://github.com/ColbyMIST/MIST4610-Project2/raw/refs/heads/main/MIST4610Project2.twbx)





 # Database Description
 Database name: cs_bpp41344
