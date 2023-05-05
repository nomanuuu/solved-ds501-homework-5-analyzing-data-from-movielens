Download Link: https://assignmentchef.com/product/solved-ds501-homework-5-analyzing-data-from-movielens
<br>



In this case study we will look at the movies data set from MovieLens. It contains data about users and how they rate movies.

<strong>Problem 1: Importing the MovieLens data set and merging it into a single data frame</strong>

##

## Attaching package: ‘dplyr’

## The following objects are masked from ‘package:stats’:

##

##                  filter, lag

## The following objects are masked from ‘package:base’:

##

##                        intersect, setdiff, setequal, union

mlData &lt;- distinct(mlData) print(colnames(mlData))

## [1] “user_id” “movie_title” “genre” “rating” “release_date” ## [6] “age” “gender” “occupation”

<strong>Report some basic details of the data you collected. For example:</strong>

<ul>

 <li>How many movies have an average rating over 4.5 overall?</li>

</ul>

<strong>– </strong>I found there were 11 movies with a mean rating over 4.5.

<table width="632">

 <tbody>

  <tr>

   <td width="632">mlData_aggregates &lt;- mlData %&gt;% group_by(movie_title) %&gt;%summarise(mean_rating = mean(rating, na.rm = TRUE))high_mean_rating_count &lt;- mlData_aggregates %&gt;% filter(mean_rating &gt; 4.5) %&gt;% nrow()mlData_aggregates %&gt;% filter(mean_rating &gt; 4.5) %&gt;% head(high_mean_rating_count)</td>

  </tr>

 </tbody>

</table>

## # A tibble: 11 x 2

##             movie_title                                                                                                   mean_rating

##           &lt;fct&gt;       &lt;dbl&gt; ## 1 “Aiqing wansui (1994)”   5

## 2 “Entertaining Angels: The Dorothy Day Story (1996)”                                                 5

## 3 “Great Day in Harlem, A (1994)”                                                                                      5

## 4 “Marlene Dietrich: Shadow and Light (1996) ”                                                             5

## 5 “Pather Panchali (1955)”                                                                                                    4.62

## 6 “Prefontaine (1997)”                                                                                                          5

## 7 “Saint of Fort Washington, The (1993)”                                                                         5

## 8 “Santa with Muscles (1996)”   5 ## 9 “Someone Else’s America (1995)”        5

## 10 “Star Kid (1997)”                                                                                                               5

## 11 “They Made Me a Criminal (1939)”                                                                              5

<ul>

 <li>How many movies have an average rating over 4.5 among men?

  <ul>

   <li>I found there were 18 movies with a mean rating over 4.5 among women.</li>

  </ul></li>

</ul>

<table width="632">

 <tbody>

  <tr>

   <td width="632">mlData_aggregates &lt;- mlData %&gt;% group_by(movie_title) %&gt;% filter(gender == “M”) %&gt;%summarise(mean_rating_men = mean(rating, na.rm = TRUE)) %&gt;% full_join(mlData_aggregates)</td>

  </tr>

 </tbody>

</table>

## Joining, by = “movie_title”

<table width="632">

 <tbody>

  <tr>

   <td width="632">high_mean_men_count &lt;- mlData_aggregates %&gt;% filter(mean_rating_men &gt; 4.5) %&gt;% nrow()mlData_aggregates %&gt;% arrange(desc(mean_rating_men)) %&gt;% select(-mean_rating) %&gt;% head()</td>

  </tr>

 </tbody>

</table>

## # A tibble: 6 x 2

##           movie_title                                                                                              mean_rating_men

##         &lt;fct&gt;                                                                                                                                  &lt;dbl&gt;

## 1 Aiqing wansui (1994) 5 ## 2 Delta of Venus (1994)             5 ## 3 Entertaining Angels: The Dorothy Day Story (1996) 5

## 4 Great Day in Harlem, A (1994) 5 ## 5 Leading Man, The (1996)       5

## 6 Letter From Death Row, A (1998)                                                                                             5

<ul>

 <li>How many movies have an average rating over 4.5 among women?

  <ul>

   <li>I found by using a similar approach to the above that there were 16 movies with a mean rating over 4.5 among women.</li>

  </ul></li>

</ul>

## Joining, by = “movie_title”

## # A tibble: 6 x 2

##           movie_title                                                                    mean_rating_women

##         &lt;fct&gt;                                                                                                             &lt;dbl&gt;

## 1 Everest (1998)                                                                                                          5

## 2 Faster Pussycat! Kill! Kill! (1965)             5 ## 3 Foreign Correspondent (1940)               5 ## 4 Maya Lin: A Strong Clear Vision (1994)               5

