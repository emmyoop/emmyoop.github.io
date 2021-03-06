---
title:          ipynb test
username:       emmyoop
default:        true
image:          https://live.staticflickr.com/65535/50811759871_46087e7f0f_b.jpg
ipynb:          can maybe add link to github .md file for notebook so they dont have to be stored here?
---

# Ask Hacker News versus Tell Hacker News Analysis

Analyze if posts asking questions get more attention on Hacker News or if post showing things get more attention.

### General Project Setup


```python
from csv import reader
import datetime as dt
```


```python
DATASET_NAME_FULL = "HN_posts_year_to_Sep_26_2016.csv"
DATASET_NAME_SAMPLE = "hacker_news.csv"

ASK_HN = "ask hn"
TELL_HN = "show hn"

ID_COL = 0
TITLE_COL = 1
URL_COL = 2
POINTS_COL = 3
NBR_COMMENTS_COL = 4
AUTHOR_COL = 5
CREATED_DATE_COL = 6


```


```python
def read_in_file(file_name):
    open_file = open(file_name)
    read_file = reader(open_file)
    file_list = list(read_file)
    return file_list

hn_sample = read_in_file(DATASET_NAME_SAMPLE)
hn_full = read_in_file(DATASET_NAME_FULL)
```

### Data Exploration


```python
def data_summary(dataset, print_results=False):

    if print_results:
        print("   rows: " + str(len(dataset)))
        print("columns: " + str(len(dataset[1])))
        print("*" * 25)
#         print(dataset[:4])
    return len(dataset), len(dataset[1])
```


```python
data_summary(hn_full, True)
```

       rows: 293120
    columns: 7
    *************************
    [['id', 'title', 'url', 'num_points', 'num_comments', 'author', 'created_at'], ['12579008', 'You have two days to comment if you want stem cells to be classified as your own', 'http://www.regulations.gov/document?D=FDA-2015-D-3719-0018', '1', '0', 'altstar', '9/26/2016 3:26'], ['12579005', 'SQLAR  the SQLite Archiver', 'https://www.sqlite.org/sqlar/doc/trunk/README.md', '1', '0', 'blacksqr', '9/26/2016 3:24'], ['12578997', 'What if we just printed a flatscreen television on the side of our boxes?', 'https://medium.com/vanmoof/our-secrets-out-f21c1f03fdc8#.ietxmez43', '1', '0', 'pavel_lishin', '9/26/2016 3:19']]





    (293120, 7)




```python
data_summary(hn_sample, True)
```

       rows: 20101
    columns: 7
    *************************
    [['id', 'title', 'url', 'num_points', 'num_comments', 'author', 'created_at'], ['12224879', 'Interactive Dynamic Video', 'http://www.interactivedynamicvideo.com/', '386', '52', 'ne0phyte', '8/4/2016 11:52'], ['10975351', 'How to Use Open Source and Shut the Fuck Up at the Same Time', 'http://hueniverse.com/2016/01/26/how-to-use-open-source-and-shut-the-fuck-up-at-the-same-time/', '39', '10', 'josep2', '1/26/2016 19:30'], ['11964716', "Florida DJs May Face Felony for April Fools' Water Joke", 'http://www.thewire.com/entertainment/2013/04/florida-djs-april-fools-water-joke/63798/', '2', '1', 'vezycash', '6/23/2016 22:20']]





    (20101, 7)




```python
def dict_summary(hn_dict):
    max_key = max(hn_dict, key=lambda k: hn_dict[k])
    min_key = min(hn_dict, key=lambda k: hn_dict[k])
    print("Max: " + max_key + ":" + str(hn_dict[max_key]))
    print("Min: " + min_key + ":" + str(hn_dict[min_key]))
    print('\n')
    
def top_x_dicts(d, x, title, print_results=False):
    '''
    Takes in a dictionary in the format (hour: float) and number of results to return 
    and returns the top number of results as an ordered list.
    
    Can also print out the results.
    '''
    list_dict = []
    
    for k,v in d.items():
        list_dict.append([v,k])
    list_dict_sorted = sorted(list_dict, reverse=True)
    idx = 0
    for i in list_dict_sorted:
        reverse = [i[1], i[0]]
        list_dict_sorted[idx] = reverse
        idx += 1
    if print_results:
        print("Top " + str(x) + " Results")
        nbr = 1
        for i in list_dict_sorted:
            time = dt.datetime.strptime(i[0], "%H")
            output = "{place}:  {hour}: {cnt:.2f} {title} per hour"
            print(output.format(place=nbr, 
                                hour=dt.datetime.strftime(time, "%H:%M"), 
                                cnt=i[1],
                                title=title))
            nbr += 1
            if nbr > x:
                break
    
    return list_dict_sorted[:x]
                
        
```

