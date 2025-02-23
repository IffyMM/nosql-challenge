# nosql-challenge
# Introduction

A food hygiene rating is assigned to facilities throughout the United Kingdom based on their evaluation by the UK Food Standards Agency. The editors of Eat Safe, Love, a food magazine, have hired you to analyze some of the ratings data so that their journalists and food critics can determine the topics of future stories.

#Database and JupiterNotebook setup

-Import the data provided in the establishments.json file from your Terminal.
=Name the database uk_food and the collection establishments. Copy the text you used to import your data from your Terminal to a markdown cell in your notebook.
-Within your notebook, import the libraries you need: PyMongo and Pretty Print (pprint).
-Create an instance of the Mongo Client.
-Confirm that you created the database and loaded the data properly:
-List the databases you have in MongoDB. Confirm that (uk_food) is listed.
-List the collection(s) in the database to ensure that (establishments) is there.
-Find and display one document in the establishments collection using find_one and display with pprint.
-Assign the establishments collection to a variable to prepare the collection for use.


# Updated the Database

An exciting new halal restaurant just opened in Greenwich, but hasn't been rated yet. The magazine has asked you to include it in your analysis. Add the following information to the database:
{
    "BusinessName":"Penang Flavours",
    "BusinessType":"Restaurant/Cafe/Canteen",
    "BusinessTypeID":"",
    "AddressLine1":"Penang Flavours",
    "AddressLine2":"146A Plumstead Rd",
    "AddressLine3":"London",
    "AddressLine4":"",
    "PostCode":"SE18 7DY",
    "Phone":"",
    "LocalAuthorityCode":"511",
    "LocalAuthorityName":"Greenwich",
    "LocalAuthorityWebSite":"http://www.royalgreenwich.gov.uk",
    "LocalAuthorityEmailAddress":"health@royalgreenwich.gov.uk",
    "scores":{
        "Hygiene":"",
        "Structural":"",
        "ConfidenceInManagement":""
    },
    "SchemeType":"FHRS",
    "geocode":{
        "longitude":"0.08384000",
        "latitude":"51.49014200"
    },
    "RightToReply":"",
    "Distance":462
    3.9723280747176,
    "NewRatingPending":True
}

-Update the new restaurant with the BusinessTypeID found.

-The magazine is not interested in any establishments in Dover, so check how many documents contain the Dover Local Authority. Then, remove any establishments within the Dover Local Authority from the database, and check the number of documents to ensure they were deleted.

-Updated, Some of the number values are stored as strings, when they should be stored as numbers.
-Use update_many to convert latitude and longitude to decimal numbers.
-Use update_many to convert RatingValue to integer numbers.

# Analysis

note: RatingValue refers to the overall rating decided by the Food Authority and ranges from 1-5. The higher the value, the better the rating.

Questions to be answered:

Question 1: Which establishments have a hygiene score equal to 20?

Used a query: {'scores.Hygiene: 20}
displayed results and stored it in a dataFrame.

analysis: 41 restuarants had a hygiene score of 20.


Question 2: Which establishments in London have a RatingValue greater than or equal to 4?

used the $regex to find establishments that had London in it:
query ={'LocalAuthorityName': {'$regex': 'London'}, "RatingValue": {'$gte': 4}}

Analysis: We found 33 results with rating value greater than or equal to 4

Question 3: What are the top 5 establishments with a RatingValue of 5, sorted by lowest hygiene score, nearest to the new restaurant added, "Penang Flavours"?

used a parameter to find geocodes near Penang Flavours, with a degree search
parameter= establishments.find_one({"BusinessName": "Penang Flavours"}, {"geocode": 1})
degree_search = 0.01, 
sorted and limited it to 5, and stored data in a dataFrame

Question 4: How many establishments in each Local Authority area have a hygiene score of 0? Sort the results from highest to lowest, and print out the top ten local authority areas

Create a pipeline that:

 *Matches establishments with a hygiene score of 0
 *Groups the matches by Local Authority 
 *Sorts the matches from highest to lowest
aggregated the pipeline and stored data in a dataframe, 
Analysis- 
	_id	count
0	Thanet	1130
1	Greenwich	882
2	Maidstone	713, 
had a the highest ones.


# References
UK Food Standards AgencyLinks to an external site. (2022). UK food hygiene rating data API. https://ratings.food.gov.uk/open-data/en-GBLinks to an external site.. Contains public sector information licensed under the Open Government Licence v3.0Links to an external site.
Accessed Sept 9, 2022 and Sept 12, 2022 with the establishment settings as follows: longitude=51.5072, latitude=-0.1276, maxdistancelimit=4567, pagesize=10000, sortoptionkey=distance, pagenumber=(1,2,3,4,5,6,7,8).

edX Boot Camps LLC