## 5 Mina Tannenbaum (1994)       5 ## 6 Prefontaine (1997)  5

<ul>

 <li>Let us order by mean rating but keep men/women mean rating columns for comparison. Note that some movies were not rated by both men and women.</li>

</ul>

## # A tibble: 10 x 4

##             movie_title                                                         mean_rating_wom~ mean_rating_men mean_rating

##            &lt;fct&gt;                                                                                             &lt;dbl&gt;                           &lt;dbl&gt;                  &lt;dbl&gt;

## 1 “Prefontaine (1997)”                                                                                5                                   5                          5

## 2 “Someone Else’s America (1995)”                                                          5                                NA                         5

## 3 “Aiqing wansui (1994)”                                                                          NA                                   5                          5

## 4 “Entertaining Angels: The Dorot~                                                       NA                                   5                          5

## 5 “Great Day in Harlem, A (1994)”                                                         NA                                   5                          5

## 6 “Marlene Dietrich: Shadow and L~                                                     NA                                   5                          5

## 7 “Saint of Fort Washington, The ~                                                        NA                                   5                          5

## 8 “Santa with Muscles (1996)”                                                                NA                                   5                          5

## 9 “Star Kid (1997)”                                                                                     NA                                   5                          5

## 10 “They Made Me a Criminal (1939)”                                                  NA                                   5                          5

<ul>

 <li>How many movies have an median rating over 4.5 among men over age 30?</li>

</ul>

<strong>– </strong>I found there were 47 movies with a median rating over 4.5 among men over 30.

<table width="632">

 <tbody>

  <tr>

   <td width="632">mlData_aggregates &lt;- mlData %&gt;% group_by(movie_title) %&gt;% filter(gender == “M”, age &gt; 30) %&gt;%summarise(median_rating_men30plus = median(rating)) %&gt;% full_join(mlData_aggregates)</td>

  </tr>

 </tbody>

</table>

## Joining, by = “movie_title”

high_median_men30plus_count &lt;- mlData_aggregates %&gt;%

filter(median_rating_men30plus &gt; 4.5) %&gt;% nrow()

mlData_aggregates %&gt;%

arrange(desc(median_rating_men30plus)) %&gt;% select(movie_title, median_rating_men30plus) %&gt;% head(10)

## # A tibble: 10 x 2

##             movie_title                                                                                                median_rating_men30plus

##            &lt;fct&gt;                                                                                                                                                    &lt;dbl&gt;

## 1 Aiqing wansui (1994)                                                                                                                                        5

## 2 Anna (1996)                                                                                                                                                        5

## 3 Aparajito (1956)         5 ## 4 Big Sleep, The (1946)              5 ## 5 Casablanca (1942)   5 ## 6 Citizen Kane (1941)   5 ## 7 Close Shave, A (1995)             5 ## 8 Delta of Venus (1994)             5 ## 9 Entertaining Angels: The Dorothy Day Story (1996) 5

## 10 Faithful (1996)                                                                                                                                                 5

-How many movies have an median rating over 4.5 among women over age 30?

<ul>

 <li>I found using a simlar approach to the above that there were 70 movies with a median rating over 4.5</li>

</ul>

among women over 30.

## Joining, by = “movie_title”

## [1] 70 ## # A tibble: 10 x 2

##             movie_title                                     median_rating_women30plus

##            &lt;fct&gt;                                                                                               &lt;dbl&gt;

## 1 Amateur (1994)          5 ## 2 Angel Baby (1995)   5

## 3 Bent (1997) 5 ## 4 Best Men (1997)      5

## 5 Big Lebowski, The (1998) 5 ## 6 Blade Runner (1982)        5 ## 7 Brassed Off (1996)   5 ## 8 Braveheart (1995)   5 ## 9 Casablanca (1942)   5

## 10 Cats Don’t Dance (1997)                                                                          5

<ul>

 <li>For comparison, order by median rating but keep men/women over 30 median rating columns. Note here that some movies were not rated by both men over 30 and women over 30.</li>

</ul>

## Joining, by = “movie_title”

## # A tibble: 10 x 4

##             movie_title                                         median_rating median_rating_men~ median_rating_wome~

##           &lt;fct&gt;       &lt;dbl&gt;      &lt;dbl&gt;      &lt;dbl&gt; ## 1 Aiqing wansui (1994)      5              5              NA ## 2 Aparajito (1956)   5               5              NA

## 3 Casablanca (1942)                                                           5                                          5                                            5

## 4 Citizen Kane (1941)                                                         5                                          5                                            4

