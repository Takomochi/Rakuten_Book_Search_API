# Rakuten Book Search API

## Overview of the project
I love reading Japanese historical novel. One of my favorite authors is Ryotaro Shiba (司馬 遼太郎). I want to know more about his books by collecting and analyzing data. So, I have collected data from [Rakuten Book Search API](https://webservice.rakuten.co.jp/) and explored the data. 

## Resources
- Data: Rakuten API
- Software: Python 3.7.10, Jupyter Notebook

## Results
### 1. Collect data from Rakuten API
On the Rakuten web service page, the documentation explains how to retrieve data. The notebook for the data retrieval is located [here](https://github.com/Takomochi/Rakuten_Book_Search_API/blob/main/rakuten_books_api.ipynb).

When I saw the results, there were 10 pages of information. Therefore, I made a function that will loop 10 times and extract information from each page.

```
# Function to extract all the information about Ryotaro Shiba's Books
def extract_data():
    ryotaro_books_df = pd.DataFrame()
    for i in range(1,10):
        res = requests.get(f"https://app.rakuten.co.jp/services/api/BooksBook/Search/20170404?format=json&author=%E5%8F%B8%E9%A6%AC&booksGenreId=001004008&page={i}&applicationId={app_id}")
        results = res.json()
        
        for j in range(len(results['Items'])):
            books_info = results['Items'][j]['Item']
            _df = pd.DataFrame(books_info, index=[j])
            ryotaro_books_df = ryotaro_books_df.append(_df)
    return ryotaro_books_df
```
The exported CSV file is located [here](https://github.com/Takomochi/Rakuten_Book_Search_API/blob/main/Resources/ryotaro_books.csv).

### 2. Analysis and Visualization

1. **Publisher names and counts <br>**

    Top 3 publishers
    - Bungeisyunsyu
    - Asahi newspaper
    - Shincho-sha

<img src="https://user-images.githubusercontent.com/85041697/147604909-334fd966-800f-4f3a-8a77-4c7b1307de9a.png">   

2. **Book Size Ratio<br>**
Most of Ryotaro Shiba's Books are paperback books followed by collected works.

<img src="https://user-images.githubusercontent.com/85041697/147605754-42dc6262-01c8-4162-9d9a-122ae76d974d.png">

3. **Book Prices<br>**
 we can see that the majority of his books are between 700 yen and 1700 yen. Although there are still outliers on the chart, those are sets of multiple books

<img src="https://user-images.githubusercontent.com/85041697/147605968-46ce8266-db01-4bab-87ec-b7a741c51c50.png">

4. **Distribution of review average<br>**
Most of the reviews are between 4.0 and 4.25 which is pretty high.

<img src="https://user-images.githubusercontent.com/85041697/147606099-a80fe160-18a3-4bda-9955-2b35d36a5d8f.png">

5. **Ryotaro Shiba's Books ranking <br>**
It is very clear that "竜馬がゆく", "燃えよ剣", and "国盗り物語" are popular series.

    <img src="https://user-images.githubusercontent.com/85041697/147606141-8da39e54-4ca6-48ad-957d-59d61e3b7eab.png" width="226">


6. **Correlation<br>**
By creating regression plots, compare relationships between reviewAverage, reviewCount, and itemPrice.

    <img src="https://user-images.githubusercontent.com/85041697/147606237-d6c57cfc-bd2c-4d0b-b1f3-394a3ead0c97.png">

    <img src="https://user-images.githubusercontent.com/85041697/147606241-03dabec5-8bfb-468f-beb1-7b2ad58fd6d7.png">

    The most correlation we have is between review average and review count. Review average and item price has less correlation as we can see in the heatmap too.

7. **Word Cloud (ItemCaption)<br>**
I can see that words in the word cloud are mostly related to Sengoku era(戦国時代) and Bakumatsu era(幕末). This makes sense because of the ranking above. "Ryoma Goes His Way"(竜馬がゆく) is Bakumatsu era, "Moeyo sword"(燃えよ剣) is also Bakumatsu era, and "The Story of National Theft"(国盗り物語) is Sengoku era.

    <img src="https://user-images.githubusercontent.com/85041697/147606560-ecfe43f2-ad18-4ead-9829-c38f0eff2584.png" width=600>
