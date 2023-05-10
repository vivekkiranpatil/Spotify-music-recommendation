# Spotify Recommendation System and Streamlit Web App

## Introduction and Project Goals

Spotify creators and artists rely on user interaction to increase traffic on their profiles. In adddition to song plays, artists often make playlists of personally-recommended songs to engage with their audience. For users, Spotify may recommend its own playlists, similar artists or songs based on other user metrics or artist/song similarities. This project seeks to increase direct user interaction with artists in Spotify by building a content-based recommendation system of songs in a artist's playlist. The user will input any songs of their choosing or manually adjust Spotify audio features, and the recommender will return a list of similar songs from the playlist based on the numerical data of audio features as defined by Spotify. By suggesting playlist songs with the most similarities to a user-suggested track we aim to increase user engagement and traffic to the artist's playlist and Spotify profile.

## Data

We will be using a 1700+ row dataset collected from the Spotify API and consisting of songs from a playlist from DJ and producer Four Tet, which can be found in the above CSV file. The dataset consists of descriptive metadata (track name, artist name) as well as numerical audio features which we will use for modeling (acousticness, danceability, duration, energy, instrumentalness, liveness, loudness, tempo and valence).

We will test our models on two additional smaller playlists to evaluate its feasability with different-sized datasets - Shower Songs (200 rows) and Max Richter's Kitchen Playlist (78 rows).

## Obtaining Playlist Data

We first access the Spotify API in order to extract playlist metadata. [Spotipy](https://spotipy.readthedocs.io/en/2.19.0/) is a Python library which allows developers access to the Spotify Web API upon input of their Client ID and Client Secret (these can be obtained through [Spotify For Developers](https://developer.spotify.com/)). We convert our data to a Pandas DataFrame and save as a .csv file in the repository; this process can be emulated with any Spotify playlist using the creator's username and playlist URI (obtainable in Spotify). We now have a dataset ready for preprocessing and modeling.

## Clustering and Modeling

We select a subset of our data containing the numerical audio features mentioned above. To accurately discern if a content-based recommendation system is viable for this dataset, we can cluster and visualize our data points in a two-dimensional space using KMeans to determine the optimal number of clusters. We scale our data and use dimensionality reduction to visualize the data points in a two-dimensional space. The below graphs show each song represented as a data point in three-cluster and two-cluster scatter plots.

<img src="/images/threeclusters.png" width="400" height="300"/> <img src="/images/twoclusters.png" width="400" height="300"/>

For this particular dataset, the three-cluster plot is optimal for our content-based recommendation system; however we can use the two-cluster plot to create a classifier to predict which song ends up in which cluster and the feature importances of this classification. Knowing this will help us determine the effects of this particular playlist on our recommendation system and how it might function with other playlists of different character and type.

<img src="/images/feature_importances.png" width="400" height="300"/> <img src="/images/confusion_matrix.png" width="400" height="300"/>
<img src="/images/lime_visual.png" width="800" height="300"/>

A Logistic Regression classification model yields 98.2% accuracy on an untrained set of data. Visualizing our feature importances shows acousticness and energy as most important, and we can use a LIME visual on any track in the playlist to determine the importance of each feature on its classification. 

## Building a Recommendation System

Based on the visualized data points of the above three-cluster model, we implement a content-based recommendation system for this playlist. Our system allows a user to input one or multiple songs of their choosing in Spotify; they will then be recommended similar songs from the artist's playlist. The user can select any songs as long they exist in Spotify's database for their recommendations.

Separating out audio features in our dataset, we calculate the mean vector of a song list as our "song center" and use cosine distance to find closest matches to return as a DataFrame. The end result is a user-input song or song list returning closest-distance song suggestions for the user based on Spotify's numerical audio features.

## Challenges we faced

We experimented with addtitional datascraping tools from open source websitess like SoundCloud, Deezer, Pandora and Apple Music. The problem with these platforms included very limited api calls to scrape data and anti-automation and bot-bursting servies that didn't allow us to scrap data effectively. 

The result was we had to settle with using Spotipy, which allowed us to scrape upto 500 api calls per month, which we scraped using multiple accounts. The undersampling problem was mostly solved by SMOT.

Moreover, we experimented with other Tech Stacks like Node Stack, and Python-Flask. All of them required extensive work in frontend developement to inculcate into a deployable format to analyze data.
Choosing Streamlit was the optimal outcome due to very little need for frontend development as they support markdown developement.

## Future prospects

There are several directions we could take this project in the future to improve or expand upon the current implementation:

1. Incorporating User Feedback: One way to further personalize the recommendations could be to incorporate user feedback. For example, we could ask users to rate how much they like the recommended songs, and use this feedback to adjust the recommendations for future users. This would require building a feedback mechanism into the web app and incorporating it into the recommendation algorithm.
2. Exploring Alternative Recommendation Algorithms: While the current content-based recommendation system based on cosine distance is effective, we could explore other algorithms such as collaborative filtering or matrix factorization to see if they could improve the recommendations. This would require collecting additional data, such as user listening histories or ratings, and implementing a different algorithm in the recommendation system.
3. Expanding to Additional Playlists: While our initial implementation used a playlist from Four Tet, we could expand the project to include additional playlists from a variety of artists and genres. This would allow us to test the effectiveness of the recommendation system on a wider range of music, and potentially identify any limitations of the current algorithm.
4. Incorporating External Data: We could also explore incorporating external data sources, such as artist or album information, to further personalize the recommendations. For example, we could recommend songs from the same album or by the same artist as the user-input track, or incorporate genre or mood information to generate more targeted recommendations.