## 5 Close Shave, A (1995)                                                     5                                          5                                            5

## 6 Entertaining Angels: Th~                                                5                                          5                                         NA

## 7 Faust (1994)                                                                      5                                          5                                         NA

## 8 Godfather, The (1972) 5 5 4 ## 9 Great Day in Harlem, A ~ 5 5 NA

## 10 Hugo Pool (1997)                                                           5                                          1                                         NA

<ul>

 <li>What are the ten most “popular” movies?</li>

</ul>

<strong>– </strong>Perhaps we might consider a movie popular if it has both a high mean and median rating. I found there were many films with a mean rating of 5 and many films with a median rating of 5. Upon finding the intersection of these two sets, it turned out that there were 10 films with both a mean rating of 5 and a median rating of 5. Without considering rating count, we could propose that the top ten most popular films are these 10 films with median and mean rating of 5. ‘

<table width="632">

 <tbody>

  <tr>

   <td width="632">mlData_popular &lt;- mlData_aggregates %&gt;% filter((mean_rating == 5) &amp; (median_rating == 5) )mlData_popular %&gt;% select(movie_title, median_rating, mean_rating) %&gt;% head(nrow(mlData_popular))</td>

  </tr>

 </tbody>

</table>

## # A tibble: 10 x 3

##             movie_title                                                                                                      median_rating mean_rating

##            &lt;fct&gt;                                                                                                                                  &lt;dbl&gt;                  &lt;dbl&gt;

## 1 “Aiqing wansui (1994)”             5 5 ## 2 “Entertaining Angels: The Dorothy Day Story (1996)” 5 5

## 3 “Great Day in Harlem, A (1994)”                                                                                                 5                          5

## 4 “Marlene Dietrich: Shadow and Light (1996) ”                                                                         5                          5

## 5 “Prefontaine (1997)”                                                                                                                     5                          5

## 6 “Saint of Fort Washington, The (1993)” 5              5 ## 7 “Santa with Muscles (1996)” 5              5 ## 8 “Someone Else’s America (1995)”       5              5

## 9 “Star Kid (1997)”                                                                                                                             5                          5

## 10 “They Made Me a Criminal (1939)”                                                                                          5                          5

<ul>

 <li>Make some conjectures about how easy various groups are to please!</li>

 <li>Question: Does the mean rating of all films depend on the age of the reviewer?</li>

 <li>Answer: It appears not, at least without grouping by further characteristics.</li>

</ul>

<h1>Average Film Rating vs. Critic Age</h1>

<ul>

 <li>Question: Do some film genres just generally receive higher ratings than other genres? Do some film genres perform well with certain groups but poorly with other groups?</li>

 <li>Answer: The highest rated genre (using mean rating of all films within each genre) is Film-Noir with a mean rating of about 3.92 while the lowest rated genre is Fantasy with a mean rating of about 3.22.</li>

 <li>Answer: Usually there is not much difference between ratings by men vs women but men tend to enjoy Film-Noir more than women while women enjoy Musicals more than men.</li>

 <li>Answer: Children (critics aged less than 16) enjoy war, sci-fi, and animation films the most.</li>

</ul>

## Joining, by = “genre” ## Joining, by = “genre”

<h1>Considering Mean Film Rating by Genre</h1>

The top 3 highest and 3 lowest rated genres

<table width="582">

 <tbody>

  <tr>

   <td width="60"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="56"> </td>

  </tr>

  <tr>

   <td width="60"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="56"> </td>

  </tr>

  <tr>

   <td width="60"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="56"> </td>

  </tr>

  <tr>

   <td width="60"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="56"> </td>

  </tr>

  <tr>

   <td width="60"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="56"> </td>

  </tr>

  <tr>

   <td width="60"> </td>

   <td width="93"></td>

   <td width="93"> </td>

   <td width="93"></td>

   <td width="93"></td>

   <td width="93"> </td>

   <td width="56"> </td>

  </tr>

  <tr>

   <td width="60"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="56"> </td>

  </tr>

  <tr>

   <td width="60"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="56"> </td>

  </tr>

  <tr>

   <td width="60"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="56"> </td>

  </tr>

  <tr>

   <td width="60"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="93"> </td>

   <td width="56"> </td>

  </tr>

 </tbody>

</table>

4

3

2

1

0

Fantasy                  Horror                 Childrens                 Drama                     War                  Film−Noir

<h2>Film Genre</h2>

## # A tibble: 5 x 3

##         genre              mean_rating_men mean_rating_women

##         &lt;fct&gt;                                     &lt;dbl&gt;                                &lt;dbl&gt;

