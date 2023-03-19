# FIFA_21_Data_Cleaning_Task

![download](https://user-images.githubusercontent.com/105908253/226148262-bcbe00cd-63b1-4757-85da-d986ba03d2d0.jpg)

# Introduction:
A data cleaning task was introduced using the hashtag #datacleaningchallenge to allow beginners, intermediate and pro data analyst in the tech space to exercise their skills using any tool of their choice.
Power Query on Power BI was used as my preferred tool for this task

# Dataset Description:
 
 ![image](https://user-images.githubusercontent.com/105908253/226147063-06738ed6-5562-40fc-89c4-e4fcfffdb1d3.png)

The dataset used for this challenge is the FIFA-U21 data. The dataset was sourced from Kaggle.com. The dataset was obtained in its raw state after web scrapping from sofifa.com. It contains the details of football players alongside the performance.
The dataset contains 18979 rows and 77 columns. The datafile also include a data dictionary to further give the data analyst more insight on the player information.

# Data Cleaning Process:

 ![image](https://user-images.githubusercontent.com/105908253/226148154-288dc068-a6d7-42fd-a74c-a509447b4ac9.png)

The dataset was imported into the power query editor and loaded for transformation.
Walk with me as I take you through the step-by-step process of cleaning the dataset from its messy state to a clean state ready to use for analysis.

# ID, Name & Long Name: 
The ID column contains the unique number assigned to every player. The data type was converted to numbers. The ‘Name’ column contains the players short name while the ‘Long Name’ column contains the players full name. The column name was changed to ‘Full Name’.
							
![image](https://user-images.githubusercontent.com/105908253/226147106-8836a3ae-532c-4d8a-aa28-920a23e8148c.png)

![image](https://user-images.githubusercontent.com/105908253/226147115-018e15ea-5209-428c-9dc7-e92477cab41a.png)

# PhotoURL, PlayerURL & Nationality:
The ‘PhotoUrl’ and ‘PlayerUrl’ columns contains the players’ photo and full names embedded in its URL, while the ‘Nationality’ column contain the players country of origin. The decision to retain the ‘PhotoUrl & PlayerUrl’ column was based on the purpose of this challenge which was to clean the data. 

![image](https://user-images.githubusercontent.com/105908253/226147150-7df4be7b-f4e5-4a95-bcca-f2c8a6652b84.png)

# Age, OVA & POT:
The ‘Age’ column contains the age of individual players. The column was converted to number data type. The Overall Players’ Analysis (OVA) and Players Potential (POT) column were converted to percentage as written in the data dictionary. A custom column was used to multiply the value by 0.01 (100) to give the answer in percentage.

![image](https://user-images.githubusercontent.com/105908253/226147162-257d89a7-8214-438e-9da1-9190760a8127.png)

![image](https://user-images.githubusercontent.com/105908253/226147168-86082043-172b-4346-98d9-573f8e687967.png)

# Club:
The column consists of inconsistent data entry. Some of the club’s name had numerical prefix attached to their names, like 1.FC Koln. The prefix was removed from the data.

![image](https://user-images.githubusercontent.com/105908253/226147263-0a68b15e-1b59-42e9-a287-82c9df86d797.png)

![image](https://user-images.githubusercontent.com/105908253/226147269-4af18ae9-1d89-4f1d-880c-d4c91fe65c31.png)

# Contract:
Filtering the ‘Contract’ column, I realized it contained inconsistent values. It contains values in 3 format, 2004~2021, free and 23rd July 2020 on loan. Thus, conditional formatting was used to achieve clean data.

![image](https://user-images.githubusercontent.com/105908253/226147283-fa47fc2e-94fd-4c45-a5de-b1f923b0497f.png)

The delimiter was changed from ‘ ~ ’ to ‘ - ‘. A conditional formatting was used to separate the players who have active contract (2004-2021) from players on ‘loan’ and players who are ‘free’. I further created a custom column to calculate the contract duration of each player. The formula below was used.
=If text.contains([contract],”-“), then Number.From(Text.AfterDelimiter([contract],”-“)) – Number.From(Text.BeforeDelimiter([contract],”-“)), else “No Contract”)

![image](https://user-images.githubusercontent.com/105908253/226147303-3220bff9-3a1b-4851-abf9-1646d6d7f790.png)

# Position:
The ‘Position’ column consist one or more positions the player has ever played in. The ‘position’ column was removed as there’s another column ‘Best Position’ which describe the best position for the player. This is best fit for analysis.

![image](https://user-images.githubusercontent.com/105908253/226147384-b139f8ea-75de-4bab-ba8c-1e5a59673a6b.png)

# Joined:
The joined column was filtered to see if there are any inconsistencies in the data. The data has consistent data and the correct data type (date).

![image](https://user-images.githubusercontent.com/105908253/226147391-477a3376-8ca7-4c0e-9111-9b3426973c23.png)

# Loan Date End:
The ‘loan date end’ column has 97% empty rows. This is because the players on loan account for only 3% of the entire dataset.

![image](https://user-images.githubusercontent.com/105908253/226147406-0600d868-edb2-4ef6-88eb-3eec6a6e829d.png)

To solve the problem, I replaced the empty cells with ‘Null’.

![image](https://user-images.githubusercontent.com/105908253/226147409-4e1fba84-e809-4fba-9183-22c4a55ef108.png)

# Height & Weight:
The column contains inconsistent data. The value of height was entered in two different format- 120cm & 5’10” (foot&inches), while the ‘weight’ column has values entered in two format- ‘kg’ and ‘ibs’.

![image](https://user-images.githubusercontent.com/105908253/226147418-84fcf11e-3f59-4720-9b13-5b96a35fdcb1.png)

To achieve consistent data in this column, I converted the foot & inches values to cm. This was done using a formula to multiply the feet by 30.48 and the inches by 2.54 which is the standard conversion.
Let cm = if Text.Contains([Height],"cm") then Number.From( Text.BeforeDelimiter([Height],"cm")) else null, ft = Number.From ( Text.BeforeDelimiter([Height], "'")), inch = Number.From ( Text.BetweenDelimiters( [Height], "'", """")),
Result = if cm is null then (ft*30.48) + (inch*2.54) else cm in Result

While for the weight column, I converted all the values in kg to lbs using the standard conversion rate of 2.20
= If text.contains([weight],”kg“), then Number.From(Text.BeforeDelimiter([weight],”kg“)) *2-20 else Text.BeforeDelimiter([weight],”lbs“))

![image](https://user-images.githubusercontent.com/105908253/226147440-b2abd770-98f9-4bac-ae96-02757d398b51.png)
 
# Value, Wage & Release Clause:
The above columns had data type issues and the values with suffixes ‘M & k’ in front. Where M is for millions, ‘K’ for thousands and ‘€’ for euro.

![image](https://user-images.githubusercontent.com/105908253/226147465-cfd6fd02-1f1b-41da-8fa1-8be6d112822d.png)
 
To normalize the values, the ‘€’, ‘K’ & ‘M’ was removed. A custom column was created to to multiply the values with ‘K’ by 1000 and values with ‘M’ by 1000000 using the formula below.
=IF Text.Contains([value],”M”) then Number.From(Text.beforeDelimiter([value],”M”))*1000000 else Number.From(Text.BeforeDelimiter([value],”K”))*1000)
The data type was converted to whole number and the column remained.

![image](https://user-images.githubusercontent.com/105908253/226147481-79ab7f06-69ac-407f-9d9c-edc891d2edc7.png)
 
•The following columns ‘Attacking’, ‘Crossing’, ‘Finishing’, ‘Heading Accuracy’, ‘Short Passing’, ‘Volley’, ‘Skill’, ‘Dribbling’, ‘Curve’, ‘FK_Accuracy’, ‘Long_Passing’, ‘Ball_Control’, ‘Movement’, ‘Acceleration’, ‘Sprint_Speed’, ‘Agility’, ‘Reaction’, ‘Balance’, ‘Power’, ‘Short_Power’, ‘Jumping’, ‘Stamina’, ‘Strength’, ‘Long_Shot’, ‘Mentality’, ‘Aggression’, ‘Interceptions’, ‘Positioning’, ‘Vision’, ‘Penalties’, ‘Composure’, ‘Defending’, Marking’, ‘Standing_Tackle’, ‘Sliding_Tackle’, ‘GoalKeeping’, ‘GK_Diving’, ‘GK_Handling’, ‘GK_Kicking’, ‘Gk_Positioning’, ‘GK_Reflexex’, ‘Total_Stats’, ‘Base_Stats’, ‘Preferred_Foot’, ‘BOV’, ‘Attacking_Workrate’, ‘Defending_Workrate’, ‘PAC’, ‘SHO’, ‘DRI’, ‘DEF’, ‘PHY’, were all crosschecked and changes were made for the wrong data type. Also, some column names were corrected.

![image](https://user-images.githubusercontent.com/105908253/226147493-4a144d43-7eb0-47d0-b2ba-309cc0e66c2f.png)

# W/F, SM & IR:
The following columns contains rating on a scale of 1-5 with a special character ‘★’ in front of each value. The columns had a wrong data type.

![image](https://user-images.githubusercontent.com/105908253/226147515-e5024c81-4236-4bfe-8298-1451c8a25331.png)

The special character was replaced with space and the column names was written in full with the data type changed to whole number.

![image](https://user-images.githubusercontent.com/105908253/226147519-db9d0fc5-687e-41ec-9575-cb4582011ffe.png)

# Hits:
The column had a text data type and contained numbers in tens, hundreds, and thousands but the thousands were written in short form where 1600 is written as 1.6k.

![image](https://user-images.githubusercontent.com/105908253/226147529-152cb3cc-43a5-46ed-a63d-911051459427.png)

A custom column was created to multiply the values with ‘K’ by 1000 using the formula =IF Text.Contains(Hits),”K”) then Number.From(Text.BeforeDelimiter([Hits],”K”))*1000 else Text.BeforeDelimiter([Hits],”K”))

![image](https://user-images.githubusercontent.com/105908253/226147532-2a946f6d-4299-432c-9c55-3014672dac72.png)

# Conclusion:
After a thorough checking and cross-checking for errors, missing values, empty cells, outliers, and wrong data type. I can say the messy dataset is now clean and consistent which can now be used for analysis.
I’m glad I participated in this challenge to horn and sharpen skill. Quite a lot of learning I did while taking up this task. What initially looked difficult and impossible has been done. Good to say I’m proud of myself.


Thank you for reading through. I hope you find this worth reading and helpful. If you need my support on working on this dataset, you can reach me on twitter [@pinnacle_paul](https://twitter.com/pinnacle_paul) and LinkedIn [@PaulAderounmu](https://www.linkedin.com/in/pauladerounmu)
