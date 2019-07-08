# SQL-questions

Highschooler ( ID, name, grade ) 
English: There is a high school student with unique ID and a given first name in a certain grade. 

Friend ( ID1, ID2 ) 
English: The student with ID1 is friends with the student with ID2. Friendship is mutual, so if (123, 456) is in the Friend table, so is (456, 123). 

Likes ( ID1, ID2 ) 
English: The student with ID1 likes the student with ID2. Liking someone is not necessarily mutual, so if (123, 456) is in the Likes table, there is no guarantee that (456, 123) is also present. 

4. Find the number of students who are either friends with Cassandra or are friends of friends of Cassandra. Do not count Cassandra, even though technically she is a friend of a friend. 

My solution
'''sql
select count(*)
from (select h2.name 
from friend, highschooler h1, highschooler h2
where friend.id1 = h1.id and friend.id2 = h2.id and h1.name = 'Cassandra'
union all
select h2.name 
from friend f1, friend f2, highschooler h1, highschooler h2
where (f1.id1 = h1.id and f1.id2 = f2.id1 and f2.id2 = h2.id) and h1.name = 'Cassandra' and h2.name <> 'Cassandra')
'''

'''sql
select count(*)
FROM Friend
WHERE ID1 IN (
  SELECT ID2
  FROM Friend
  WHERE ID1 IN (
    SELECT ID
    FROM Highschooler
    WHERE name = 'Cassandra'
  )
);
'''
