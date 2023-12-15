Benjamin Harris (bsh72), Michelle Hui (msh334), Tina Li (tl676), Vicki Yang (vzy3)
#### Helps cocktail enthusiasts explore new cocktail recipes where the user can input liked and disliked ingredients as well as must have and must not have ingredients 
#### Our app will return the top 10 matching recommendations

## Contents

- [Summary](#summary)
- [Examples](#examples)
- [Methods](#methods)

## Summary
This was our final project in CS4300 that recommends cocktail recipes using information retrieval algorithms. Our app fills a market need because it it incorporates flavor profiles and a popularity measure into the drink recommendation system. For other existing cocktail recommendation systems, they either don’t take flavor profile into consideration or only allow for a singular flavor filter. Our app also allows users to choose ingredients they want to exclude from any possible recommendations (in the case of allergies or other dietary restrictions). Since we’ve also implemented Rocchio to allow for users to give feedback on the recommendations and have the app make live adjustments, this is also different from existing websites that may provide cocktail recommendations. 

## Examples
#### Example: lemon is a liked ingredient

![image7](https://github.com/michelleh1109/cocktail-recommender/assets/90575654/e806d849-9b68-4750-bdeb-9e7acfb5cce1)

In this example, our results have changed (improved) due to the use of a popularity measurement when recommending drinks. Drink ranks now include a measurement of how much a drink is liked by other people which factors into how highly it is rated by our system. Gin rickey is a far more popular drink than a lemon drop so it is further up in the recommendations for our final app. We use popularity (from rankings given by people) to recommend drinks since it’s more likely that someone will like a drink that is liked by many other people 

#### Example: lemon as a disliked ingredient

![image2](https://github.com/michelleh1109/cocktail-recommender/assets/90575654/d21d9320-4ff3-40cb-ac71-94b6a4de898c)

This is a clear improvement on our previous system which seemingly didn’t handle dislikes well and just returned the remaining drinks without a disliked ingredient in alphabetical order. Our new system takes into account a cosine ranking of the drinks using the disliked ingredient as a negative weighting when ranking drinks and a popularity measure to recommend drinks that a user might like more due to a generally favorable rating of a drink.
#### Example: vodka must be included as an ingredient and the flavor should be fruity but must not be citrusy

![image5](https://github.com/michelleh1109/cocktail-recommender/assets/90575654/6f0ba564-5574-4df1-bc99-27a49c28ba7f)

Since our flavor filter now filters for liked flavors, we can’t exclude flavors, but this is still an improvement on our old system since popularity measures are given a higher weighting and more equally applied to drink rankings to give users drinks that they have the highest probability of liking. 

## Methods
 - Tokenization - We tokenize the inputs from users into token words which are then turned into a query vector.  
 - TF-IDF - We use a built-in TfidfVectorizer from SKLearn to create document frequency vectors for the inputs for likes/dislikes/includes/excludes
 - Cosine Similarity - Our system uses cosine similarity to compute rankings for the drinks in our dataset given a tokenized query. 
 - Boolean search - Our system uses boolean search (boolean and and boolean not) to include or exclude certain inputs from any results by limiting the possible indices of the dataset that can be returned by the system. Any recommendations must then adhere by the user’s inputs for the must include/exclude search boxes. 
 - Rocchio - We used Rocchio’s algorithm as our text mining component to allow the user to provide feedback on results. We use the user feedback (if they like or dislike a recommended drink/drinks) in subsequent queries when calculating the cosine similarity for a new query vector. 

#### Tokenization of ingredients
Another improvement we have made to the information retrieval aspect is improving tokenization. Part of this was done through modifying our dataset by removing measurements from the ingredients list for a drink which would’ve been treated as separate tokens. While doing this, we made sure not to remove numbers or phrases that are essential parts of an ingredient. In addition, we now remove any measurement phrases such as “oz”, “cup”, and “ml” too while not modifying any actual ingredient tokens as well. 

#### Social Component
We incorporated a popularity ranking to sort the cocktail recommendations. We did this by finding a new drink popularity dataset that contains more drinks and for drinks not contained in the original flavors and ingredients cocktail dataset, their popularity measure is calculated through using cosine similarity and using the measurement of the most similar drink in the dataset. Our query then adds fractional values of these measurements to the cosine similarity metric that results in the outputs of our system. We’ve chosen to display this measurement in our recommendation system output with the label “Popularity Score”. The most popular drink out of our recommendations is indicated by a star in the bottom right corner of the corresponding cell in the list of recommendations. 

#### Text Mining
To implement the text mining component of our app, we chose to use Rocchio’s algorithm to iterate on our query. When displaying our outputs, we have now included thumbs up and thumbs down buttons with each output. The user can then click on these buttons to indicate if they like or dislike the recommended drink and all future recommendations (new searches) will include these likes/dislikes in the new (updated) query. This holds for any additional query until the user refreshes the page. All new queries will include like/dislike measurements from previous recommendations when calculating the new results. 

#### Known issues
One of our known issues is that we currently use a tokenizer that splits on words rather than phrases which will cause an ingredient like “lemon juice” to become “lemon” and “juice”. We chose to do this purposefully because some ingredients contain multiple words and/or numbers which may cause issues if tokenized as a single phrase because something like “no.3 london dry gin” is a type of gin, but it would not match the input “gin” so we determined that it was better to split on words so that matching would better fit the intent instead of being exact. Unfortunately, this means that our boolean search malfunctions (as intended) since the tokenization forcibly separates some multi-word ingredients into separate tokens. Another known issue is that our flavor filters currently only allow for the user to choose flavors that they like and do not allow the user to exclude flavors they don’t like. We’d like to know if this is okay to ensure that there is less confusion when using the filter buttons or if it would be better to include flavor selection with likes and dislikes. Lastly, another potential issue is that our popularity score isn’t very interpretable by users at the moment. There can be a wide range within the popularity scores and they aren’t restricted to any scale so users don’t have a way to interpret the score besides knowing that a larger number means a higher score. Another slight issue is that we no longer display ingredient measurements due to how our tokenization works now and we do not display drink preparation notes at all. This may be an inconvenience to users who are looking for drink recommendations and recipes so we would like to know if we should include this or if it would be too crowded for a recommendation system to have. 