<table width="335">

 <tbody>

  <tr>

   <td width="105">## 1 Film-Noir</td>

   <td width="195">3.97</td>

   <td width="35">3.74</td>

  </tr>

  <tr>

   <td width="105">## 2 Mystery</td>

   <td width="195">3.67</td>

   <td width="35">3.56</td>

  </tr>

  <tr>

   <td width="105">## 3 Western</td>

   <td width="195">3.64</td>

   <td width="35">3.51</td>

  </tr>

  <tr>

   <td width="105">## 4 Musical</td>

   <td width="195">3.47</td>

   <td width="35">3.64</td>

  </tr>

  <tr>

   <td width="105">## 5 Childrens</td>

   <td width="195">3.32</td>

   <td width="35">3.43</td>

  </tr>

 </tbody>

</table>

<h1>Highest average rated genres among children</h1>

<table width="582">

 <tbody>

  <tr>

   <td width="46"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="42"> </td>

  </tr>

  <tr>

   <td width="46"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="42"> </td>

  </tr>

  <tr>

   <td width="46"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="42"> </td>

  </tr>

  <tr>

   <td width="46"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="42"> </td>

  </tr>

  <tr>

   <td width="46"> </td>

   <td width="71"> </td>

   <td width="71"></td>

   <td width="71"> </td>

   <td width="71"></td>

   <td width="71"></td>

   <td width="71"> </td>

   <td width="71"></td>

   <td width="42"> </td>

  </tr>

  <tr>

   <td width="46"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="42"> </td>

  </tr>

  <tr>

   <td width="46"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="42"> </td>

  </tr>

  <tr>

   <td width="46"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="42"> </td>

  </tr>

  <tr>

   <td width="46"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="71"> </td>

   <td width="42"> </td>

  </tr>

 </tbody>

</table>

3

2

1

0

Thriller          Western        Romance         Horror            Crime          Animation         Sci−Fi              War

<h2>Film Genre</h2>

+ Question: How do critic ratings depend on how old a film is? + Answer: Apparently no, but there appears to be more variance in ratings among newer films.

<h1>Mean Film Ratings by Release Date</h1>

1922−01926−01930−01931932−01931934−1935−01936−01937−01938−01939−01940−01941942−0−01943−0194−01945−01946−01947−0−01948−0−01949−0−01950−0−0195−01952−0−0−01953−0−01954−0−0195−0−1956−0−01957−0−01958−0−01959−0−01960−0−01960−06−28−0196−0−01962−0−01963−0−01964−0−1965−0−0−196−0196−01968−0−01969−0−01970−0−0197−0197−0−01972−0−01973−0−01974−0−1975−0−01975−05−−0−01976−0−0−01976−03−081977−0−0197−01979−0−0−01980−0−−0198−0198−01982−0−01983−0−01984−0−2−1985−0−01986−0−01986−04−21987−0−0−01988−0−01988−03−2−0198−0−019−03−0819−01919−019−019−197−019−0190−019−019−02−0−0193−0−019−04−0−0194−0−0195−0195−08−−0195−0−0195−195−−0196−0−0196−0−0196−0−0196−0−0196−02−02−0196−02−05−196−02−0−0196−02−2−196−02−2−25196−02−23−3196−02−28196−03−0−0196−03−08−1946−03−0−196−03−−2196−03−221986−03−23196−03−21956−03−31926−04−02196−04−03196−04−05196−04−196−04−196−04−23196−04−2196−04−28196−05−0196−05−03196−05−196−05−1956−05−22196−05−24196−05−3196−06−05196−06−07196−06−196−06−2196−06−28196−06−2196−07−03196−07−196−07−196−07−196−07−196−07−22196−07−21976−07−3196−08−02196−08−07196−08−0196−08−196−08−21946−08−23196−08−3196−0196−0196−01926−01936−01976−0196−0196−0196−0196−0196−0196−−04196−−0196−−196−−196−−196−−196−−20196−−241936−−250−041946−−270−05196−−280−1986−0−196−0−196−0−196−0−196−0−25196−0−26196−0−30196−196−196−196−−01986−−08196−−197−0−197−0−222−06197−0−2−197−0−32−197−02−1937−02−21957−02−02−25197−02−2−2197−02−2197−02−28197−03−051937−03−0−01957−03−−1987−03−2−197−03−26−24197−03−28−2197−04−04−3197−04−197−04−197−04−22197−04−25197−04−3197−05−0197−05−021947−05−0197−05−197−05−197−05−23197−05−31947−06−06197−06−197−06−2197−06−27197−07−04197−07−1987−08−0197−08−0197−08−197−08−22197−08−197−0197−01947−01967−197−197−197−1937−197−198−0198−0198−0−00−198−01−−2−198−01−23−262−21958−01−2−25198−02−02−26198−02−062−3198−02−11998−04−8−02−131998−108−02−201998−1078−03−06−018−03−1−098−03−1−168−03−18−03−8−03−1

