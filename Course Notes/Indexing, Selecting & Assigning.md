# Native accessors
Consider this DataFrame:

    reviews

    country	description	designation	points	price	province	region_1	region_2	taster_name	taster_twitter_handle	title	variety	winery
    0	Italy	Aromas include tropical fruit, broom, brimston...	Vulkà Bianco	87	NaN	Sicily & Sardinia	Etna	NaN	Kerin O’Keefe	@kerinokeefe	Nicosia 2013 Vulkà Bianco (Etna)	White Blend	Nicosia
    1	Portugal	This is ripe and fruity, a wine that is smooth...	Avidagos	87	15.0	Douro	NaN	NaN	Roger Voss	@vossroger	Quinta dos Avidagos 2011 Avidagos Red (Douro)	Portuguese Red	Quinta dos Avidagos
    ...	...	...	...	...	...	...	...	...	...	...	...	...	...
    129969	France	A dry style of Pinot Gris, this is crisp with ...	NaN	90	32.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Domaine Marcel Deiss 2012 Pinot Gris (Alsace)	Pinot Gris	Domaine Marcel Deiss
    129970	France	Big, rich and off-dry, this is powered by inte...	Lieu-dit Harth Cuvée Caroline	90	21.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Domaine Schoffit 2012 Lieu-dit Harth Cuvée Car...	Gewürztraminer	Domaine Schoffit
    129971 rows × 13 columns


Hence to access the country property of reviews we can use:

    reviews.country

    0            Italy
    1         Portugal
              ...   
    129969      France
    129970      France
    Name: country, Length: 129971, dtype: object


We can do the same with columns in a DataFrame:

    reviews['country']

    0            Italy
    1         Portugal
              ...   
    129969      France
    129970      France
    Name: country, Length: 129971, dtype: object


Doesn't a pandas Series look kind of like a fancy dictionary? It pretty much is, so it's no surprise that, to drill down to a single specific value, we need only use the indexing
operator [] once more:

    reviews['country'][0]

    'Italy'


# Index-based selection
Pandas indexing works in one of two paradigms. The first is index-based selection: selecting data based on its numerical position in the data. iloc follows this paradigm.
To select the first row of data in a DataFrame, we may use the following:

    reviews.iloc[0]

    country                                                    Italy
    description    Aromas include tropical fruit, broom, brimston...
                                       ...                        
    variety                                              White Blend
    winery                                                   Nicosia
    Name: 0, Length: 13, dtype: object


Both loc and iloc are row-first, column-second. This is the opposite of what we do in native Python, which is column-first, row-second.
This means that it's marginally easier to retrieve rows, and marginally harder to get retrieve columns. To get a column with iloc, we can do the following:

    reviews.iloc[:, 0]

    0            Italy
    1         Portugal
              ...   
    129969      France
    129970      France
    Name: country, Length: 129971, dtype: object


On its own, the : operator, which also comes from native Python, means "everything". When combined with other selectors, however, it can be used to indicate a range of values.
For example, to select the country column from just the first, second, and third row, we would do:

    reviews.iloc[:3, 0]

    0       Italy
    1    Portugal
    2          US
    Name: country, dtype: object


Or, to select just the second and third entries, we would do:

    reviews.iloc[1:3, 0]

    1    Portugal
    2          US
    Name: country, dtype: object


It's also possible to pass a list:

    reviews.iloc[[0, 1, 2], 0]

    0       Italy
    1    Portugal
    2          US
    Name: country, dtype: object