### Initial Data Cleanup


```python
headers = hn_full[0]
hn_full.pop(0)
hn_sample.pop(0)
```




    ['id', 'title', 'url', 'num_points', 'num_comments', 'author', 'created_at']




```python
data_summary(hn_full, True)
data_summary(hn_sample, True)
print("*" * 25)
print(headers)
```

       rows: 293119
    columns: 7
    *************************
    [['12579008', 'You have two days to comment if you want stem cells to be classified as your own', 'http://www.regulations.gov/document?D=FDA-2015-D-3719-0018', '1', '0', 'altstar', '9/26/2016 3:26'], ['12579005', 'SQLAR  the SQLite Archiver', 'https://www.sqlite.org/sqlar/doc/trunk/README.md', '1', '0', 'blacksqr', '9/26/2016 3:24'], ['12578997', 'What if we just printed a flatscreen television on the side of our boxes?', 'https://medium.com/vanmoof/our-secrets-out-f21c1f03fdc8#.ietxmez43', '1', '0', 'pavel_lishin', '9/26/2016 3:19'], ['12578989', 'algorithmic music', 'http://cacm.acm.org/magazines/2011/7/109891-algorithmic-composition/fulltext', '1', '0', 'poindontcare', '9/26/2016 3:16']]
       rows: 20100
    columns: 7
    *************************
    [['12224879', 'Interactive Dynamic Video', 'http://www.interactivedynamicvideo.com/', '386', '52', 'ne0phyte', '8/4/2016 11:52'], ['10975351', 'How to Use Open Source and Shut the Fuck Up at the Same Time', 'http://hueniverse.com/2016/01/26/how-to-use-open-source-and-shut-the-fuck-up-at-the-same-time/', '39', '10', 'josep2', '1/26/2016 19:30'], ['11964716', "Florida DJs May Face Felony for April Fools' Water Joke", 'http://www.thewire.com/entertainment/2013/04/florida-djs-april-fools-water-joke/63798/', '2', '1', 'vezycash', '6/23/2016 22:20'], ['11919867', 'Technology ventures: From Idea to Enterprise', 'https://www.amazon.com/Technology-Ventures-Enterprise-Thomas-Byers/dp/0073523429', '3', '1', 'hswarna', '6/17/2016 0:01']]
    *************************
    ['id', 'title', 'url', 'num_points', 'num_comments', 'author', 'created_at']


### Filter to relevant posts


```python
def filter_by_begining(dataset):
    filtered_dataset_ask = []
    filtered_dataset_tell = []
    filtered_dataset_other = []

    for row in dataset:
        title = row[1].lower()
        if title.startswith(ASK_HN):
            filtered_dataset_ask.append(row)
        elif title.startswith(TELL_HN):
            filtered_dataset_tell.append(row)
        else:
            filtered_dataset_other.append(row)
    return filtered_dataset_ask, filtered_dataset_tell, filtered_dataset_other
                
```


