## A Drop in User Engagement at Yammer

In this guided project, we'll be investigating a drop in user engagement at Yammer – a social network for communicating with coworkers. Individuals share documents, updates, and ideas by posting them in groups. Yammer is free to use indefinitely, but companies must pay license fees if they want access to administrative controls, including integration with user management systems like ActiveDirectory.

Yammer has a centralized Analytics team, which sits in the Engineering organization. Their primary goal is to drive better product and business decisions using data. They do this partially by providing tools and education that make other teams within Yammer more effective at using data to make better decisions. They also perform ad-hoc analysis to support specific decisions.

### Assignement 

You show up to work Tuesday morning, September 2, 2014. The head of the Product team walks over to your desk and asks you what you think about the latest activity on the user engagement dashboards. You fire them up, and something immediately jumps out:

![Screen Shot 2022-10-24 at 3 53 54 PM](https://user-images.githubusercontent.com/95102899/197645002-d6156028-f7c9-4696-b7ef-e6ff209fc290.png)

The above chart shows the number of engaged users each week. Yammer defines engagement as having made some type of server call by interacting with the product (shown in the data as events of type "engagement"). Any point in this chart can be interpreted as "the number of users who logged at least one engagement event during the week starting on that date."

You are responsible for determining what caused the dip at the end of the chart shown above and, if appropriate, recommending solutions for the problem.

### 1.Let's first come up with a list of possible causes for the dip in retention shown in the chart above. After that, I will check every hypothesis one by one and query my data to accept or deny the hypothesis. 

- People went on vacation in the end of July + August 
- Long weekend/ Holidays (could be combined with the first hypothesis)
- Lay-offs
- Broken tracking system
- Broken website/app

Now, let's dig into our tables and see what they look like. 

![Screen Shot 2022-10-24 at 3 44 28 PM](https://user-images.githubusercontent.com/95102899/197645058-ebedf1a4-b9d3-4410-adae-acd97df1624d.png)


![Screen Shot 2022-10-24 at 3 48 06 PM](https://user-images.githubusercontent.com/95102899/197645066-d1dce5ab-c751-41aa-bcbd-dc7e7a4100c5.png)


![Screen Shot 2022-10-24 at 4 09 02 PM](https://user-images.githubusercontent.com/95102899/197646917-d3912955-9cfe-41e9-9622-f6c4d7c8eecc.png)


![Screen Shot 2022-10-24 at 4 10 19 PM](https://user-images.githubusercontent.com/95102899/197646926-dbea3e91-e683-4ef1-86cc-3870ff5f2d76.png)