Finally, it's worth knowing that negative numbers can be used in selection. This will start counting forwards from the end of the values. So for example here are the last five elements
of the dataset:

    reviews.iloc[-5:]

    country	description	designation	points	price	province	region_1	region_2	taster_name	taster_twitter_handle	title	variety	winery
    129966	Germany	Notes of honeysuckle and cantaloupe sweeten th...	Brauneberger Juffer-Sonnenuhr Spätlese	90	28.0	Mosel	NaN	NaN	Anna Lee C. Iijima	NaN	Dr. H. Thanisch (Erben Müller-Burggraef) 2013 ...	Riesling	Dr. H. Thanisch (Erben Müller-Burggraef)
    129967	US	Citation is given as much as a decade of bottl...	NaN	90	75.0	Oregon	Oregon	Oregon Other	Paul Gregutt	@paulgwine	Citation 2004 Pinot Noir (Oregon)	Pinot Noir	Citation
    129968	France	Well-drained gravel soil gives this wine its c...	Kritt	90	30.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Domaine Gresser 2013 Kritt Gewurztraminer (Als...	Gewürztraminer	Domaine Gresser
    129969	France	A dry style of Pinot Gris, this is crisp with ...	NaN	90	32.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Domaine Marcel Deiss 2012 Pinot Gris (Alsace)	Pinot Gris	Domaine Marcel Deiss
    129970	France	Big, rich and off-dry, this is powered by inte...	Lieu-dit Harth Cuvée Caroline	90	21.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Domaine Schoffit 2012 Lieu-dit Harth Cuvée Car...	Gewürztraminer	Domaine Schoffit


# Label-based selection
The second paradigm for attribute selection is the one followed by the loc operator: label-based selection. In this paradigm, it's the data index value, not its position, which matters.
For example, to get the first entry in reviews, we would now do the following:

    reviews.loc[0, 'country']
    
    'Italy'


iloc is conceptually simpler than loc because it ignores the dataset's indices. When we use iloc we treat the dataset like a big matrix (a list of lists), one that we have to index into
by position. loc, by contrast, uses the information in the indices to do its work. Since your dataset usually has meaningful indices, it's usually easier to do things using loc instead.
For example, here's one operation that's much easier using loc:

    reviews.loc[:, ['taster_name', 'taster_twitter_handle', 'points']]

            taster_name	  taster_twitter_handle	points
    0	Kerin O’Keefe @kerinokeefe	87
    1	Roger Voss	  @vossroger	87
    ...	...	  ...	...
    129969	Roger Voss	  @vossroger	90
    129970	Roger Voss	  @vossroger	90
    129971 rows × 3 columns


# Choosing between loc and iloc
When choosing or transitioning between loc and iloc, there is one "gotcha" worth keeping in mind, which is that the two methods use slightly different indexing schemes. iloc uses the
Python stdlib indexing scheme, where the first element of the range is included and the last one excluded. So 0:10 will select entries 0,...,9. loc, meanwhile, indexes inclusively.
So 0:10 will select entries 0,...,10.

Why the change? Remember that loc can index any stdlib type: strings, for example. If we have a DataFrame with index values Apples, ..., Potatoes, ..., and we want to select "all the
alphabetical fruit choices between Apples and Potatoes", then it's a lot more convenient to index df.loc['Apples':'Potatoes'] than it is to index something like df.loc['Apples',
'Potatoet'] (t coming after s in the alphabet).

This is particularly confusing when the DataFrame index is a simple numerical list, e.g. 0,...,1000. In this case df.iloc[0:1000] will return 1000 entries, while df.loc[0:1000] return
1001 of them! To get 1000 elements using loc, you will need to go one lower and ask for df.loc[0:999]. Otherwise, the semantics of using loc are the same as those for iloc.

# Manipulating the index
Here is what happens when we set_index to the title field:

    reviews.set_index("title")

    country	description	designation	points	price	province	region_1	region_2	taster_name	taster_twitter_handle	variety	winery
    title												
    Nicosia 2013 Vulkà Bianco (Etna)	Italy	Aromas include tropical fruit, broom, brimston...	Vulkà Bianco	87	NaN	Sicily & Sardinia	Etna	NaN	Kerin O’Keefe	@kerinokeefe	White Blend	Nicosia
    Quinta dos Avidagos 2011 Avidagos Red (Douro)	Portugal	This is ripe and fruity, a wine that is smooth...	Avidagos	87	15.0	Douro	NaN	NaN	Roger Voss	@vossroger	Portuguese Red	Quinta dos Avidagos
    ...	...	...	...	...	...	...	...	...	...	...	...	...
    Domaine Marcel Deiss 2012 Pinot Gris (Alsace)	France	A dry style of Pinot Gris, this is crisp with ...	NaN	90	32.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Pinot Gris	Domaine Marcel Deiss
    Domaine Schoffit 2012 Lieu-dit Harth Cuvée Caroline Gewurztraminer (Alsace)	France	Big, rich and off-dry, this is powered by inte...	Lieu-dit Harth Cuvée Caroline	90	21.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Gewürztraminer	Domaine Schoffit
    129971 rows × 12 columns

This is useful if you can come up with an index for the dataset which is better than the current one.