```python
filtered_full_dataset_ask, filtered_full_dataset_tell, filtered_full_dataset_other = filter_by_begining(hn_full)

print("ASK HN Posts")
data_summary(filtered_full_dataset_ask, True)
print("*" * 25)
print("TEll HN Posts")
data_summary(filtered_full_dataset_tell, True)
print("*" * 25)
print("Other Posts")
data_summary(filtered_full_dataset_other, True)
```

    ASK HN Posts
       rows: 9139
    columns: 7
    *************************
    [['12578908', 'Ask HN: What TLD do you use for local development?', '', '4', '7', 'Sevrene', '9/26/2016 2:53'], ['12578522', 'Ask HN: How do you pass on your work when you die?', '', '6', '3', 'PascLeRasc', '9/26/2016 1:17'], ['12577908', 'Ask HN: How a DNS problem can be limited to a geographic region?', '', '1', '0', 'kuon', '9/25/2016 22:57'], ['12577870', 'Ask HN: Why join a fund when you can be an angel?', '', '1', '3', 'anthony_james', '9/25/2016 22:48']]
    *************************
    TEll HN Posts
       rows: 10158
    columns: 7
    *************************
    [['12578335', 'Show HN: Finding puns computationally', 'http://puns.samueltaylor.org/', '2', '0', 'saamm', '9/26/2016 0:36'], ['12578182', 'Show HN: A simple library for complicated animations', 'https://christinecha.github.io/choreographer-js/', '1', '0', 'christinecha', '9/26/2016 0:01'], ['12578098', 'Show HN: WebGL visualization of DNA sequences', 'http://grondilu.github.io/dna.html', '1', '0', 'grondilu', '9/25/2016 23:44'], ['12577991', 'Show HN: Pomodoro-centric, heirarchical project management with ES6 modules', 'https://github.com/jakebian/zeal', '2', '0', 'dbranes', '9/25/2016 23:17']]
    *************************
    Other Posts
       rows: 273822
    columns: 7
    *************************
    [['12579008', 'You have two days to comment if you want stem cells to be classified as your own', 'http://www.regulations.gov/document?D=FDA-2015-D-3719-0018', '1', '0', 'altstar', '9/26/2016 3:26'], ['12579005', 'SQLAR  the SQLite Archiver', 'https://www.sqlite.org/sqlar/doc/trunk/README.md', '1', '0', 'blacksqr', '9/26/2016 3:24'], ['12578997', 'What if we just printed a flatscreen television on the side of our boxes?', 'https://medium.com/vanmoof/our-secrets-out-f21c1f03fdc8#.ietxmez43', '1', '0', 'pavel_lishin', '9/26/2016 3:19'], ['12578989', 'algorithmic music', 'http://cacm.acm.org/magazines/2011/7/109891-algorithmic-composition/fulltext', '1', '0', 'poindontcare', '9/26/2016 3:16']]





    (273822, 7)




```python
filtered_sample_dataset_ask, filtered_sample_dataset_tell, filtered_sample_dataset_other = filter_by_begining(hn_sample)

print("ASK HN Posts")
data_summary(filtered_sample_dataset_ask, True)
print("*" * 25)
print("TEll HN Posts")
data_summary(filtered_sample_dataset_tell, True)
print("*" * 25)
print("Other Posts")
data_summary(filtered_sample_dataset_other, True)

```

    ASK HN Posts
       rows: 1744
    columns: 7
    *************************
    [['12296411', 'Ask HN: How to improve my personal website?', '', '2', '6', 'ahmedbaracat', '8/16/2016 9:55'], ['10610020', 'Ask HN: Am I the only one outraged by Twitter shutting down share counts?', '', '28', '29', 'tkfx', '11/22/2015 13:43'], ['11610310', 'Ask HN: Aby recent changes to CSS that broke mobile?', '', '1', '1', 'polskibus', '5/2/2016 10:14'], ['12210105', 'Ask HN: Looking for Employee #3 How do I do it?', '', '1', '3', 'sph130', '8/2/2016 14:20']]
    *************************
    TEll HN Posts
       rows: 1162
    columns: 7
    *************************
    [['10627194', 'Show HN: Wio Link  ESP8266 Based Web of Things Hardware Development Platform', 'https://iot.seeed.cc', '26', '22', 'kfihihc', '11/25/2015 14:03'], ['10646440', 'Show HN: Something pointless I made', 'http://dn.ht/picklecat/', '747', '102', 'dhotson', '11/29/2015 22:46'], ['11590768', 'Show HN: Shanhu.io, a programming playground powered by e8vm', 'https://shanhu.io', '1', '1', 'h8liu', '4/28/2016 18:05'], ['12178806', 'Show HN: Webscope  Easy way for web developers to communicate with Clients', 'http://webscopeapp.com', '3', '3', 'fastbrick', '7/28/2016 7:11']]
    *************************
    Other Posts
       rows: 17194
    columns: 7
    *************************
    [['12224879', 'Interactive Dynamic Video', 'http://www.interactivedynamicvideo.com/', '386', '52', 'ne0phyte', '8/4/2016 11:52'], ['10975351', 'How to Use Open Source and Shut the Fuck Up at the Same Time', 'http://hueniverse.com/2016/01/26/how-to-use-open-source-and-shut-the-fuck-up-at-the-same-time/', '39', '10', 'josep2', '1/26/2016 19:30'], ['11964716', "Florida DJs May Face Felony for April Fools' Water Joke", 'http://www.thewire.com/entertainment/2013/04/florida-djs-april-fools-water-joke/63798/', '2', '1', 'vezycash', '6/23/2016 22:20'], ['11919867', 'Technology ventures: From Idea to Enterprise', 'https://www.amazon.com/Technology-Ventures-Enterprise-Thomas-Byers/dp/0073523429', '3', '1', 'hswarna', '6/17/2016 0:01']]





    (17194, 7)



   ### Determine Average Questions asked


