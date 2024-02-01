
Hackathons are great for implementing ideas that you might not otherwise implement thinking they're too silly to be of any real value.

My team decided to build a clothing recommendation system for [AI ATL](https://www.aiatl.io/). It would take into account clothes you already have and make recommendations based on that - something we had never seen before (well maybe you can find your clothes in an online store and then just browse whatever they recommend but forget about that), especially recommendations across stores. Also, well, pairing good outfits every day is a daily struggle for CS majors. 
### Clothes -> Vectors

On a high level, our idea was to use GPT Vision to create text descriptions of clothing items and then convert those into embeddings to compare two items. Without GPT Vision, this was probably not possible to do at our skill level since we'd have to create an entirely new model  which would be able to create vector embeddings from images of clothes (not even accounting for background noise and irrelevant items in the pictures). With GPT Vision, we were able to produce extremely strong results with a simple prompt. 

```
"Describe every (or the only) clothing item in these image in detail, be sure to pay attention to little things in the clothing items. Do not talk about the relative positioning of the items, meaning talk about each of the items out of context of the image - as if they were a seperate image. ONLY return the descriptions of the items seperate by a new line."
```

This prompt was enough to get us very detailed and accurate descriptions of clothes, even if they were captured with a normal phone camera while someone was wearing them with tons of other noise in the picture as well. 

```
This is a flannel shirt featuring a bold red and black checkered pattern. It has a classic collar, a button-down front with visible buttons, and long sleeves with buttoned cuffs. There appears to be a chest pocket on the left side, though it's not clearly visible. The material looks soft and slightly textured, typical of flannel shirts. The hem is straight and appears to be designed to be worn untucked. The inside label is visible at the back of the neck.
```

GPT Vision's performance was honestly very impressive and slightly scary. 
### Item Inventory

Now that we had a way to convert clothes to vectors, we needed a way to recommend clothes to users. We decided to scrape popular clothing stores and then run them through our GPT Vision query to then store in a vector DB along with the link to the item. 

This was fairly easy to do, we started scraping things off a website and successfully scraped an entire category. 

EXCEPT -- there was a big problem. GPT Vision has its usage limited to a set number of tokens. For our purposes, we could only make 100 API requests in a day with it. 

We tried some other models, but none were as promising as GPT Vision. We decided to continue anyway and used multiple accounts to build an inventory of Men's Tops. Yep, you can only get sensible recommendations from our app if you scan men's bottoms.

### Recommendation Algorithm 

Our recommendation algorithm was built around a simple observation - online clothing stores love to market their other items under an items page to show what could go well with it. While scraping the clothes off of an online store, we also scraped these recommendations and also vectorized these. This meant we had an extremely simple and human way to know what clothes go well with what other clothes. We took this a step further to enable cross store recommendations and go beyond the 3 sets of recommendations from the store. 

1. User inputs a clothing item -> vectorize this and try to find a similar item in our inventory
2. Look at all the clothes the store recommends with the item found which is similar to the user's item
3. For each item in the recommendations, run a vector search on the inventory to find other similar items from different stores
4. Fetch the top 5 matches and send links to the frontend

This ended up working fairly for men's bottoms (this will never stop being funny) but we could never test it's potential with a full inventory. Maybe one day I will have the motivation to fine tune an open source vision model and use that to vectorize a couple of online store but for now let's just assume this algorithm is very scientific and amazing. 

### Spaghetti 

We ended up winning the hackathon!?!? I'm just kidding - we won the best domain award out of all of them, but at least my devpost has the winner tag. 

Implementing all of that in 36 hours was more tedious than it needed to be, but we got through it. Interestingly enough, the next day, Meta made a post talking about their outfit recommendation feature on the Meta Rayban glasses. Classic Zuckerberg.

[Code Repo](https://github.com/yatharth-b/styles.compare) (don't judge)