<h2>Release Date</h2>

<ul>

 <li>Question: How does occupation relate to mean ratings?</li>

 <li>Answer: The occupations that tend to rate movies the highest are the unemployed, doctors, lawyers, educators, and artists. Since the unemployed are the easiest to please we might consider focusing on this group. However, the unemployed may not have as much money to spend on movies, so consider next what types of films are best liked by lawyers.</li>

</ul>

<h1>Easiest Types of Occupations to Entertain</h1>

The 5 highest mean movie ratings when grouped by occupation

<table width="582">

 <tbody>

  <tr>

   <td width="70"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="67"> </td>

  </tr>

  <tr>

   <td width="70"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="67"> </td>

  </tr>

  <tr>

   <td width="70"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="67"> </td>

  </tr>

  <tr>

   <td width="70"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="67"> </td>

  </tr>

  <tr>

   <td width="70"> </td>

   <td width="111"></td>

   <td width="111"></td>

   <td width="111"></td>

   <td width="111"> </td>

   <td width="67"> </td>

  </tr>

  <tr>

   <td width="70"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="67"> </td>

  </tr>

  <tr>

   <td width="70"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="67"> </td>

  </tr>

  <tr>

   <td width="70"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="67"> </td>

  </tr>

  <tr>

   <td width="70"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="111"> </td>

   <td width="67"> </td>

  </tr>

 </tbody>

</table>

3

2

1

0

artist                         educator                        doctor                          lawyer                          none

Occupation

<h1>Favorite Genres of Lawyer</h1>

<h2><strong>Problem 2: Expand our investigation to histograms</strong></h2>

<strong>An obvious issue with any inferences drawn from Problem 1 is that we did not consider how many times a movie was rated.</strong>

<ul>

 <li>Plot a histogram of the ratings of all movies.</li>

</ul>

<h1>Movie Ratings</h1>

<ul>

 <li>Plot a histogram of the number of ratings each movie received.</li>

</ul>

## Joining, by = “movie_title”

<h1>Movie Rating Frequencies Distribution</h1>

Movie Rating Frequencies Distribution Frequencies for movies rated 500 times or fewer

<ul>

 <li>Plot a histogram of the average rating for each movie.</li>

 <li>Plot a histogram of the average rating for movies which are rated more than 100 times.

  <ul>

   <li>Notice that when we include movies with 100 or fewer ratings, there are more mean ratings on the ends of the distribution. So when we reduce the dataset to just films with more than 100 ratings, the distribution of rating means tends to have lower variance.</li>

   <li>Generally speaking, it is better to trust that a movie with high mean rating and a high number (&gt;100) of critic ratings than a movie with high mean rating and a low number (&lt;= 100) of critic ratings. The reason is that infrequently rated movies are more likely to have very high or very low mean. In contrast, frequently rated films are more likely to have a moderate mean rating (between 2 and 4). So a frequently rated film with high mean rating is robust to increases in the number of ratings while an infrequently rated film with high mean may have a high mean due to chance.</li>

  </ul></li>

</ul>

<h1>Distribution of Mean Rating of Films</h1>

