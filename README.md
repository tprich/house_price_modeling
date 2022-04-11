# Housing Prices - Linear Regression & Kaggle Competition

## Some Background

This project focuses on housing data from Ames. The goal is to create a Regression model that takes the given data and predicts the sale price of a house. In order to test just how well the model works, a separate test dataset was provided that did not include the sale price, meaning there was no way to confirm how well the model's predictions did. That is where [Kaggle](https://www.kaggle.com/) comes in. Once the model predicts the price of the houses on the test data set, the predictions are then uploaded to Kaggle and scored accordingly. This is a contest between classmates, and whoever has the lowest score at the end wins.


## Problem Statement

The first things that people look at when buying a house is the number of bedrooms and bathrooms, the overall square footage, and the price. The thing is there are so many other things factoring into how much the house will sell for. One day you may want to sell your house, and you want to get the most for it that you can. It's usually not feasable to build new rooms onto the house, but there are other things you can do besides a fresh coat of paint to increase your house's worth. After researching and modeling home prices in Ames, I will provide recommendations on what can be done to up the selling price. 

## Description of Data

There were three datasets provided to me: train.csv, test.csv, and sample_sub_reg.csv.

The train.csv dataset is where most of the work was done. The models were built using it and it had the sale price included so that I could evaluate just how well the features worked together prior to submitting to Kaggle. The final version of this dataset at the end of every notebook was saved to the cleaned_data directory, and any time an updated training dataframe needed to be exported for a submission, it was saved to the ready_for_model directory.

The test.csv dataset did not have the sale price, so it couldn't be used to evaluate models in Jupyter Notebook. The model created using the training set was to process the test dataset and create predictions. Those predictions were then adjusted to reflect the layout of sample_sub_reg.csv. Once adjusted, the predictions could then be submitted to Kaggle and a score generated. The lower the score the better.


## Analyzing, Visualizing, and Modeling the Data

My process was broken down into 4 sections. When transferring features, I kept them as dataframes so I could reference how the data should look if I needed to do some feature engineering or correcting some nulls. 

First was data cleaning, the notebooks that start with 01. The notebook 01.1_Training_Data_Cleaning was where I cleaned the provided training dataset. I began by separating out the columns with null values and then focusing on the numeric columns. Once I confirmed there were no nulls, or had corrected the nulls, I transfered the features as a dataframe to 02.1_Numeric_EDA_and_Feat_Engineering. I kept the numeric features separate so that I could quickly edit models using only numbers and no dummy columns. I then converted the object data type columns that were rankings to numeric hierarchies and transferred them to the 02.1 notebook. What I noticed about the numeric columns is that almost all of the nulls were supposed to be zeroes, or were non-existant and thus could be represented as zeroes. This included the converted columns. The object columns where checked to make sure nothing odd was in them and then transferred to 02.2_Object_EDA_and_Feat_Engineering. There was not a lot of cleaning for those columns as I chose to do the dummies in notebook 02.2 for ease of reading and review. Notebook 01.2_Fixing_Test_Dataframe is where the test dataset was modified to match what the training dataframe and features dataframe contained. It also filled in any nulls or converted any features as needed. Notebook 01.2 would continue to be updated until the end.

The second step was EDA, visualization, and creating and testing models. This took place in the 02 notebooks. For the numeric data in notebook 02.1, I noticed that the strongest features were often the ones that spoke to the quality of the referenced feature, which makes sense. I found it interesting that the sales around the housing bubble in the late 2000s did not seem to have any effect on the price of the homes. It was through EDA that I found that there were some outliers that I could not reconcile the sale price with the features provided and they had to be dropped. Luckily there were only two. I ran the LASSO and Ridge models here, but they could not do any better than my existing models so I did not continue to use them. They did however point out some interesting features, such as masonry veneer and garage variables being important. I noticed a lot of overlap too that I tried to address by removing some features in hopes of preventing overfitting. You can see more notes in that notebook as I was working on the features.

For notebook 02.2_Object_EDA_and_Feat_Engineering, I focused solely on what I should dummy. I narrowed it down based on the correlation ranges and dispersement of rows amongst the features. The heatmaps and scatterplots in the notebook helped visualize these features. I also took into consideration whether or not the dummies may interact with the numeric features. This led me to include columns like masonry veneer type. I tried to run a regression model with just dummies to see what happens, and unsurprisingly it did not do well. You can look at notebook 02.2 for more details.