```python
def avg_by_column(dataset, column):
    nbr_comments = 0
    for row in dataset:
        try:
            nbr_comments += float(row[column])
        except:
            print("Error in Comments Columns: " + row[column])
            print(row)
    avg_comments = nbr_comments / len(dataset)
    
    return avg_comments
```


```python
avg_ask_comments_full = avg_by_column(filtered_full_dataset_ask, NBR_COMMENTS_COL)
avg_tell_comments_full = avg_by_column(filtered_full_dataset_tell, NBR_COMMENTS_COL)
avg_other_comments_full = avg_by_column(filtered_full_dataset_other, NBR_COMMENTS_COL)

results_string = "Average {title} Comments: {average}"
print("**FULL DATASET**")
print(results_string.format(title=ASK_HN, average=avg_ask_comments_full))
print(results_string.format(title=TELL_HN, average=avg_tell_comments_full))
print(results_string.format(title="Other", average=avg_other_comments_full))
```

    **FULL DATASET**
    Average ask hn Comments: 10.393478498741656
    Average show hn Comments: 4.886099625910612
    Average Other Comments: 6.4572678601427205



```python
avg_ask_comments_sample = avg_by_column(filtered_sample_dataset_ask, NBR_COMMENTS_COL)
avg_tell_comments_sample = avg_by_column(filtered_sample_dataset_tell, NBR_COMMENTS_COL)
avg_other_comments_sample = avg_by_column(filtered_sample_dataset_other, NBR_COMMENTS_COL)

results_string = "Average {title} Comments: {average}"

print("**SAMPLE DATASET**")
print(results_string.format(title=ASK_HN, average=avg_ask_comments_sample))
print(results_string.format(title=TELL_HN, average=avg_tell_comments_sample))
print(results_string.format(title="Other", average=avg_other_comments_sample))
```

    **SAMPLE DATASET**
    Average ask hn Comments: 14.038417431192661
    Average show hn Comments: 10.31669535283993
    Average Other Comments: 26.8730371059672


#### Do above results point to question popularity?

I really don't think there is enough information yet to draw any conclusions.  It would probably be useful to also take the points into consideration as well.

But Ask Posts do receive more comments on average than tell posts.


```python
avg_ask_points_full = avg_by_column(filtered_full_dataset_ask, POINTS_COL)
avg_tell_points_full = avg_by_column(filtered_full_dataset_tell, POINTS_COL)
avg_other_points_full = avg_by_column(filtered_full_dataset_other, POINTS_COL)

results_string = "Average {title} Points: {average}"

print("**FULL DATASET**")
print(results_string.format(title=ASK_HN, average=avg_ask_points_full))
print(results_string.format(title=TELL_HN, average=avg_tell_points_full))
print(results_string.format(title="Other", average=avg_other_points_full))
```

    **FULL DATASET**
    Average ask hn Points: 11.31174089068826
    Average show hn Points: 14.843571569206537
    Average Other Points: 15.156010108756783



```python
avg_ask_points_sample = avg_by_column(filtered_sample_dataset_ask, POINTS_COL)
avg_tell_points_sample = avg_by_column(filtered_sample_dataset_tell, POINTS_COL)
avg_other_points_sample = avg_by_column(filtered_sample_dataset_other, POINTS_COL)

results_string = "Average {title} Points: {average}"

print("**SAMPLE DATASET**")
print(results_string.format(title=ASK_HN, average=avg_ask_points_sample))
print(results_string.format(title=TELL_HN, average=avg_tell_points_sample))
print(results_string.format(title="Other", average=avg_other_points_sample))
```

    **SAMPLE DATASET**
    Average ask hn Points: 15.061926605504587
    Average show hn Points: 27.555077452667813
    Average Other Points: 55.4067698034198