<table width="569">

 <tbody>

  <tr>

   <td width="42"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td width="61"> </td>

   <td width="38"> </td>

   <td width="0"></td>

  </tr>

  <tr>

   <td rowspan="2" width="42"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td rowspan="2" width="61"> </td>

   <td rowspan="2" width="38"> </td>

   <td width="0"></td>

  </tr>

  <tr>

   <td width="37"> </td>

   <td width="24"> </td>

  </tr>

  <tr>

   <td rowspan="2" width="42"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td rowspan="2" width="37"> </td>

   <td rowspan="2" width="24"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td rowspan="2" width="61"> </td>

   <td rowspan="2" width="38"> </td>

   <td width="0"></td>

  </tr>

  <tr>

   <td width="24"> </td>

   <td width="37"> </td>

  </tr>

  <tr>

   <td width="42"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td width="37"> </td>

   <td width="24"> </td>

   <td width="24"> </td>

   <td width="37"> </td>

   <td colspan="2" width="61"> </td>

   <td width="61"> </td>

   <td width="38"> </td>

   <td width="0"></td>

  </tr>

  <tr>

   <td rowspan="2" width="42"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td rowspan="2" width="37"> </td>

   <td rowspan="2" width="24"> </td>

   <td rowspan="2" width="24"> </td>

   <td rowspan="2" width="37"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td rowspan="2" width="61"> </td>

   <td rowspan="2" width="38"> </td>

   <td width="0"></td>

  </tr>

  <tr>

   <td width="37"> </td>

   <td width="24"> </td>

  </tr>

  <tr>

   <td width="42"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td width="37"> </td>

   <td width="24"> </td>

   <td width="24"> </td>

   <td width="37"> </td>

   <td width="37"> </td>

   <td width="24"> </td>

   <td width="24"> </td>

   <td width="37"> </td>

   <td colspan="2" width="61"> </td>

   <td width="61"> </td>

   <td width="38"> </td>

   <td width="0"></td>

  </tr>

  <tr>

   <td width="42"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td width="37"> </td>

   <td width="24"> </td>

   <td width="24"> </td>

   <td width="37"> </td>

   <td width="37"> </td>

   <td width="24"> </td>

   <td width="24"> </td>

   <td width="37"> </td>

   <td colspan="2" width="61"></td>

   <td width="61"> </td>

   <td width="38"> </td>

   <td width="0"></td>

  </tr>

  <tr>

   <td width="42"> </td>

   <td colspan="2" width="61"></td>

   <td colspan="2" width="61"> </td>

   <td width="37"> </td>

   <td width="24"> </td>

   <td width="24"> </td>

   <td width="37"> </td>

   <td width="37"> </td>

   <td width="24"> </td>

   <td width="24"> </td>

   <td width="37"> </td>

   <td colspan="2" width="61"> </td>

   <td width="61"> </td>

   <td width="38"> </td>

   <td width="0"></td>

  </tr>

  <tr>

   <td rowspan="3" width="42"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" rowspan="2" width="61"> </td>

   <td rowspan="3" width="37"> </td>

   <td rowspan="3" width="24"> </td>

   <td rowspan="3" width="24"> </td>

   <td rowspan="3" width="37"> </td>

   <td rowspan="3" width="37"> </td>

   <td rowspan="3" width="24"> </td>

   <td rowspan="3" width="24"> </td>

   <td rowspan="3" width="37"> </td>

   <td colspan="2" width="61"> </td>

   <td rowspan="3" width="61"></td>

   <td rowspan="3" width="38"> </td>

   <td width="0"></td>

  </tr>

  <tr>

   <td rowspan="2" width="37"> </td>

   <td rowspan="2" width="24"> </td>

   <td rowspan="2" width="37"> </td>

   <td rowspan="2" width="24"> </td>

   <td width="0"></td>

  </tr>

  <tr>

   <td width="24"> </td>

   <td width="37"> </td>

   <td width="0"></td>

  </tr>

  <tr>

   <td width="42"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td colspan="2" width="61"> </td>

   <td width="61"> </td>

   <td width="38"> </td>

   <td width="0"></td>

  </tr>

  <tr>

   <td width="42"></td>

   <td width="37"></td>

   <td width="24"></td>

   <td width="24"></td>

   <td width="37"></td>

   <td width="37"></td>

   <td width="24"></td>

   <td width="24"></td>

   <td width="37"></td>

   <td width="37"></td>

   <td width="24"></td>

   <td width="24"></td>

   <td width="37"></td>

   <td width="37"></td>

   <td width="24"></td>

   <td width="62"></td>

   <td width="38"></td>

   <td width="0"> </td>

  </tr>

 </tbody>

</table>

200

150

100

50

0

1                                      2                                       3                                      4                                      5

Mean Rating

<h1>Distribution of Mean Rating of Films</h1>

<ul>

 <li>Make some conjectures about the distribution of ratings!</li>

</ul>

<ul>

 <li>Question: We saw that movies with a large number of ratings or few ratings may tend to have more extreme results. Do films with a large number of ratings do better or worse than those with a moderate number of ratings? What about films with very few ratings.</li>

 <li>Answer: It looks like films that are rated often tend to have a higher mean rating compared to the whole dataset while films that have very few ratings have a lower mean rating compared to the whole dataset.</li>

</ul>

count_deciles = quantile(mlData_aggregates$rating_count, c(.1, .2, .3, .4, .5, .6, .7, .8, .9)) count_deciles

##        10%      20%      30%      40%      50%      60%      70%      80%      90%

##         2.0                      6.0 12.0 22.0 42.0 65.6 108.0 188.4 363.9