Notebook 02.3_Combined_EDA_and_Feat_Engineering brought the numeric and object datatypes together in one spot to be modified. As it turned out, the first version of the model in that notebook turned out to be my best model. It seems like the numeric data drove the majority of the price, but the dummies helped to bring down the variance. You can see in the notebook that my residuals began to become homoskedastic and that the model's predictions vs actual sales price was very close to being linear. I was trying to address more outliers, but unfortunately I just ran out of time to do it properly. 

Notebook 03_Model_Prep_and_Submissions was the third step in my process. Any time I wanted to make a submission to Kaggle, I would take my model from the 02 notebooks and transfer it here. I would then use the test dataset that was updated accordingly to create predictions for the sale price. Once created, I saved the properly formatted dataframes to the submissions directory and then submitted them to Kaggle. The model numbers are the same as the ones in the 02 notebooks to help keep track of everything. 

Lastly, the 04_Final_Submission notebook contains the final versions of the test dataset, the training dataset, and the features the created the best model. The regression model was then recreated and evaluated one last time. Please see the notebook for more details. It also contains some conclusions and recommendations as well.

## Conclusions and Recommendations

There are a couple big conclusions that I made while working on the different models. First is that having special features or even a pool does not have as big an impact on home prices as I thought. It could be because there were so few houses with these things, but without a larger dataset we may not know. Second is many important factors can't really be changed without significant financial input. Square footage, number of rooms, where the house is actually located, the type of roof it has, etc. These things are some of the more important factors in price, but they can't be easily fixed. Unsurprisingly, the best way to improve sale price is to improve the overall quality and condition of the rooms and features of the home. The highest correlated feature was Overall Quality after all. 

In regards to my problem statement, here are 5 things that I recommend focusing on if you wanted to increase your potential sale price on your home. Before I start though, I do not recommend building onto the house just to sell it. It's waste of your money and unless it's a full bathroom, it is not as valuable as one might think. These recommmendations are taken straight from notebook 04. There are visualizations to go along with each recommendation in there. 

Recommendation 1: Garage Updates

First off, having a garage attached to your home is a huge plus. However, you could be leaving money on the table if you don't have it up to date and in good condition. Looking at the heatmap below, you can see that having and unfinished garage can be more detremental to your selling price than not having a garage at all! If you improve the quality to even just a 3 out of 5, an average rating, the possibilities open up. 


Recommendation 2: Update the Kitchen

There's a reason kitchen updates are so common. Having a good quality kitchen will do you good when it comes to selling your house. Not only that, you can benefit from it to while you're living there. It's a win-win! You'll note that as the quality goes up, the lowest price also goes up. It's also hard to get below a 2 on quality, hence there are no 1s in this plot.


Recommendation 3: Add Some Masonry Veneer to Your Home

Did you know that not having any type of masonry veneer on your house is hurting your sale price? I sure didn't until I did this project. As you can see from the heatmap above, not having any has a rather strong negative correlation on sale price. You may want to do something more than common brick, but having even that is better than none.


Recommendation 4: Upgrade Your Heating System

It's not something most people think about until they have to. If you your heating system is outdated and doesn't work as well as it used to, upgrading it can increase your house's value. Not only that, it can make the winter months more bearable for you. It may also save you money on heating if you buy an energy efficient model. Food for thought.


Recommendation 5: If You Have a Basement, Up the Quality of It.

If you have a basement, it would do you good to make sure that it is updated with quality improvements and that the overall condition is good. The chart below multiplies condition and quality to show that having a higher number in both improves your range. Interestingly, if you have a basement but it is not scoring high in quality or condition rankings, your house may be taking a bigger hit than a home without a basement. If your basement isn't that tall, you can make up for it by making sure the condition is good, i.e. not damp or cracking.

## Best Kaggle Score

My best Kaggle submission score, prior to the remaining 30% of data being added in, was 23,282.12783


## Next Steps

The biggest thing here is that this project would benefit from more time to analyze the outliers to narrow down the variance and get a more accurate model. I don't think this project would benefit from more data as the bias seemed to be low most of the time and more data would probably cause more variance, which is not needed.

It would be interesting to run some other models other than regression to see if I could do any better. Additionally, a better grasp on how dummies work in regression models would help me narrow down exactly what is pertinent and what can be thrown out. However the biggest fix would still be more time to address more outliers, try LASSO and Ridge more, and just get a better feel for the modeling process to make it cleaner and smoother.