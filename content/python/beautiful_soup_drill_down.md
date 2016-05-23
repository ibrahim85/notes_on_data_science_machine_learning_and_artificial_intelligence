Title: Drilling Down With Beautiful Soup
Slug: beautiful_soup_drill_down
Summary: Drilling Down With Beautiful Soup
Date: 2016-05-01 12:00
Category: Python
Tags: Web Scraping
Authors: Chris Albon




```python
# Import required modules
import requests
from bs4 import BeautifulSoup
import pandas as pd
```

### Download the HTML and create a Beautiful Soup object


```python
# Create a variable with the URL to this tutorial
url = 'http://en.wikipedia.org/wiki/List_of_A_Song_of_Ice_and_Fire_characters'

# Scrape the HTML at the url
r = requests.get(url)

# Turn the HTML into a Beautiful Soup object
soup = BeautifulSoup(r.text)
```

If we looked at the soup object, we'd see that the names we want are in a heirarchical list. In psuedo-code, it looks like:

- class=toclevel-1 span=toctext
    - class=toclevel-2 span=toctext CHARACTER NAMES
    - class=toclevel-2 span=toctext CHARACTER NAMES
    - class=toclevel-2 span=toctext CHARACTER NAMES
    - class=toclevel-2 span=toctext CHARACTER NAMES
    - class=toclevel-2 span=toctext CHARACTER NAMES

To get the CHARACTER NAMES, we are going to need to drill down to grap into loclevel-2 and grab the toctext

### Setting up where to put the results


```python
# Create a variable to score the scraped data in
character_name = []
```

### Drilling down with a forloop


```python
# for each item in all the toclevel-2 li items
# (except the last three because they are not character names),
for item in soup.find_all('li',{'class':'toclevel-2'})[:-3]:
    # find each span with class=toctext,
    for post in item.find_all('span',{'class':'toctext'}):
        # add the stripped string of each to character_name, one by one
        character_name.append(post.string.strip())
```

### The results


```python
# View all the character names
character_name
```




    ['Eddard Stark',
     'Catelyn Stark',
     'Robb Stark',
     'Sansa Stark',
     'Arya Stark',
     'Bran Stark',
     'Rickon Stark',
     'Jon Snow',
     'Lyanna Stark',
     'Roose Bolton',
     'Ramsay Snow',
     'Hodor',
     'Osha',
     'Jeyne Poole',
     'Jojen and Meera Reed',
     'Jeyne Westerling',
     'Daenerys Targaryen',
     'Viserys Targaryen',
     'Rhaegar Targaryen',
     'Aegon V Targaryen',
     'Aerys II Targaryen',
     'Aegon Targaryen',
     'Jon Connington',
     'Jorah Mormont',
     'Brynden Rivers',
     'Jon Arryn',
     'Lysa Arryn',
     'Robert Arryn',
     'Tywin Lannister',
     'Cersei Lannister',
     'Jaime Lannister',
     'Joffrey Baratheon',
     'Tyrion Lannister',
     'Kevan Lannister',
     'Lancel Lannister',
     'Bronn',
     'Gregor Clegane',
     'Sandor Clegane',
     'Podrick Payne',
     'Robert Baratheon',
     'Myrcella Baratheon',
     'Tommen Baratheon',
     'Stannis Baratheon',
     'Melisandre',
     'Davos Seaworth',
     'Renly Baratheon',
     'Brienne of Tarth',
     'Beric Dondarrion',
     'Gendry',
     'Balon Greyjoy',
     'Asha Greyjoy',
     'Theon Greyjoy',
     'Euron Greyjoy',
     'Victarion Greyjoy',
     'Aeron Greyjoy',
     'Doran Martell',
     'Arianne Martell',
     'Quentyn Martell',
     'Elia Martell',
     'Oberyn Martell',
     'The Sand Snakes',
     'Areo Hotah',
     'Hoster Tully',
     'Edmure Tully',
     'Brynden Tully',
     'Walder Frey',
     'Mace Tyrell',
     'Willas Tyrell',
     'Garlan Tyrell',
     'Loras Tyrell',
     'Margaery Tyrell',
     'Olenna Tyrell',
     'Jeor Mormont',
     'Maester Aemon',
     'Yoren',
     'Samwell Tarly',
     'Janos Slynt',
     'Mance Rayder',
     'Ygritte',
     'Val',
     'Petyr Baelish',
     'Varys',
     'Pycelle',
     'Barristan Selmy',
     'Arys Oakheart',
     'Ilyn Payne',
     'Qyburn',
     'Balon Swann',
     'Khal Drogo',
     'Syrio Forel',
     "Jaqen H'ghar",
     'Illyrio Mopatis',
     'Thoros of Myr',
     'Ser Duncan the Tall']



## Quick analysis: Which house has the most main characters?


```python
# Create a list object where to store the for loop results
houses = []
```


```python
# For each element in the character_name list,
for name in character_name:
    # split up the names by a blank space and select the last element
    # this works because it is the last name if they are a house,
    # but the first name if they only have one name,
    # Then append each last name to the houses list
    houses.append(name.split(' ')[-1])
```


```python
# Convert houses into a pandas series (so we can use value_counts())
houses = pd.Series(houses)

# Count the number of times each name/house name appears
houses.value_counts()
```




    Stark         8
    Tyrell        6
    Targaryen     6
    Greyjoy       6
    Baratheon     6
    Lannister     6
    Martell       5
    Tully         3
    Arryn         3
    Payne         2
    Clegane       2
    Mormont       2
    Snow          2
    Bolton        1
    Aemon         1
    Val           1
    Frey          1
    Rivers        1
    Varys         1
    Bronn         1
    Hotah         1
    Tarly         1
    Osha          1
    Tall          1
    Yoren         1
    Rayder        1
    Snakes        1
    Myr           1
    Seaworth      1
    Qyburn        1
    Forel         1
    Baelish       1
    Poole         1
    H'ghar        1
    Drogo         1
    Swann         1
    Selmy         1
    Gendry        1
    Tarth         1
    Slynt         1
    Westerling    1
    Hodor         1
    Mopatis       1
    Pycelle       1
    Ygritte       1
    Reed          1
    Dondarrion    1
    Connington    1
    Oakheart      1
    Melisandre    1
    dtype: int64