<table width="632">

 <tbody>

  <tr>

   <td width="632">oft_rated_films_df &lt;- mlData %&gt;% group_by(movie_title) %&gt;% summarise( rating_count = n(),median_rating = median(rating), mean_rating = mean(rating)) %&gt;% filter(rating_count &gt; count_deciles[9]) head(oft_rated_films_df)</td>

  </tr>

 </tbody>

</table>

## # A tibble: 6 x 4

##           movie_title                                                   rating_count median_rating mean_rating

##         &lt;fct&gt;                                                                          &lt;int&gt;                       &lt;dbl&gt;                  &lt;dbl&gt;

<table width="502">

 <tbody>

  <tr>

   <td width="3"> </td>

   <td width="293">## 1 2001: A Space Odyssey (1968)</td>

   <td width="119">1036</td>

   <td width="63">4</td>

   <td width="28">3.97</td>

   <td width="127"> </td>

  </tr>

  <tr>

   <td width="3"> </td>

   <td width="293">## 2 Abyss, The (1989)</td>

   <td width="119">604</td>

   <td width="63">4</td>

   <td width="28">3.59</td>

   <td width="127"> </td>

  </tr>

  <tr>

   <td width="3"> </td>

   <td width="293">## 3 African Queen, The (1951)</td>

   <td width="119">608</td>

   <td width="63">4</td>

   <td width="28">4.18</td>

   <td width="127"> </td>

  </tr>

  <tr>

   <td width="3"> </td>

   <td width="293">## 4 Air Force One (1997)</td>

   <td width="119">862</td>

   <td width="63">4</td>

   <td width="28">3.63</td>

   <td width="127"> </td>

  </tr>

  <tr>

   <td width="3"> </td>

   <td width="293">## 5 Aladdin (1992)</td>

   <td width="119">876</td>

   <td width="63">4</td>

   <td width="28">3.81</td>

   <td width="127"> </td>

  </tr>

  <tr>

   <td width="3"> </td>

   <td width="293">## 6 Alien (1979)</td>

   <td width="119">1164</td>

   <td width="63">4</td>

   <td width="28">4.03</td>

   <td width="127"> </td>

  </tr>

  <tr>

   <td colspan="6" width="632">rarely_rated_films_df &lt;- mlData %&gt;% group_by(movie_title) %&gt;% summarise( rating_count = n(),median_rating = median(rating), mean_rating = mean(rating)) %&gt;% filter(rating_count &lt; count_deciles[3]) head(rarely_rated_films_df)</td>

  </tr>

  <tr>

   <td width="3"></td>

   <td width="280"></td>

   <td width="102"></td>

   <td width="54"></td>

   <td width="28"></td>

   <td width="58"></td>

  </tr>

 </tbody>

</table>

## # A tibble: 6 x 4

##           movie_title                                                                             rating_count median_rating mean_rating

##         &lt;fct&gt;                                                                                                    &lt;int&gt;                       &lt;dbl&gt;                  &lt;dbl&gt;

## 1 1-900 (1994)               5              3              2.6 ## 2 3 Ninjas: High Noon At Mega Mountain (~     10           1              1

## 3 8 Heads in a Duffel Bag (1997)                                                                    4                              4                   3.25

## 4 8 Seconds (1994)       4              4              3.75 ## 5 A Chef in Love (1996)        8              4              4.12

## 6 Á köldum klaka (Cold Fever) (1994)                                                            2                              3                   3

mean(oft_rated_films_df$mean_rating)

## [1] 3.631668

mean(rarely_rated_films_df$mean_rating)

## [1] 2.621521

mean(mlData_aggregates$mean_rating)

## [1] 3.078132

<h2><strong>Problem 3: Correlation: Men versus women</strong></h2>

<strong>Let us look more closely at the relationship between the pieces of data we have.</strong>

<ul>

 <li>Make a scatter plot of men versus women and their mean rating for every movie.</li>

 <li>Make a scatter plot of men versus women and their mean rating for movies rated more than 200 times.</li>

 <li>Compute the correlation coefficent between the ratings of men and women.

  <ul>

   <li>When we compare mean ratings between men and women while including movies with 100 or fewer ratings, the correlation between mean rating among men and mean rating among women appears positive but not very strong for prediction. The correlation coefficient in this case is 0.5149489. When considering movies with more than 100 ratings the correlation is stronger with a correlation coefficient in this case of 0.8042434.</li>

   <li>Considering movies with more than 100 ratings, the relationship between mean men rating and mean women rating is linear for the most part. This is more true near the mean of the mean ratings (about 3.5) where a rating of about 3.5 among men corresponds to a mean rating of about 3.5 among women. The relation appears not quite linear for high and low mean ratings.</li>

  </ul></li>

</ul>