### Determine if time effects post popularity


```python
def convert_datetimes(dataset, col):
    datetime_dataset = []
    
    fmts = ["%m/%d/%Y %H:%M", "%Y-%M-%D %H:%M:%S"]
    
    for row in dataset:
        if type(row[col]) != dt.datetime:  # needed for rerunability
            for fmt in fmts:
                try:
                    row[col] = dt.datetime.strptime(row[col], fmt)
                    break
                except ValueError as err:
                    pass
        datetime_dataset.append(row)
    
    return datetime_dataset

def hourly_counts(dataset, comments_col, time_col, points_col):
    hourly_dict = {}
    total_hourly_comment_dict = {}
    avg_hourly_comment_dict = {}
    total_hourly_points_dict = {}
    avg_hourly_points_dict = {}
    
    
    for row in dataset:
        hour = dt.datetime.strftime(row[time_col], "%H")
        if hour in hourly_dict:
            hourly_dict[hour] += 1
        else:
            hourly_dict[hour] = 1
        
        if hour in total_hourly_comment_dict:
            total_hourly_comment_dict[hour] += int(row[comments_col])
        else:
            total_hourly_comment_dict[hour] = int(row[comments_col])
        
        if hour in total_hourly_points_dict:
            total_hourly_points_dict[hour] += int(row[points_col])
        else:
            total_hourly_points_dict[hour] = int(row[points_col])
    
    for key in total_hourly_comment_dict:
        avg_hourly_comment_dict[key] = total_hourly_comment_dict[key] / hourly_dict[key]
    for key in total_hourly_points_dict:
        avg_hourly_points_dict[key] = total_hourly_points_dict[key] / hourly_dict[key]
    
    return hourly_dict, total_hourly_comment_dict, avg_hourly_comment_dict, total_hourly_points_dict, avg_hourly_points_dict


```


```python
def display_summary(dataset, name):
    
    filtered_dataset_dt = convert_datetimes(dataset, CREATED_DATE_COL)

    if data_summary(dataset) != data_summary(filtered_dataset_dt):
        print("Number of results has changed unexpededly!")

    print('*' * 25)
    print(name +' Posts  -  ' + str(len(dataset)))
    print('*' * 25)
    hourly, hourly_comments, avg_hourly_comments, hourly_point, avg_hourly_point = hourly_counts(filtered_dataset_dt, 
                                                                                    NBR_COMMENTS_COL, 
                                                                                    CREATED_DATE_COL,
                                                                                    POINTS_COL)

    print('Posts By Hour')
    dict_summary(hourly)
    top_x_dicts(hourly, 5, "Total Posts", True)
    print('*' * 15)
    print('Total Comments By Hour')
    dict_summary(hourly_comments)
    top_x_dicts(hourly_comments, 5,"Total Comments", True)
    print('*' * 15)
    print('Average Comments By Hour')
    dict_summary(avg_hourly_comments)
    top_x_dicts(avg_hourly_comments, 5,"Average Comments", True)
    print('*' * 15)
    print('Total Points By Hour')
    dict_summary(hourly_point)
    top_x_dicts(hourly_point, 5,"Total Points", True)
    print('*' * 15)
    print('Average Points By Hour')
    dict_summary(avg_hourly_point)
    top_x_dicts(avg_hourly_point, 5,"Average Points", True)
```

## FULL DATASET ANALYSIS