# Conditional selection
For example, suppose that we're interested specifically in better-than-average wines produced in Italy.
We can start by checking if each wine is Italian or not:

    reviews.country == 'Italy'

    0         True
    1         False
              ...  
    129969    False
    129970    False
    Name: country, Length: 129971, dtype: bool


This operation produced a Series of True/False booleans based on the country of each record. This result can then be used inside of loc to select the relevant data:

    reviews.loc[reviews.country == 'Italy']

    country	description	designation	points	price	province	region_1	region_2	taster_name	taster_twitter_handle	title	variety	winery
    0	Italy	Aromas include tropical fruit, broom, brimston...	Vulkà Bianco	87	NaN	Sicily & Sardinia	Etna	NaN	Kerin O’Keefe	@kerinokeefe	Nicosia 2013 Vulkà Bianco (Etna)	White Blend	Nicosia
    6	Italy	Here's a bright, informal red that opens with ...	Belsito	87	16.0	Sicily & Sardinia	Vittoria	NaN	Kerin O’Keefe	@kerinokeefe	Terre di Giurfo 2013 Belsito Frappato (Vittoria)	Frappato	Terre di Giurfo
    ...	...	...	...	...	...	...	...	...	...	...	...	...	...
    129961	Italy	Intense aromas of wild cherry, baking spice, t...	NaN	90	30.0	Sicily & Sardinia	Sicilia	NaN	Kerin O’Keefe	@kerinokeefe	COS 2013 Frappato (Sicilia)	Frappato	COS
    129962	Italy	Blackberry, cassis, grilled herb and toasted a...	Sàgana Tenuta San Giacomo	90	40.0	Sicily & Sardinia	Sicilia	NaN	Kerin O’Keefe	@kerinokeefe	Cusumano 2012 Sàgana Tenuta San Giacomo Nero d...	Nero d'Avola	Cusumano
    19540 rows × 13 columns


This DataFrame has ~20,000 rows. The original had ~130,000. That means that around 15% of wines originate from Italy. We also wanted to know which ones are better than average.
Wines are reviewed on a 80-to-100 point scale, so this could mean wines that accrued at least 90 points.
We can use the ampersand (&) to bring the two questions together:

    reviews.loc[(reviews.country == 'Italy') & (reviews.points >= 90)]

    country	description	designation	points	price	province	region_1	region_2	taster_name	taster_twitter_handle	title	variety	winery
    120	Italy	Slightly backward, particularly given the vint...	Bricco Rocche Prapó	92	70.0	Piedmont	Barolo	NaN	NaN	NaN	Ceretto 2003 Bricco Rocche Prapó (Barolo)	Nebbiolo	Ceretto
    130	Italy	At the first it was quite muted and subdued, b...	Bricco Rocche Brunate	91	70.0	Piedmont	Barolo	NaN	NaN	NaN	Ceretto 2003 Bricco Rocche Brunate (Barolo)	Nebbiolo	Ceretto
    ...	...	...	...	...	...	...	...	...	...	...	...	...	...
    129961	Italy	Intense aromas of wild cherry, baking spice, t...	NaN	90	30.0	Sicily & Sardinia	Sicilia	NaN	Kerin O’Keefe	@kerinokeefe	COS 2013 Frappato (Sicilia)	Frappato	COS
    129962	Italy	Blackberry, cassis, grilled herb and toasted a...	Sàgana Tenuta San Giacomo	90	40.0	Sicily & Sardinia	Sicilia	NaN	Kerin O’Keefe	@kerinokeefe	Cusumano 2012 Sàgana Tenuta San Giacomo Nero d...	Nero d'Avola	Cusumano
    6648 rows × 13 columns


Suppose we'll buy any wine that's made in Italy or which is rated above average. For this we use a pipe (|):

    reviews.loc[(reviews.country == 'Italy') | (reviews.points >= 90)]

    country	description	designation	points	price	province	region_1	region_2	taster_name	taster_twitter_handle	title	variety	winery
    0	Italy	Aromas include tropical fruit, broom, brimston...	Vulkà Bianco	87	NaN	Sicily & Sardinia	Etna	NaN	Kerin O’Keefe	@kerinokeefe	Nicosia 2013 Vulkà Bianco (Etna)	White Blend	Nicosia
    6	Italy	Here's a bright, informal red that opens with ...	Belsito	87	16.0	Sicily & Sardinia	Vittoria	NaN	Kerin O’Keefe	@kerinokeefe	Terre di Giurfo 2013 Belsito Frappato (Vittoria)	Frappato	Terre di Giurfo
    ...	...	...	...	...	...	...	...	...	...	...	...	...	...
    129969	France	A dry style of Pinot Gris, this is crisp with ...	NaN	90	32.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Domaine Marcel Deiss 2012 Pinot Gris (Alsace)	Pinot Gris	Domaine Marcel Deiss
    129970	France	Big, rich and off-dry, this is powered by inte...	Lieu-dit Harth Cuvée Caroline	90	21.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Domaine Schoffit 2012 Lieu-dit Harth Cuvée Car...	Gewürztraminer	Domaine Schoffit
    61937 rows × 13 columns

