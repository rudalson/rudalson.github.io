---
layout: post
title:  "Hackerrank - SQL - 01."
categories: Hackerrank
tags: sql hackerrank
---
SQL이 약해서 익히기 위해 Hackerrank를 풀어봤다.

```
select distinct city 
from station 
where city REGEXP '^[aeiou]' AND city regexp '[aeiou]$';

SELECT DISTINCT CITY FROM STATION
WHERE CITY REGEXP '^[aeiou].*[aeiou]$';
```




## Question
Query the Name of any student in STUDENTS who scored higher than  Marks. Order your output by the last three characters of each name. If two or more students both have names ending in the same last three characters (i.e.: Bobby, Robby, etc.), secondary sort them by ascending ID.

Input Format

The STUDENTS table is described as follows: The Name column only contains uppercase (A-Z) and lowercase (a-z) letters.

Sample Input



Sample Output

Ashley
Julia
Belvet
Explanation

Only Ashley, Julia, and Belvet have Marks > . If you look at the last three characters of each of their names, there are no duplicates and 'ley' < 'lia' < 'vet'.


### My solution
```sql
select name from students where marks > 75 order by right(name, 3), id asc;
```



## The PADS
Generate the following two result sets:

Query an alphabetically ordered list of all names in OCCUPATIONS, immediately followed by the first letter of each profession as a parenthetical (i.e.: enclosed in parentheses). For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S).
Query the number of ocurrences of each occupation in OCCUPATIONS. Sort the occurrences in ascending order, and output them in the following format: 

There are a total of [occupation_count] [occupation]s.
where [occupation_count] is the number of occurrences of an occupation in OCCUPATIONS and [occupation] is the lowercase occupation name. If more than one Occupation has the same [occupation_count], they should be ordered alphabetically.

Note: There will be at least two entries in the table for each type of occupation.

Input Format

The OCCUPATIONS table is described as follows: Occupation will only contain one of the following values: Doctor, Professor, Singer or Actor.

Sample Input

An OCCUPATIONS table that contains the following records:




### My solution
```sql
select concat(name, "(", left(occupation, 1), ")") from OCCUPATIONS order by name;

select 
  concat("There are a total of ", count(occupation), " ", lower(occupation), "s.")
from OCCUPATIONS
group by occupation
order by count(occupation), occupation
;
```