```python
display_summary(filtered_full_dataset_ask, "ASK")
```

    *************************
    ASK Posts  -  9139
    *************************
    Posts By Hour
    Max: 15:646
    Min: 05:209
    
    
    Top 5 Results
    1:  15:00: 646.00 Total Posts per hour
    2:  18:00: 614.00 Total Posts per hour
    3:  17:00: 587.00 Total Posts per hour
    4:  16:00: 579.00 Total Posts per hour
    5:  19:00: 552.00 Total Posts per hour
    ***************
    Total Comments By Hour
    Max: 15:18525
    Min: 09:1477
    
    
    Top 5 Results
    1:  15:00: 18525.00 Total Comments per hour
    2:  13:00: 7245.00 Total Comments per hour
    3:  17:00: 5547.00 Total Comments per hour
    4:  14:00: 4972.00 Total Comments per hour
    5:  18:00: 4877.00 Total Comments per hour
    ***************
    Average Comments By Hour
    Max: 15:28.676470588235293
    Min: 09:6.653153153153153
    
    
    Top 5 Results
    1:  15:00: 28.68 Average Comments per hour
    2:  13:00: 16.32 Average Comments per hour
    3:  12:00: 12.38 Average Comments per hour
    4:  02:00: 11.14 Average Comments per hour
    5:  10:00: 10.68 Average Comments per hour
    ***************
    Total Points By Hour
    Max: 15:13978
    Min: 09:1763
    
    
    Top 5 Results
    1:  15:00: 13978.00 Total Points per hour
    2:  13:00: 7962.00 Total Points per hour
    3:  17:00: 7155.00 Total Points per hour
    4:  18:00: 6850.00 Total Points per hour
    5:  16:00: 5970.00 Total Points per hour
    ***************
    Average Points By Hour
    Max: 15:21.637770897832816
    Min: 23:7.626822157434402
    
    
    Top 5 Results
    1:  15:00: 21.64 Average Points per hour
    2:  13:00: 17.93 Average Points per hour
    3:  12:00: 13.58 Average Points per hour
    4:  10:00: 13.44 Average Points per hour
    5:  17:00: 12.19 Average Points per hour



```python
display_summary(filtered_full_dataset_tell, "TELL")

```

    *************************
    TELL Posts  -  10158
    *************************
    Posts By Hour
    Max: 15:836
    Min: 05:172
    
    
    Top 5 Results
    1:  15:00: 836.00 Total Posts per hour
    2:  16:00: 801.00 Total Posts per hour
    3:  17:00: 761.00 Total Posts per hour
    4:  14:00: 696.00 Total Posts per hour
    5:  18:00: 656.00 Total Posts per hour
    ***************
    Total Comments By Hour
    Max: 14:3839
    Min: 05:592
    
    
    Top 5 Results
    1:  14:00: 3839.00 Total Comments per hour
    2:  15:00: 3824.00 Total Comments per hour
    3:  16:00: 3769.00 Total Comments per hour
    4:  12:00: 3609.00 Total Comments per hour
    5:  13:00: 3314.00 Total Comments per hour
    ***************
    Average Comments By Hour
    Max: 12:6.994186046511628
    Min: 05:3.441860465116279
    
    
    Top 5 Results
    1:  12:00: 6.99 Average Comments per hour
    2:  07:00: 6.68 Average Comments per hour
    3:  11:00: 6.00 Average Comments per hour
    4:  08:00: 5.60 Average Comments per hour
    5:  14:00: 5.52 Average Comments per hour
    ***************
    Total Points By Hour
    Max: 15:11657
    Min: 05:1834
    
    
    Top 5 Results
    1:  15:00: 11657.00 Total Points per hour
    2:  16:00: 11487.00 Total Points per hour
    3:  12:00: 10787.00 Total Points per hour
    4:  17:00: 10563.00 Total Points per hour
    5:  14:00: 10503.00 Total Points per hour
    ***************
    Average Points By Hour
    Max: 12:20.905038759689923
    Min: 03:10.524271844660195
    
    
    Top 5 Results
    1:  12:00: 20.91 Average Points per hour
    2:  11:00: 19.26 Average Points per hour
    3:  13:00: 17.02 Average Points per hour
    4:  19:00: 16.06 Average Points per hour
    5:  06:00: 15.99 Average Points per hour



