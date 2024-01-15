# Decoding the Evolution of Top 400 Games (1977-2020)

<p>In this project, I'll explore the top 400 best-selling video games created between 1977 and 2020. I'll compare a dataset on game sales with critic and user reviews to determine whether or not video games have improved as the gaming market has grown.</p>

## Data
<h3 id="game_sales"><code>game_sales</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>game</code></td>
<td>varchar</td>
<td>Name of the video game</td>
</tr>
<tr>
<td style="text-align:left;"><code>platform</code></td>
<td>varchar</td>
<td>Gaming platform</td>
</tr>
<tr>
<td style="text-align:left;"><code>publisher</code></td>
<td>varchar</td>
<td>Game publisher</td>
</tr>
<tr>
<td style="text-align:left;"><code>developer</code></td>
<td>varchar</td>
<td>Game developer</td>
</tr>
<tr>
<td style="text-align:left;"><code>games_sold</code></td>
<td>float</td>
<td>Number of copies sold (millions)</td>
</tr>
<tr>
<td style="text-align:left;"><code>year</code></td>
<td>int</td>
<td>Release year</td>
</tr>
</tbody>
</table>
<h3 id="reviews"><code>reviews</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>game</code></td>
<td>varchar</td>
<td>Name of the video game</td>
</tr>
<tr>
<td style="text-align:left;"><code>critic_score</code></td>
<td>float</td>
<td>Critic score according to Metacritic</td>
</tr>
<tr>
<td style="text-align:left;"><code>user_score</code></td>
<td>float</td>
<td>User score according to Metacritic</td>
</tr>
</tbody>
</table>
<p>Let's begin by looking at some of the top selling video games of all time!</p>

## 1. The ten best-selling video games
```sql

select *
from game_sales
order by games_sold desc
limit 10
```

    10 rows affected.





<table>
    <tr>
        <th>game</th>
        <th>platform</th>
        <th>publisher</th>
        <th>developer</th>
        <th>games_sold</th>
        <th>year</th>
    </tr>
    <tr>
        <td>Wii Sports for Wii</td>
        <td>Wii</td>
        <td>Nintendo</td>
        <td>Nintendo EAD</td>
        <td>82.90</td>
        <td>2006</td>
    </tr>
    <tr>
        <td>Super Mario Bros. for NES</td>
        <td>NES</td>
        <td>Nintendo</td>
        <td>Nintendo EAD</td>
        <td>40.24</td>
        <td>1985</td>
    </tr>
    <tr>
        <td>Counter-Strike: Global Offensive for PC</td>
        <td>PC</td>
        <td>Valve</td>
        <td>Valve Corporation</td>
        <td>40.00</td>
        <td>2012</td>
    </tr>
    <tr>
        <td>Mario Kart Wii for Wii</td>
        <td>Wii</td>
        <td>Nintendo</td>
        <td>Nintendo EAD</td>
        <td>37.32</td>
        <td>2008</td>
    </tr>
    <tr>
        <td>PLAYERUNKNOWN&#x27;S BATTLEGROUNDS for PC</td>
        <td>PC</td>
        <td>PUBG Corporation</td>
        <td>PUBG Corporation</td>
        <td>36.60</td>
        <td>2017</td>
    </tr>
    <tr>
        <td>Minecraft for PC</td>
        <td>PC</td>
        <td>Mojang</td>
        <td>Mojang AB</td>
        <td>33.15</td>
        <td>2010</td>
    </tr>
    <tr>
        <td>Wii Sports Resort for Wii</td>
        <td>Wii</td>
        <td>Nintendo</td>
        <td>Nintendo EAD</td>
        <td>33.13</td>
        <td>2009</td>
    </tr>
    <tr>
        <td>Pokemon Red / Green / Blue Version for GB</td>
        <td>GB</td>
        <td>Nintendo</td>
        <td>Game Freak</td>
        <td>31.38</td>
        <td>1998</td>
    </tr>
    <tr>
        <td>New Super Mario Bros. for DS</td>
        <td>DS</td>
        <td>Nintendo</td>
        <td>Nintendo EAD</td>
        <td>30.80</td>
        <td>2006</td>
    </tr>
    <tr>
        <td>New Super Mario Bros. Wii for Wii</td>
        <td>Wii</td>
        <td>Nintendo</td>
        <td>Nintendo EAD</td>
        <td>30.30</td>
        <td>2009</td>
    </tr>
</table>


## 2. Missing review scores
<p>First, it's important to explore the limitations of the database. One big shortcoming is that there is not any <code>reviews</code> data for some of the games on the <code>game_sales</code> table. </p>


```sql
select count(g.game)
from game_sales g
Left join reviews r
on g.game = r.game
where (critic_score is null and user_score is null)
```


    1 rows affected.





<table>
    <tr>
        <th>count</th>
    </tr>
    <tr>
        <td>31</td>
    </tr>
</table>

