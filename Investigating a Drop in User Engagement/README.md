## A Drop in User Engagement at Yammer

In this guided project, we'll be investigating a drop in user engagement at Yammer – a social network for communicating with coworkers. Individuals share documents, updates, and ideas by posting them in groups. Yammer is free to use indefinitely, but companies must pay license fees if they want access to administrative controls, including integration with user management systems like ActiveDirectory.

Yammer has a centralized Analytics team, which sits in the Engineering organization. Their primary goal is to drive better product and business decisions using data. They do this partially by providing tools and education that make other teams within Yammer more effective at using data to make better decisions. They also perform ad-hoc analysis to support specific decisions.

### Assignement.

You show up to work Tuesday morning, September 2, 2014. The head of the Product team walks over to your desk and asks you what you think about the latest activity on the user engagement dashboards. You fire them up, and something immediately jumps out:

![Screen Shot 2022-10-24 at 3 53 54 PM](https://user-images.githubusercontent.com/95102899/197645002-d6156028-f7c9-4696-b7ef-e6ff209fc290.png)

The above chart shows the number of engaged users each week. Yammer defines engagement as having made some type of server call by interacting with the product (shown in the data as events of type "engagement"). Any point in this chart can be interpreted as "the number of users who logged at least one engagement event during the week starting on that date."

You are responsible for determining what caused the dip at the end of the chart shown above and, if appropriate, recommending solutions for the problem.

### 1.List of hypotheses.

Let's first come up with a list of possible causes for the dip in retention shown in the chart above. After that, I will check every hypothesis one by one and query my data to accept or deny the hypothesis. 

- Growth rate went down 
- People went on vacation in the end of July + August 
- Long weekend/ Holidays (could be combined with the vacation hypothesis)
- Broken tracking system
- Broken website/app

### 2. Dataset.
Now, let's dig into our tables and see what they look like. 

**Users table** 

![Screen Shot 2022-10-24 at 3 44 28 PM](https://user-images.githubusercontent.com/95102899/197645058-ebedf1a4-b9d3-4410-adae-acd97df1624d.png)

**Events table** 

![Screen Shot 2022-10-24 at 3 48 06 PM](https://user-images.githubusercontent.com/95102899/197645066-d1dce5ab-c751-41aa-bcbd-dc7e7a4100c5.png)

**Emails table** 

![Screen Shot 2022-10-24 at 4 09 02 PM](https://user-images.githubusercontent.com/95102899/197646917-d3912955-9cfe-41e9-9622-f6c4d7c8eecc.png)

**Rollup period table** 

![Screen Shot 2022-10-24 at 4 10 19 PM](https://user-images.githubusercontent.com/95102899/197646926-dbea3e91-e683-4ef1-86cc-3870ff5f2d76.png)


### 3. Checking hypotheses. 

**Let's first check the growth rate.** 

```sql

SELECT 
  DATE_TRUNC('day', created_at) AS day,
  COUNT (*) AS all_users,
  COUNT (
    CASE 
      WHEN activated_at IS NOT NULL THEN user_id
      ELSE NULL 
    END
  ) AS activated_users
FROM 
  tutorial.yammer_users
WHERE 
  created_at >= '2014-05-01'
  AND created_at < '2014-09-01'
GROUP BY 
  1
ORDER BY 
  1
  
  ```
  
  As we see from the chart below, the growth rate didn't change – users are active during weekdays and are not so active during weekends. 
  
  ![Screen Shot 2022-11-03 at 12 40 50 PM](https://user-images.githubusercontent.com/95102899/199818980-5541ca8f-14f8-4530-be88-f786936b0bdd.png)
  
  
**Vacation/Holiday**

We can check this hypothesis by comparing the engagement rate over time in different countries. Since holidays are different across countries, there's a possibility that one or two countries would show a drop while the others would stay the same. 

On Mode, you can easily visualize your findings with a Tableau-like program. However, for my hypothesis, I needed to compare the engagement rate over time in 47 countries side by side. In my opinion, the easiest way to do that is to create a quick and simple visualization in RStudio. I queried my data first and then exported it into RStudio. 



```sql
SELECT
  location,
  DATE_TRUNC('week', occurred_at),
  COUNT (DISTINCT user_id) AS user_count
FROM
  tutorial.yammer_events
WHERE
  event_type = 'engagement'
  AND event_name = 'login'
GROUP BY
  location,
  2
ORDER BY
  2,
  3 DESC
  
 ```
 
 ```r
 install.packages('tidyverse')
library(tidyverse)
str(yammer_countries)
head(yammer_countries)

install.packages('ggplot2')
library(ggplot2)

ggplot(yammer_countries, aes(x=date_trunc, y=user_count))+
  geom_line()+
  facet_wrap(~location)
 ```
 
 ![Screen Shot 2022-11-03 at 12 48 13 PM](https://user-images.githubusercontent.com/95102899/199819890-9e73852b-2cc2-4fa0-ba80-e5f8132cff68.png)
 
 As we see in the facet wrap chart above, the line goes down drastically for engagement in the US. However, we can't accept our hypothesis simply because the US has a lot more users in general, and therefore the line would show a bigger drop. If you look closely at other countries  with many users (Germany, Australia, Brazil, France etc), you'll see a drop among all of them in the same place on the timeline – August. 
 
 **Therefore, we are not accepting the hypothesis about the engagement drop because of vacation/holiday.**

#### Type of Device.

 Let's check if the drop in engagement happened on all devices at the same time or if it happened only on one device. To do that, we will first check what type of devices the users log in on and engage from. 

```sql
SELECT DISTINCT device
FROM tutorial.yammer_events
```
![Screen Shot 2022-11-05 at 4 07 22 PM](https://user-images.githubusercontent.com/95102899/200145043-fe32aa83-19ed-4bf3-90a2-9f3157d495ae.png)

Now, I will go ahead and classify all devices into 3 categories: *phone, computer, and tablet.* I will have to google some of the devices if the names are unfamiliar. I will use these classifications in my analysis. 



 