```python
display_summary(filtered_full_dataset_other, "OTHER")
```

    *************************
    OTHER Posts  -  273822
    *************************
    Posts By Hour
    Max: 16:18790
    Min: 05:6155
    
    
    Top 5 Results
    1:  16:00: 18790.00 Total Posts per hour
    2:  17:00: 18363.00 Total Posts per hour
    3:  15:00: 18043.00 Total Posts per hour
    4:  18:00: 17406.00 Total Posts per hour
    5:  14:00: 16929.00 Total Posts per hour
    ***************
    Total Comments By Hour
    Max: 17:118217
    Min: 05:41773
    
    
    Top 5 Results
    1:  17:00: 118217.00 Total Comments per hour
    2:  16:00: 116322.00 Total Comments per hour
    3:  15:00: 115286.00 Total Comments per hour
    4:  18:00: 112502.00 Total Comments per hour
    5:  14:00: 108277.00 Total Comments per hour
    ***************
    Average Comments By Hour
    Max: 12:7.58521387672617
    Min: 22:5.838466157673501
    
    
    Top 5 Results
    1:  12:00: 7.59 Average Comments per hour
    2:  11:00: 7.37 Average Comments per hour
    3:  02:00: 7.18 Average Comments per hour
    4:  13:00: 7.15 Average Comments per hour
    5:  05:00: 6.79 Average Comments per hour
    ***************
    Total Points By Hour
    Max: 17:277696
    Min: 05:96617
    
    
    Top 5 Results
    1:  17:00: 277696.00 Total Points per hour
    2:  16:00: 275203.00 Total Points per hour
    3:  18:00: 268580.00 Total Points per hour
    4:  15:00: 262514.00 Total Points per hour
    5:  19:00: 248023.00 Total Points per hour
    ***************
    Average Points By Hour
    Max: 02:16.712053891357318
    Min: 20:13.785120643431636
    
    
    Top 5 Results
    1:  02:00: 16.71 Average Points per hour
    2:  12:00: 16.70 Average Points per hour
    3:  11:00: 16.29 Average Points per hour
    4:  00:00: 16.12 Average Points per hour
    5:  13:00: 16.02 Average Points per hour


## SAMPLE DATASET


```python
display_summary(filtered_sample_dataset_ask, "ASK")
```

    *************************
    ASK Posts  -  1744
    *************************
    Posts By Hour
    Max: 15:116
    Min: 07:34
    
    
    Top 5 Results
    1:  15:00: 116.00 Total Posts per hour
    2:  19:00: 110.00 Total Posts per hour
    3:  21:00: 109.00 Total Posts per hour
    4:  18:00: 109.00 Total Posts per hour
    5:  16:00: 108.00 Total Posts per hour
    ***************
    Total Comments By Hour
    Max: 15:4477
    Min: 09:251
    
    
    Top 5 Results
    1:  15:00: 4477.00 Total Comments per hour
    2:  16:00: 1814.00 Total Comments per hour
    3:  21:00: 1745.00 Total Comments per hour
    4:  20:00: 1722.00 Total Comments per hour
    5:  18:00: 1439.00 Total Comments per hour
    ***************
    Average Comments By Hour
    Max: 15:38.5948275862069
    Min: 09:5.5777777777777775
    
    
    Top 5 Results
    1:  15:00: 38.59 Average Comments per hour
    2:  02:00: 23.81 Average Comments per hour
    3:  20:00: 21.52 Average Comments per hour
    4:  16:00: 16.80 Average Comments per hour
    5:  21:00: 16.01 Average Comments per hour
    ***************
    Total Points By Hour
    Max: 15:3479
    Min: 09:329
    
    
    Top 5 Results
    1:  15:00: 3479.00 Total Points per hour
    2:  16:00: 2522.00 Total Points per hour
    3:  13:00: 2062.00 Total Points per hour
    4:  17:00: 1941.00 Total Points per hour
    5:  18:00: 1741.00 Total Points per hour
    ***************
    Average Points By Hour
    Max: 15:29.99137931034483
    Min: 03:6.925925925925926
    
    
    Top 5 Results
    1:  15:00: 29.99 Average Points per hour
    2:  13:00: 24.26 Average Points per hour
    3:  16:00: 23.35 Average Points per hour
    4:  17:00: 19.41 Average Points per hour
    5:  10:00: 18.68 Average Points per hour



