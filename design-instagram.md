# Designing Instagram User Feed

Instagram is a huge system itself and if we can possibly not discuss the entire design in a 50 to 60 minute Design Interview. So we will be selecting some essential features and designing a system which will serve these features. We will also be discussing the trade offs in our design, improving the design and making it scalable to a large user base. The features which we will gonna be including are:



1. Presenting User’s Feed
2. Like a Post
3. Comment on a Post
4. Followers and Followee concept



## Designing the System

Initially we can begin with three tables : Posts Table, Likes Table and Comments Table. The schema of our Posts Table will look like this :



| Post_ID | User_ID | timestamp | comments_count | likes_count | posts_count |
| :------ | :------ | :-------- | :------------- | :---------- | :---------- |
|         |         |           |                |             |             |

The table will store every post in a row with its unique **Post_ID** along with the **User_ID** of the user who posted it and the **timestamp** storing the time when the post was uploaded by the user.

The Likes Table will have the following schema :



| Like_ID | Post_ID | User_ID | timestamp |
| :------ | :------ | :------ | :-------- |
|         |         |         |           |

The Comments Table will also have the similar structure :



| Comment_ID | Post_ID | User_ID | timestamp |
| :--------- | :------ | :------ | :-------- |
|            |         |         |           |

Every Like event will be stored in the Like Table as a new row with a unique **Like_ID** along with the **Post_ID** to which the like is given and the **User_ID** describing the user who gave that like and the **timestamp** storing the time when the like event was triggered. The Exact same thing goes with the Comments Table.



## Discussing the Likes and Comments Event

On Instagram you can probably like the posts of the other users. Following our earlier discussed schema if **User_1** likes a particular post with id **Post_1** at a particular time **t1**, then a like event will be triggered and will be stored in our **Likes** table in the following manner.



| Like_ID  | Post_ID | User_ID | timestamp |
| :------- | :------ | :------ | :-------- |
| Like_001 | Post_1  | User_1  | t1        |

When we need to show the number of likes on a particular post, then we can query the likes table to count the number of like events with the particular post id. Suppose we need to find the number of likes on a post having id **Post_1**, then we could make the following query :



**like_count** : SELECT COUNT(*) FROM Likes TABLE WHERE POST_ID = Post_1


This is a naive approach and will not scale well for a large number of posts or various posts having huge numbers of likes. Querying the database for computing the number of likes every single time is probably a bad idea.

We can also follow another approach of storing the count of likes for a particular post in the post table itself and updating the count every time someone likes that post. This is a much better approach as when we are required to give the number of likes for a particular post then we need to simply query the likes_count from the Posts table for that particular post id.

Like event triggered on **Post_1** :

Increment **likes_count** for **Post_1**


When we need to fetch the number of likes on **Post_1** :

Fetch the **likes_count** for **Post_1**


Same approach can be followed for implementing Comments. Instagram allows users to reply to a particular comment. But it does not allow users to again reply over that early reply on the comment. So it allows users to go one level deep in the recursive comment actions. But here we will be keeping things simple and say that no further comments can be made over a comment.

Followers and Followee over Instagram

On Instagram a particular user can follow other users. This feature could be achieved by designing a single table. The table schema for follower-followee table looks like this :



| Follower_ID | Followee_ID | timestamp |
| :---------- | :---------- | :-------- |
|             |             |           |

Suppose a user with ID User_1 follows another user with ID User_2 at time t2 then an entry will be made in the follower-followee table in this manner :



| Follower_ID | Followee_ID | timestamp |
| :---------- | :---------- | :-------- |
| User_1      | User_2      | t2        |

So when we need to find the number of followers for a particular user with id User_1 then we need to make a query in the follower-followee table as follows :



**number_of_followers** : SELECT COUNT(*) FROM follower-followee TABLE WHERE Followee_ID = User_1


In the same way when we need to find the number of people User_1 is following then our query will look like this :



**number_of_followees** : SELECT COUNT(*) FROM follower-followee TABLE WHERE Follower_ID = User_1



The approach of querying the **follower-followee** table every time when we need to compute the number of followers and followee is also a bad idea. In the case of computing this value for millions of users or for some famous users/ celebrities having millions of followers will probably be expensive.

This problem can be solved in the same way we solved the problem of computing the number of likes for a post earlier. We can store the count of follower and followee of a particular user as a separate column in the User Table. So when a user is being followed or if he/she follows any other user then we can increment the number of follower and followee for that User in its row. Hence when we are required to fetch the number of follower or followee for a user then we won’t be required to query the entire table every time, now we just need to fetch the follower and followee count from the User Table for that particular user.

You can also extend this design for achieving the Unfollowing feature of Instagram. This time instead of incrementing the values we will be decrementing the corresponding values in the table.



## Designing the User Feed

An Instagram User Feed is basically the list of posts of all the people you follow, sorted by the upload time so that the most recent post comes at the top and the old ones at the bottom.

If you need to build the feed for a user with id **User_1** then you will be querying the **Posts** table to find the posts of the users who lie in the followee list of **User_1** and then sort these posts by their **timestamp** and present it to the User.

Fetching the list of followee for **User_1** :



**followee_list** = SELECT Followee_ID FROM follower_followee TABLE WHERE Follower_ID = User_1


Now it's time to get the Feed for **User_1** :



**user_feed** = SELECT post_content FROM Posts TABLE WHERE User_ID IN followee_list ORDER BY timestamp DESC


That can be one way to build a user feed. But again querying the entire db two times for fetching the feed for users can be a bad idea. As there are millions of users over Instagram and they can request their feeds probably a lot of time, so there will be a huge number of db calls.

There can be another better approach. Instead of building the feed every time from scratch for every user, we can store the feed for every user in a local storage like a Cache. This will avoid building the feed every time a user requests for it. Now we need to keep this feed updated with all the latest incoming posts. How to achieve that ?

Suppose a user uploads a post on Instagram. Then his/her followers should be able to see that post. So whenever a user posts something we will be adding that post to the existing feed of their followers. This will keep the user feed updated and whenever a user requests for his/her feed, then we just have to query for it in the local storage and return it. We no longer have to make db calls every time a user demands for his/her feed.

The entire design of the System can look like this :

![img](https://systemsthatscale.org/problems/problem-media/insta-design.png)