Pandas comes with a few built-in conditional selectors, two of which we will highlight here. The first is isin. isin is lets you select data whose value "is in" a list of values.
For example, here's how we can use it to select wines only from Italy or France:

    reviews.loc[reviews.country.isin(['Italy', 'France'])]

    country	description	designation	points	price	province	region_1	region_2	taster_name	taster_twitter_handle	title	variety	winery
    0	Italy	Aromas include tropical fruit, broom, brimston...	Vulkà Bianco	87	NaN	Sicily & Sardinia	Etna	NaN	Kerin O’Keefe	@kerinokeefe	Nicosia 2013 Vulkà Bianco (Etna)	White Blend	Nicosia
    6	Italy	Here's a bright, informal red that opens with ...	Belsito	87	16.0	Sicily & Sardinia	Vittoria	NaN	Kerin O’Keefe	@kerinokeefe	Terre di Giurfo 2013 Belsito Frappato (Vittoria)	Frappato	Terre di Giurfo
    ...	...	...	...	...	...	...	...	...	...	...	...	...	...
    129969	France	A dry style of Pinot Gris, this is crisp with ...	NaN	90	32.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Domaine Marcel Deiss 2012 Pinot Gris (Alsace)	Pinot Gris	Domaine Marcel Deiss
    129970	France	Big, rich and off-dry, this is powered by inte...	Lieu-dit Harth Cuvée Caroline	90	21.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Domaine Schoffit 2012 Lieu-dit Harth Cuvée Car...	Gewürztraminer	Domaine Schoffit
    41633 rows × 13 columns


The second is isnull (and its companion notnull). These methods let you highlight values which are (or are not) empty (NaN). For example, to filter out wines lacking a price tag in the
dataset, here's what we would do:

    reviews.loc[reviews.price.notnull()]

    country	description	designation	points	price	province	region_1	region_2	taster_name	taster_twitter_handle	title	variety	winery
    1	Portugal	This is ripe and fruity, a wine that is smooth...	Avidagos	87	15.0	Douro	NaN	NaN	Roger Voss	@vossroger	Quinta dos Avidagos 2011 Avidagos Red (Douro)	Portuguese Red	Quinta dos Avidagos
    2	US	Tart and snappy, the flavors of lime flesh and...	NaN	87	14.0	Oregon	Willamette Valley	Willamette Valley	Paul Gregutt	@paulgwine	Rainstorm 2013 Pinot Gris (Willamette Valley)	Pinot Gris	Rainstorm
    ...	...	...	...	...	...	...	...	...	...	...	...	...	...
    129969	France	A dry style of Pinot Gris, this is crisp with ...	NaN	90	32.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Domaine Marcel Deiss 2012 Pinot Gris (Alsace)	Pinot Gris	Domaine Marcel Deiss
    129970	France	Big, rich and off-dry, this is powered by inte...	Lieu-dit Harth Cuvée Caroline	90	21.0	Alsace	Alsace	NaN	Roger Voss	@vossroger	Domaine Schoffit 2012 Lieu-dit Harth Cuvée Car...	Gewürztraminer	Domaine Schoffit
    120975 rows × 13 columns


# Assigning data
Going the other way, assigning data to a DataFrame is easy. You can assign either a constant value:

    reviews['critic'] = 'everyone'
    reviews['critic']

    0         everyone
    1         everyone
              ...   
    129969    everyone
    129970    everyone
    Name: critic, Length: 129971, dtype: object

Or with an iterable of values:

    reviews['index_backwards'] = range(len(reviews), 0, -1)

    reviews['index_backwards']
    0         129971
    1         129970
              ...  
    129969         2
    129970         1
    Name: index_backwards, Length: 129971, dtype: int64