## Warning: Removed 217 rows containing missing values (geom_point).

<h1>Relationship Between Ratings by Women vs. Men</h1>

## `geom_smooth()` using formula ‘y ~ x’

## `geom_smooth()` using method = ‘loess’ and formula ‘y ~ x’

<h1>Relationship Between Ratings by Women vs. Men</h1>

Mean rating comparison among films with over 200 ratings

<ul>

 <li>Conjecture under what circumstances the rating given by one gender can be used to predict the rating given by the other gender.</li>

</ul>

<strong>– </strong>Question: Are men and women more similar when they are younger or older?

<strong>–</strong>

<table width="632">

 <tbody>

  <tr>

   <td width="632">mlData_aggregates &lt;- mlData %&gt;% group_by(movie_title) %&gt;% filter(gender == “M”, age &gt; 40) %&gt;%summarise(mean_rating_men40plus = mean(rating)) %&gt;%full_join(mlData_aggregates)</td>

  </tr>

 </tbody>

</table>

## Joining, by = “movie_title”

<table width="632">

 <tbody>

  <tr>

   <td width="632">mlData_aggregates &lt;- mlData %&gt;% group_by(movie_title) %&gt;% filter(gender == “W”, age &gt; 40) %&gt;%summarise(mean_rating_women40plus = mean(rating)) %&gt;% full_join(mlData_aggregates)</td>

  </tr>

 </tbody>

</table>

## Joining, by = “movie_title”

<table width="632">

 <tbody>

  <tr>

   <td width="632">mlData_aggregates &lt;- mlData %&gt;% group_by(movie_title) %&gt;% filter(gender == “M”, age &lt; 30) %&gt;%summarise(mean_rating_men30minus = mean(rating)) %&gt;% full_join(mlData_aggregates)</td>

  </tr>

 </tbody>

</table>

## Joining, by = “movie_title”

<table width="632">

 <tbody>

  <tr>

   <td width="632">mlData_aggregates &lt;- mlData %&gt;% group_by(movie_title) %&gt;% filter(gender == “W”, age &lt; 30) %&gt;%summarise(mean_rating_women30minus = mean(rating)) %&gt;% full_join(mlData_aggregates)</td>

  </tr>

 </tbody>

</table>

## Joining, by = “movie_title”

<table width="632">

 <tbody>

  <tr>

   <td width="632">old_scatterplot &lt;- mlData_aggregates %&gt;% ggplot(aes(x = mean_rating_men40plus, y = mean_rating_women40plus)) + geom_point(aes(color = rating_count), alpha = .5) + labs(title = “Relationship Between Ratings by Women vs. Men Over 40”, x = “Mean Rating Among Men”, y = “Mean Rating Among Women”, color = “Number of Ratings”) old_scatterplot</td>

  </tr>

 </tbody>

</table>

## Warning: Removed 1662 rows containing missing values (geom_point).

<h1>Relationship Between Ratings by Women vs. Men Over 40</h1>

<table width="632">

 <tbody>

  <tr>

   <td width="632">young_scatterplot &lt;- mlData_aggregates %&gt;% ggplot(aes(x = mean_rating_men30minus, y = mean_rating_women30minus)) + geom_point(aes(color = rating_count), alpha = .5) + labs(title = “Relationship Between Ratings by Women vs. Men Over 40”, x = “Mean Rating Among Men”,</td>

  </tr>

  <tr>

   <td width="632">y = “Mean Rating Among Women”, color = “Number of Ratings”) young_scatterplot</td>

  </tr>

 </tbody>

</table>

## Warning: Removed 1662 rows containing missing values (geom_point).

<h1>Relationship Between Ratings by Women vs. Men Over 40</h1>

<table width="632">

 <tbody>

  <tr>

   <td width="632">gender_mean_corr_old = cor( mlData_aggregates$mean_rating_men40plus, mlData_aggregates$mean_rating_women40plus, )gender_mean_corr_young = cor( mlData_aggregates$mean_rating_men30minus, mlData_aggregates$mean_rating_women30minus)</td>

  </tr>

 </tbody>

</table>

<h2><strong>Problem 4: Open Ended Question: Business Intelligence</strong></h2>

<ul>

 <li>From the exploration, I would suggest marketing films to lawyers, doctors, and educators of both genders. If we have discovered anything from this dataset, it is that men and women really do not differ significantly in their preferences and trying to make business decisions based on this factor is not recommended. Consider marketing Film-Noir and War Movies. If the film is released and not well received by the first few critics, elicit ratings from more critics. Generally, by increasing the number of ratings, the film is likely to improve it’s overall mean and median ratings.</li>

</ul>