<p>It looks like a little less than ten percent of the games on the <code>game_sales</code> table don't have any reviews data. That's a small enough percentage that I can continue the exploration, but the missing reviews data is a good thing to keep in mind as we move on to evaluating results from more sophisticated queries. </p>
<p>There are lots of ways to measure the best years for video games! Let's start with what the critics think. </p>

## 3. Years that video game critics loved


```sql

select year,
      round(avg(critic_score), 2) as avg_critic_score
from game_sales 
Inner Join reviews USING(game)
GROUP BY year
ORDER BY 2 DESC
LIMIT 10
```

    10 rows affected.





<table>
    <tr>
        <th>year</th>
        <th>avg_critic_score</th>
    </tr>
    <tr>
        <td>1990</td>
        <td>9.80</td>
    </tr>
    <tr>
        <td>1992</td>
        <td>9.67</td>
    </tr>
    <tr>
        <td>1998</td>
        <td>9.32</td>
    </tr>
    <tr>
        <td>2020</td>
        <td>9.20</td>
    </tr>
    <tr>
        <td>1993</td>
        <td>9.10</td>
    </tr>
    <tr>
        <td>1995</td>
        <td>9.07</td>
    </tr>
    <tr>
        <td>2004</td>
        <td>9.03</td>
    </tr>
    <tr>
        <td>1982</td>
        <td>9.00</td>
    </tr>
    <tr>
        <td>2002</td>
        <td>8.99</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>8.93</td>
    </tr>
</table>



<p>The range of great years according to critic reviews goes from 1982 until 2020.</p>
<p>However, some of those <code>avg_critic_score</code> values look like suspiciously round numbers for averages. The value for 1982 looks especially fishy. Maybe there weren't a lot of video games in our dataset that were released in certain years. </p>


## 4. Was 1982 really that great?

<p>Let's update our query and find out whether 1982 really was such a great year for video games.</p>


```sql

select year,
      round(avg(critic_score), 2) as avg_critic_score,
      count(*) as num_games
from game_sales 
Inner Join reviews USING(game)
GROUP BY year
HAVING count(*) >= 4
ORDER BY 2 DESC
LIMIT 10
```


    10 rows affected.





<table>
    <tr>
        <th>year</th>
        <th>avg_critic_score</th>
        <th>num_games</th>
    </tr>
    <tr>
        <td>1998</td>
        <td>9.32</td>
        <td>10</td>
    </tr>
    <tr>
        <td>2004</td>
        <td>9.03</td>
        <td>11</td>
    </tr>
    <tr>
        <td>2002</td>
        <td>8.99</td>
        <td>9</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>8.93</td>
        <td>11</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>8.82</td>
        <td>13</td>
    </tr>
    <tr>
        <td>2011</td>
        <td>8.76</td>
        <td>26</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>8.67</td>
        <td>13</td>
    </tr>
    <tr>
        <td>2013</td>
        <td>8.66</td>
        <td>18</td>
    </tr>
    <tr>
        <td>2008</td>
        <td>8.63</td>
        <td>20</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>8.62</td>
        <td>13</td>
    </tr>
</table>


<p>That looks better! The <code>num_games</code> column convinces me that the new list of the critics' top games reflects years that had quite a few well-reviewed games rather than just one or two hits. But which years dropped off the list due to having four or fewer reviewed games? I am going to identify them so that someday I can track down more game reviews for those years and determine whether they might rightfully be considered as excellent years for video game releases!</p>

## 5. Years that dropped off the critics' favorites list
<p>To get started, I've created tables with the results of our previous two queries:</p>

<h3 id="top_critic_years"><code>top_critic_years</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>year</code></td>
<td>int</td>
<td>Year of video game release</td>
</tr>
<tr>
<td style="text-align:left;"><code>avg_critic_score</code></td>
<td>float</td>
<td>Average of all critic scores for games released in that year</td>
</tr>
</tbody>
</table>
<h3 id="top_critic_years_more_than_four_games"><code>top_critic_years_more_than_four_games</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>year</code></td>
<td>int</td>
<td>Year of video game release</td>
</tr>
<tr>
<td style="text-align:left;"><code>num_games</code></td>
<td>int</td>
<td>Count of the number of video games released in that year</td>
</tr>
<tr>
<td style="text-align:left;"><code>avg_critic_score</code></td>
<td>float</td>
<td>Average of all critic scores for games released in that year</td>
</tr>
</tbody>
</table>


```sql

SELECT year, avg_critic_score
FROM top_critic_years
EXCEPT
SELECT year, avg_critic_score
FROM top_critic_years_more_than_four_games
ORDER BY avg_critic_score DESC;
```

    6 rows affected.



<table>
    <tr>
        <th>year</th>
        <th>avg_critic_score</th>
    </tr>
    <tr>
        <td>1990</td>
        <td>9.80</td>
    </tr>
    <tr>
        <td>1992</td>
        <td>9.67</td>
    </tr>
    <tr>
        <td>2020</td>
        <td>9.20</td>
    </tr>
    <tr>
        <td>1993</td>
        <td>9.10</td>
    </tr>
    <tr>
        <td>1995</td>
        <td>9.07</td>
    </tr>
    <tr>
        <td>1982</td>
        <td>9.00</td>
    </tr>