```python
display_summary(filtered_sample_dataset_tell, "TELL")
```

    *************************
    TELL Posts  -  1162
    *************************
    Posts By Hour
    Max: 13:99
    Min: 06:16
    
    
    Top 5 Results
    1:  13:00: 99.00 Total Posts per hour
    2:  17:00: 93.00 Total Posts per hour
    3:  16:00: 93.00 Total Posts per hour
    4:  14:00: 86.00 Total Posts per hour
    5:  15:00: 78.00 Total Posts per hour
    ***************
    Total Comments By Hour
    Max: 14:1156
    Min: 05:58
    
    
    Top 5 Results
    1:  14:00: 1156.00 Total Comments per hour
    2:  16:00: 1084.00 Total Comments per hour
    3:  18:00: 962.00 Total Comments per hour
    4:  13:00: 946.00 Total Comments per hour
    5:  17:00: 911.00 Total Comments per hour
    ***************
    Average Comments By Hour
    Max: 18:15.770491803278688
    Min: 05:3.0526315789473686
    
    
    Top 5 Results
    1:  18:00: 15.77 Average Comments per hour
    2:  00:00: 15.71 Average Comments per hour
    3:  14:00: 13.44 Average Comments per hour
    4:  23:00: 12.42 Average Comments per hour
    5:  22:00: 12.39 Average Comments per hour
    ***************
    Total Points By Hour
    Max: 16:2634
    Min: 05:104
    
    
    Top 5 Results
    1:  16:00: 2634.00 Total Points per hour
    2:  12:00: 2543.00 Total Points per hour
    3:  17:00: 2521.00 Total Points per hour
    4:  13:00: 2438.00 Total Points per hour
    5:  15:00: 2228.00 Total Points per hour
    ***************
    Average Points By Hour
    Max: 23:42.388888888888886
    Min: 05:5.473684210526316
    
    
    Top 5 Results
    1:  23:00: 42.39 Average Points per hour
    2:  12:00: 41.69 Average Points per hour
    3:  22:00: 40.35 Average Points per hour
    4:  00:00: 37.84 Average Points per hour
    5:  18:00: 36.31 Average Points per hour



```python
display_summary(filtered_sample_dataset_other, "OTHER")
```

    *************************
    OTHER Posts  -  17194
    *************************
    Posts By Hour
    Max: 17:1169
    Min: 05:388
    
    
    Top 5 Results
    1:  17:00: 1169.00 Total Posts per hour
    2:  16:00: 1101.00 Total Posts per hour
    3:  18:00: 1084.00 Total Posts per hour
    4:  15:00: 1040.00 Total Posts per hour
    5:  19:00: 980.00 Total Posts per hour
    ***************
    Total Comments By Hour
    Max: 17:32727
    Min: 06:8714
    
    
    Top 5 Results
    1:  17:00: 32727.00 Total Comments per hour
    2:  14:00: 30973.00 Total Comments per hour
    3:  15:00: 30700.00 Total Comments per hour
    4:  18:00: 29186.00 Total Comments per hour
    5:  13:00: 28363.00 Total Comments per hour
    ***************
    Average Comments By Hour
    Max: 14:32.33089770354906
    Min: 06:21.357843137254903
    
    
    Top 5 Results
    1:  14:00: 32.33 Average Comments per hour
    2:  13:00: 30.90 Average Comments per hour
    3:  12:00: 30.35 Average Comments per hour
    4:  11:00: 29.59 Average Comments per hour
    5:  15:00: 29.52 Average Comments per hour
    ***************
    Total Points By Hour
    Max: 17:67777
    Min: 06:18864
    
    
    Top 5 Results
    1:  17:00: 67777.00 Total Points per hour
    2:  15:00: 62964.00 Total Points per hour
    3:  16:00: 59655.00 Total Points per hour
    4:  14:00: 59191.00 Total Points per hour
    5:  19:00: 58811.00 Total Points per hour
    ***************
    Average Points By Hour
    Max: 13:62.525054466230934
    Min: 20:45.24478594950604
    
    
    Top 5 Results
    1:  13:00: 62.53 Average Points per hour
    2:  14:00: 61.79 Average Points per hour
    3:  15:00: 60.54 Average Points per hour
    4:  10:00: 60.48 Average Points per hour
    5:  19:00: 60.01 Average Points per hour


### Results

I was curious about the results of looking at a sample vs looking at the entire dataset.  

The results were that posting at 3pm eastern results, on average, in the most comments.  However, in the random (I do not actually know how this sample was taken and if it was indeed random) sample, there were more Ask HN posts than Tell HN posts but in the full dataset the opposite was true.  It was about a 10% difference which is not insignificant.  

But, in both question categories posts at 3pm eastern resulted in the most comments in each post category, but on average ASK HN posts received more comments.

I also took at look at the total points questions received based on when they were posted.  ASk HN questions received teh most points, on average, when posted at 3pm eastern but TEll HN questions did not.  Questions posted at 3pm eastern did not even rank in the top five.  Since the points are based on the total of +1 and -1 points awarded I'm not sure looking at the time posted readlly gived much information about that.  If we could read the data of number of users who awarded points, positive or negative, we might be able to draw a more informed conclusion.

To have the best chance of receiving attention on Hacker News, it would be best to post an ASK HN topic at 3pm eastern.


