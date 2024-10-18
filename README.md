# BCG Job Simulation
## Introduction
PowerCo, our client in this project, is a major gas and electricity utility that supplies to enterprises. Due to the rise of competition in the market, PowerCo’s concern about customer churning has increased in recent years. This brings them to BCG, so we will help solve the root reasons for customers churning.       
   
 **Languages and libraries**
 - Python
 - Pandas
 - NumPy
 - Seaborn
 - Matplotlib
 - Scikit-Learn
 - VIF (Variance Inflation Factor)
 - DateTime

## Exploratory Data Analysis
After writing an email and "emailing" the client, I was sent two datasets. The first is the customer data which has variables like usage, sign-up date, forecasted usage, net margin, churn, etc. The second was the price data which has the customer, price date, and various prices of energy and power from periods 1-3.    

To find reasons clients have churned, I had to look at the data. The best way to do so was through visualizations. The following images are some that I looked at.    
![image](https://github.com/user-attachments/assets/b7b4055d-b9e1-431d-8f9d-14f490cfe2d0)   
![image](https://github.com/user-attachments/assets/5ea15707-0454-4e7c-8a61-745dcaa8f9e0)   
The first thing I wanted to see was the amount and percentage of clients that churned over the next 3 months. So we can see here that roughly 1400 clients have churned while about 13000 have stayed with PowerCo. That is 9.715% of clients lost which is not desirable.    
    
![image](https://github.com/user-attachments/assets/f47ddc4d-6e43-4b82-8c51-84f44f28804e)
In this graph, I wanted to see if clients were more likely to churn whether they had gas or not. I found that there was only a 2% difference that churned with and without gas. This showed me that it likely did not impact the chance a client churned.    
    
![image](https://github.com/user-attachments/assets/b0df7f61-004a-4bbd-a1f8-d7f09578ee36)
![image](https://github.com/user-attachments/assets/b9a3c8f7-39d2-47ad-b0f7-5cf1e2caebde)
Next, I wanted to view the percentage of clients that did churn in what sales channel it occurred in. I will fully state now that I realized this could be presented better which is why I will explain it. Out of the about 1400 clients that churned 57.79% of them were from 1st performing channel in gas consumption and 2nd in electricity consumption. This is a major concern. Due to the little knowledge I have of the sales channel, I can only say that there is a high chance an issue is occurring or related to that channel that is causing clients to churn.    
     
![image](https://github.com/user-attachments/assets/c91c0b2c-6c94-4657-87ee-fc2e62878db4)
In this last graph I will be showing, we can see the percentage and amount of clients that churned compared to the number of years they have been with PowerCo. There would appear to be a slight negative correlation between the two. As we go further in years with the company, the percentage that churn becomes lower and lower. Despite the sudden rise near the end, it is safe to say that correlation exists.    
    
After completing some EDA, I moved on to the next step.
## Feature Engineering
The objective of this step was to add, modify, remove, and merge columns. This was done to both enrich the dataset and to improve the model's performance.    
    
To start, I began to create columns based on those given. Using the price data, I created columns based on the difference between the last price mid-peak for both power and electricity and the first price. Then I created new columns based on both the sum and average of the price columns.    
    
With the client data, I first used LabelEncoder on the object(string) columns so that they could be used for training the model. Then I separated each date column by year, month, and day.     
    
Afterwards, I used VIF which helped to determine if different variables are highly correlated and are not churn. This is to prevent multicollinearity which can affect how the model is trained. I found that several columns appeared to be extremely correlated with each other. To further confirm this I created a heatmap.    
![image](https://github.com/user-attachments/assets/cf176f4e-b405-42bb-8c15-08726a56b00a)
In this heat map, we can see there is a lot to look at. First, we can see that the brightest spots appear to have a very high positive correlation that nearly is or has reached 1.0. We could also see that no features appear to have a high correlation with churn.    
      
Through the heatmap, I removed variables that had equal to or above a 0.95 correlation with another variable. I ended up dropping about 20 columns. Then I calculated the VIF data once again to remove any columns 10 or above in VIF. This dropped about 13 columns. This ended with the following heatmap.
![image](https://github.com/user-attachments/assets/43026355-9db2-4907-8963-dcc68e888a83)
As can be seen here, the map is less bright all over and more consistent throughout. With that completed I moved to the next step.
## Model Training
This task was without a doubt the shortest. With a dataset provided called data_for_predictions, I created a RandomForest model with the parameters (n_estimators=150, min_impurity_decrease = 0.000001, random_state = 42). I found that 150 improved the model by 0.1% which is not much but is more than any other test I made. The same could be said with using 0.000001. Now 42 is a given. With the provided dataset, I was able to have a model with an accuracy of 90.5%. Now compare that with my dataset, which resulted in a model accuracy of 90.4%. I found that the dataset provided several created columns that my dataset lacked. This gave it the edge over mine but I am content with the fact I was very close to the experts after Feature Engineering.

## Conculusion
In the end, a model was created that predicted if a client churned with 90% accuracy. This is solid but can not be fully trusted as a tendency to believe a client does not churn when they will. The model said that 3630 did not churn when in fact it was 3285. This error could cause the loss of clients if fully trusted so it is best to take it with caution in mind.