</table>



<p>Based on the insight from the task above, it looks like the early 1990s might merit consideration as the golden age of video games based on <code>critic_score</code> alone, but we'd need to gather more games and reviews data to do further analysis. </p>


## 6. Years video game players loved

<p>Now, I will be looking at the opinions of another important group of people: players! To begin, I will create a query very similar to the one I used in Task Four, except this one will look at <code>user_score</code> averages by year rather than <code>critic_score</code> averages.</p>


```sql

select year,
      round(avg(user_score), 2) as avg_user_score,
      count(*) as num_games
from game_sales 
Inner Join reviews USING(game)
GROUP BY year
HAVING count(*) >= 4
ORDER BY 2 DESC
LIMIT 10
```


    10 rows affected.





<table>
    <tr>
        <th>year</th>
        <th>avg_user_score</th>
        <th>num_games</th>
    </tr>
    <tr>
        <td>1997</td>
        <td>9.50</td>
        <td>8</td>
    </tr>
    <tr>
        <td>1998</td>
        <td>9.40</td>
        <td>10</td>
    </tr>
    <tr>
        <td>2010</td>
        <td>9.24</td>
        <td>23</td>
    </tr>
    <tr>
        <td>2009</td>
        <td>9.18</td>
        <td>20</td>
    </tr>
    <tr>
        <td>2008</td>
        <td>9.03</td>
        <td>20</td>
    </tr>
    <tr>
        <td>1996</td>
        <td>9.00</td>
        <td>5</td>
    </tr>
    <tr>
        <td>2005</td>
        <td>8.95</td>
        <td>13</td>
    </tr>
    <tr>
        <td>2006</td>
        <td>8.95</td>
        <td>16</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>8.80</td>
        <td>8</td>
    </tr>
    <tr>
        <td>2002</td>
        <td>8.80</td>
        <td>9</td>
    </tr>
</table>





## 7. Years that both players and critics loved
<p>Alright, I've got a list of the top ten years according to both critic reviews and user reviews. Are there any years that showed up on both tables? If so, those years would certainly be excellent ones!</p>

<h3 id="top_critic_years_more_than_four_games"><code>top_critic_years_more_than_four_games</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>year</code></td>
<td>int</td>
<td>Year of video game release</td>
</tr>
<tr>
<td style="text-align:left;"><code>num_games</code></td>
<td>int</td>
<td>Count of the number of video games released in that year</td>
</tr>
<tr>
<td style="text-align:left;"><code>avg_critic_score</code></td>
<td>float</td>
<td>Average of all critic scores for games released in that year</td>
</tr>
</tbody>
</table>
<p>I've also saved the results of our top user years query from the previous task into a table:</p>
<h3 id="top_user_years_more_than_four_games"><code>top_user_years_more_than_four_games</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>year</code></td>
<td>int</td>
<td>Year of video game release</td>
</tr>
<tr>
<td style="text-align:left;"><code>num_games</code></td>
<td>int</td>
<td>Count of the number of video games released in that year</td>
</tr>
<tr>
<td style="text-align:left;"><code>avg_user_score</code></td>
<td>float</td>
<td>Average of all user scores for games released in that year</td>
</tr>
</tbody>
</table>


```sql

select year
from top_critic_years_more_than_four_games
INTERSECT
select year
from top_user_years_more_than_four_games
```

    3 rows affected.





<table>
    <tr>
        <th>year</th>
    </tr>
    <tr>
        <td>1998</td>
    </tr>
    <tr>
        <td>2008</td>
    </tr>
    <tr>
        <td>2002</td>
    </tr>
</table>



<p>Looks like there is three years that both users and critics agreed were in the top ten! There are many other ways of measuring what the best years for video games are, but I'll stick with these years for now.</p>


## 8. Sales in the best video game years
<p> I know that critics and players liked these years, but what about video game makers? Were sales good?</p>
<p>This time, I will not create a table from the results from the previous task. Instead, I'll use the query from the previous task as a subquery in this one! This is a great skill to have, as we don't always have write permissions on the database we are querying.</p>


```sql

select year,
        sum(games_sold) as total_games_sold
from game_sales
where year in (select year
from top_critic_years_more_than_four_games
INTERSECT
select year
from top_user_years_more_than_four_games)
Group by year
order by 2 desc
```

    
    3 rows affected.





<table>
    <tr>
        <th>year</th>
        <th>total_games_sold</th>
    </tr>
    <tr>
        <td>2008</td>
        <td>175.07</td>
    </tr>
    <tr>
        <td>1998</td>
        <td>101.52</td>
    </tr>
    <tr>
        <td>2002</td>
        <td>58.67</td>
    </tr>
</table>


