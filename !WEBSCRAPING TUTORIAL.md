# Load the necessary libraries



```python
import requests #pip install requests
from bs4 import BeautifulSoup as bs #pip install beautiful soup 4
```

# Load our first page


```python
# Load the webpage content

r=requests.get("https://keithgalli.github.io/web-scraping/example.html")

#Convert to a beautiful soup object
soup=bs(r.content) #So we are taking the content and converting it into beautifulSoup object

#Print out our html
print(soup)
```

    <html>
    <head>
    <title>HTML Example</title>
    </head>
    <body>
    <div align="middle">
    <h1>HTML Webpage</h1>
    <p>Link to more interesting example: <a href="https://keithgalli.github.io/web-scraping/webpage.html">keithgalli.github.io/web-scraping/webpage.html</a></p>
    </div>
    <h2>A Header</h2>
    <p><i>Some italicized text</i></p>
    <h2>Another header</h2>
    <p id="paragraph-id"><b>Some bold text</b></p>
    </body>
    </html>
    
    


```python
print(soup.prettify()) #It shows all the indents 
```

    <html>
     <head>
      <title>
       HTML Example
      </title>
     </head>
     <body>
      <div align="middle">
       <h1>
        HTML Webpage
       </h1>
       <p>
        Link to more interesting example:
        <a href="https://keithgalli.github.io/web-scraping/webpage.html">
         keithgalli.github.io/web-scraping/webpage.html
        </a>
       </p>
      </div>
      <h2>
       A Header
      </h2>
      <p>
       <i>
        Some italicized text
       </i>
      </p>
      <h2>
       Another header
      </h2>
      <p id="paragraph-id">
       <b>
        Some bold text
       </b>
      </p>
     </body>
    </html>
    
    

# Start using Beautiful Soup to Scrape



## find and find_all


```python
soup
```




    <html>
    <head>
    <title>HTML Example</title>
    </head>
    <body>
    <div align="middle">
    <h1>HTML Webpage</h1>
    <p>Link to more interesting example: <a href="https://keithgalli.github.io/web-scraping/webpage.html">keithgalli.github.io/web-scraping/webpage.html</a></p>
    </div>
    <h2>A Header</h2>
    <p><i>Some italicized text</i></p>
    <h2>Another header</h2>
    <p id="paragraph-id"><b>Some bold text</b></p>
    </body>
    </html>




```python
#let's say that we just want to grab these h2 elements

h2_find=soup.find("h2")
h2_find

#It just gives us the first occurrence of it.
```




    <h2>A Header</h2>




```python
#What if we want all of the occurences occuring in the h2 tags.
h2_find_all=soup.find_all("h2")
h2_find_all
```




    [<h2>A Header</h2>, <h2>Another header</h2>]




```python
#Passing a list as the parameters

all_head_find=soup.find(["h1","h2"])
all_head_find

#So it will go and find the first element (the same ele present in the list irrespective of the order ) and return it
```




    <h1>HTML Webpage</h1>




```python
#If want all the tags in our list to be found

all_head=soup.find_all(["h1","h2"])
all_head
```




    [<h1>HTML Webpage</h1>, <h2>A Header</h2>, <h2>Another header</h2>]




```python
#We can pass attributes in our find functions

paragraph=soup.find_all("p",attrs={"id":"paragraph-id"}) #Attribute written in dictionary style.
paragraph

```




    [<p id="paragraph-id"><b>Some bold text</b></p>]




```python
#You can nest find and find all calls.
#let's say we want all the stuff inside the body.

body=soup.find('body')
body
```




    <body>
    <div align="middle">
    <h1>HTML Webpage</h1>
    <p>Link to more interesting example: <a href="https://keithgalli.github.io/web-scraping/webpage.html">keithgalli.github.io/web-scraping/webpage.html</a></p>
    </div>
    <h2>A Header</h2>
    <p><i>Some italicized text</i></p>
    <h2>Another header</h2>
    <p id="paragraph-id"><b>Some bold text</b></p>
    </body>




```python
#Now let's say that I want to find a dive inside the body.
div=body.find('div')
div
#We were able to nest find/find_all calls

headd=div.find('p')
headd
```




    <p>Link to more interesting example: <a href="https://keithgalli.github.io/web-scraping/webpage.html">keithgalli.github.io/web-scraping/webpage.html</a></p>




```python
#We can reach specific strings in our find/findall calls

soup.find_all("p",string="Some") 
#Our answer is empty, if you look at the strings they consist of more words other than some.. so 
#We need to find some other way to extract the strings which contain the particular word that we need..
#Kind of how we use greple from re during filtering.

```




    []




```python
import re

paragraphs=soup.find_all("p",string=re.compile("Some"))
paragraphs


```




    [<p><i>Some italicized text</i></p>,
     <p id="paragraph-id"><b>Some bold text</b></p>]




```python
headers=soup.find_all("h2",string=re.compile("header"))
headers
```




    [<h2>Another header</h2>]




```python
headers_case=soup.find_all("h2",string=re.compile("(H|h)eader"))
headers_case
```




    [<h2>A Header</h2>, <h2>Another header</h2>]




```python
soup.body #Short hand
```




    <body>
    <div align="middle">
    <h1>HTML Webpage</h1>
    <p>Link to more interesting example: <a href="https://keithgalli.github.io/web-scraping/webpage.html">keithgalli.github.io/web-scraping/webpage.html</a></p>
    </div>
    <h2>A Header</h2>
    <p><i>Some italicized text</i></p>
    <h2>Another header</h2>
    <p id="paragraph-id"><b>Some bold text</b></p>
    </body>



## Select (CSS selector)


```python
content=soup.select("p")
content #Selecting all the paragraph tag..
```




    [<p>Link to more interesting example: <a href="https://keithgalli.github.io/web-scraping/webpage.html">keithgalli.github.io/web-scraping/webpage.html</a></p>,
     <p><i>Some italicized text</i></p>,
     <p id="paragraph-id"><b>Some bold text</b></p>]




```python
#We can also be very precise 

precise=soup.select("div p") #We want them to select paragraphs inside div
precise
```




    [<p>Link to more interesting example: <a href="https://keithgalli.github.io/web-scraping/webpage.html">keithgalli.github.io/web-scraping/webpage.html</a></p>]




```python
#We want all the paragraphs preceded by header2


para=soup.select("h2 ~ p")
para
```




    [<p><i>Some italicized text</i></p>,
     <p id="paragraph-id"><b>Some bold text</b></p>]




```python
#We need the bold text tag inside the paragraph with the id, id="paragraph-id"

bold_text=soup.select("p#paragraph-id b")
bold_text
```




    [<b>Some bold text</b>]




```python
#Trying nested select

#We want the direct child (paragraph) of the body tag,#Direct descendents
parags=soup.select("body>p")
parags
```




    [<p><i>Some italicized text</i></p>,
     <p id="paragraph-id"><b>Some bold text</b></p>]




```python
for paragss in parags:
    print(paragss.select("i"))
```

    [<i>Some italicized text</i>]
    []
    


```python
#Grab by element with specific property
#Eg:- Alignment
soup.select("[align=middle]")
```




    [<div align="middle">
     <h1>HTML Webpage</h1>
     <p>Link to more interesting example: <a href="https://keithgalli.github.io/web-scraping/webpage.html">keithgalli.github.io/web-scraping/webpage.html</a></p>
     </div>]



## Get different properties of the HTML


```python
#string within an element(so just text and not the full tag element)

headerr=soup.find("h2")
print(headerr) #So, we do get that tag.. how do we remove it?


print(headerr.string)
```

    <h2>A Header</h2>
    A Header
    


```python
#Another example, but here there is a slight problem...
#We need all the text in the given div part.

div=soup.find("div")
print(div.prettify())
print(div.string) #We get None here.. But why?

#That is because there are two tags on ele as the same level as children so it is confused as to choose which one...
#HTML Webpage or Link to more interesting example:....
```

    <div align="middle">
     <h1>
      HTML Webpage
     </h1>
     <p>
      Link to more interesting example:
      <a href="https://keithgalli.github.io/web-scraping/webpage.html">
       keithgalli.github.io/web-scraping/webpage.html
      </a>
     </p>
    </div>
    
    None
    


```python
#For that we have a seperate method...
#get_text..

print(div.get_text()) #It returned to me all the text in div.
```

    
    HTML Webpage
    Link to more interesting example: keithgalli.github.io/web-scraping/webpage.html
    
    

### if multiple child elements then use get_text or use string. (Talking about the above code)

## How to get the links



```python
#Get specific property from the links


link=soup.find("a")
print(link)

link['href'] #We are directly getting that link..
```

    <a href="https://keithgalli.github.io/web-scraping/webpage.html">keithgalli.github.io/web-scraping/webpage.html</a>
    




    'https://keithgalli.github.io/web-scraping/webpage.html'




```python
paragraphs=soup.select('p#paragraph-id')
print(paragraphs)

#id="paragraph-id" we only need this part...
#So we know that paragraphs has it's contents in a list


print(paragraphs[0]['id'])
```

    [<p id="paragraph-id"><b>Some bold text</b></p>]
    paragraph-id
    

## Code navigation


```python
soup.body.div.h1.string #Using shortcuts
```




    'HTML Webpage'



### Knowing the following terms

    1) Parents- body is the parent tag of the other stuff inside it
    2) Children:- div, p are the children of the parent tag 
    3) Sibling:- Children on the same level.


```python
#There are many methods based on these terms alone..

#Finding the next siblings

test=soup.body.find("div")
print(test)
test.find_next_siblings()



```

    <div align="middle">
    <h1>HTML Webpage</h1>
    <p>Link to more interesting example: <a href="https://keithgalli.github.io/web-scraping/webpage.html">keithgalli.github.io/web-scraping/webpage.html</a></p>
    </div>
    




    [<h2>A Header</h2>,
     <p><i>Some italicized text</i></p>,
     <h2>Another header</h2>,
     <p id="paragraph-id"><b>Some bold text</b></p>]



# Excercise!!


```python
#Here is the source web page.

# https://keithgalli.github.io/web-scraping/webpage.html
```

###Load the web content


```python
j=requests.get("https://keithgalli.github.io/web-scraping/webpage.html")

obj1=bs(j.content)
print(obj1.prettify())
```

    <html>
     <head>
      <title>
       Keith Galli's Page
      </title>
      <style>
       table {
        border-collapse: collapse;
      }
      th {
        padding:5px;
      }
      td {
        border: 1px solid #ddd;
        padding: 5px;
      }
      tr:nth-child(even) {
        background-color: #f2f2f2;
      }
      th {
        padding-top: 12px;
        padding-bottom: 12px;
        text-align: left;
        background-color: #add8e6;
        color: black;
      }
      .block {
      width: 100px;
      /*float: left;*/
        display: inline-block;
        zoom: 1;
      }
      .column {
      float: left;
      height: 200px;
      /*width: 33.33%;*/
      padding: 5px;
      }
    
      .row::after {
        content: "";
        clear: both;
        display: table;
      }
      </style>
     </head>
     <body>
      <h1>
       Welcome to my page!
      </h1>
      <img src="./images/selfie1.jpg" width="300px"/>
      <h2>
       About me
      </h2>
      <p>
       Hi, my name is Keith and I am a YouTuber who focuses on content related to programming, data science, and machine learning!
      </p>
      <p>
       Here is a link to my channel:
       <a href="https://www.youtube.com/kgmit">
        youtube.com/kgmit
       </a>
      </p>
      <p>
       I grew up in the great state of New Hampshire here in the USA. From an early age I always loved math. Around my senior year of high school, my brother first introduced me to programming. I found it a creative way to apply the same type of logical thinking skills that I enjoyed with math. This influenced me to study computer science in college and ultimately create a YouTube channel to share some things that I have learned along the way.
      </p>
      <h3>
       Hobbies
      </h3>
      <p>
       Believe it or not, I don't code 24/7. I love doing all sorts of active things. I like to play ice hockey &amp; table tennis as well as run, hike, skateboard, and snowboard. In addition to sports, I am a board game enthusiast. The two that I've been playing the most recently are
       <i>
        Settlers of Catan
       </i>
       and
       <i>
        Othello
       </i>
       .
      </p>
      <h3>
       Fun Facts
      </h3>
      <ul class="fun-facts">
       <li>
        Owned my dream car in high school
        <a href="#footer">
         <sup>
          1
         </sup>
        </a>
       </li>
       <li>
        Middle name is Ronald
       </li>
       <li>
        Never had been on a plane until college
       </li>
       <li>
        Dunkin Donuts coffee is better than Starbucks
       </li>
       <li>
        A favorite book series of mine is
        <i>
         Ender's Game
        </i>
       </li>
       <li>
        Current video game of choice is
        <i>
         Rocket League
        </i>
       </li>
       <li>
        The band that I've seen the most times live is the
        <i>
         Zac Brown Band
        </i>
       </li>
      </ul>
      <h2>
       Social Media
      </h2>
      I encourage you to check out my content on all social media platforms
      <br/>
      <ul class="socials">
       <li class="social instagram">
        <b>
         Instagram:
        </b>
        <a href="https://www.instagram.com/keithgalli/">
         https://www.instagram.com/keithgalli/
        </a>
       </li>
       <li class="social twitter">
        <b>
         Twitter:
        </b>
        <a href="https://twitter.com/keithgalli">
         https://twitter.com/keithgalli
        </a>
       </li>
       <li class="social linkedin">
        <b>
         LinkedIn:
        </b>
        <a href="https://www.linkedin.com/in/keithgalli/">
         https://www.linkedin.com/in/keithgalli/
        </a>
       </li>
       <li class="social tiktok">
        <b>
         TikTok:
        </b>
        <a href="https://www.tiktok.com/@keithgalli">
         https://www.tiktok.com/@keithgalli
        </a>
       </li>
      </ul>
      <h2>
       Photos
      </h2>
      Here are a few photos from a trip to italy I took last year
      <div class="row">
       <div class="column">
        <img alt="Lake Como" src="images/italy/lake_como.jpg" style="height:100%"/>
       </div>
       <div class="column">
        <img alt="Pontevecchio, Florence" src="images/italy/pontevecchio.jpg" style="height:100%"/>
       </div>
       <div class="column">
        <img alt="Riomaggiore, Cinque de Terre" src="images/italy/riomaggiore.jpg" style="height:100%"/>
       </div>
      </div>
      <div>
      </div>
      <h2>
       Table
      </h2>
      My MIT hockey stats :)
      <br/>
      <table class="hockey-stats">
       <thead>
        <tr>
         <th class="season" data-sort="">
          S
         </th>
         <th class="team" data-sort="team">
          Team
         </th>
         <th class="league" data-sort="league">
          League
         </th>
         <th class="regular gp" data-sort="gp">
          GP
         </th>
         <th class="regular g" data-sort="g">
          G
         </th>
         <th class="regular a" data-sort="a">
          A
         </th>
         <th class="regular tp" data-sort="tp">
          TP
         </th>
         <th class="regular pim" data-sort="pim">
          PIM
         </th>
         <th class="regular pm" data-sort="pm">
          +/-
         </th>
         <th class="separator">
         </th>
         <th class="postseason">
          POST
         </th>
         <th class="postseason gp" data-sort="playoffs-gp">
          GP
         </th>
         <th class="postseason g" data-sort="playoffs-g">
          G
         </th>
         <th class="postseason a" data-sort="playoffs-a">
          A
         </th>
         <th class="postseason tp" data-sort="playoffs-tp">
          TP
         </th>
         <th class="postseason pim" data-sort="playoffs-pim">
          PIM
         </th>
         <th class="postseason pm" data-sort="playoffs-pm">
          +/-
         </th>
        </tr>
       </thead>
       <tbody>
        <tr class="team-continent-NA">
         <td class="season sorted">
          2014-15
         </td>
         <td class="team">
          <i>
           <img src="images/flag.png"/>
          </i>
          <span class="txt-blue">
           <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2014-2015?tab=stats">
            MIT (Mass. Inst. of Tech.)
           </a>
          </span>
         </td>
         <td class="league">
          <a href="https://www.eliteprospects.com/league/acha-ii/stats/2014-2015">
           ACHA II
          </a>
         </td>
         <td class="regular gp">
          17
         </td>
         <td class="regular g">
          3
         </td>
         <td class="regular a">
          9
         </td>
         <td class="regular tp">
          12
         </td>
         <td class="regular pim">
          20
         </td>
         <td class="regular pm">
         </td>
         <td class="separator">
          |
         </td>
         <td class="postseason">
          <a href="https://www.eliteprospects.com/league/acha-ii/stats/2014-2015">
          </a>
         </td>
         <td class="postseason gp">
         </td>
         <td class="postseason g">
         </td>
         <td class="postseason a">
         </td>
         <td class="postseason tp">
         </td>
         <td class="postseason pim">
         </td>
         <td class="postseason pm">
         </td>
        </tr>
        <tr class="team-continent-NA">
         <td class="season sorted">
          2015-16
         </td>
         <td class="team">
          <i>
           <img src="images/flag.png"/>
          </i>
          <span class="txt-blue">
           <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2015-2016?tab=stats">
            MIT (Mass. Inst. of Tech.)
           </a>
          </span>
         </td>
         <td class="league">
          <a href="https://www.eliteprospects.com/league/acha-ii/stats/2015-2016">
           ACHA II
          </a>
         </td>
         <td class="regular gp">
          9
         </td>
         <td class="regular g">
          1
         </td>
         <td class="regular a">
          1
         </td>
         <td class="regular tp">
          2
         </td>
         <td class="regular pim">
          2
         </td>
         <td class="regular pm">
         </td>
         <td class="separator">
          |
         </td>
         <td class="postseason">
          <a href="https://www.eliteprospects.com/league/acha-ii/stats/2015-2016">
          </a>
         </td>
         <td class="postseason gp">
         </td>
         <td class="postseason g">
         </td>
         <td class="postseason a">
         </td>
         <td class="postseason tp">
         </td>
         <td class="postseason pim">
         </td>
         <td class="postseason pm">
         </td>
        </tr>
        <tr class="team-continent-NA">
         <td class="season sorted">
          2016-17
         </td>
         <td class="team">
          <i>
           <img src="images/flag.png"/>
          </i>
          <span class="txt-blue">
           <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2016-2017?tab=stats">
            MIT (Mass. Inst. of Tech.)
           </a>
          </span>
         </td>
         <td class="league">
          <a href="https://www.eliteprospects.com/league/acha-ii/stats/2016-2017">
           ACHA II
          </a>
         </td>
         <td class="regular gp">
          12
         </td>
         <td class="regular g">
          5
         </td>
         <td class="regular a">
          5
         </td>
         <td class="regular tp">
          10
         </td>
         <td class="regular pim">
          8
         </td>
         <td class="regular pm">
          0
         </td>
         <td class="separator">
          |
         </td>
         <td class="postseason">
         </td>
         <td class="postseason gp">
         </td>
         <td class="postseason g">
         </td>
         <td class="postseason a">
         </td>
         <td class="postseason tp">
         </td>
         <td class="postseason pim">
         </td>
         <td class="postseason pm">
         </td>
        </tr>
        <tr class="team-continent-EU">
         <td class="season sorted">
          2017-18
         </td>
         <td class="team">
          Did not play
         </td>
         <td class="league">
          <a href="https://www.eliteprospects.com/stats">
          </a>
         </td>
         <td class="regular gp">
         </td>
         <td class="regular g">
         </td>
         <td class="regular a">
         </td>
         <td class="regular tp">
         </td>
         <td class="regular pim">
         </td>
         <td class="regular pm">
         </td>
         <td class="separator">
          |
         </td>
         <td class="postseason">
          <a href="https://www.eliteprospects.com/stats">
          </a>
         </td>
         <td class="postseason gp">
         </td>
         <td class="postseason g">
         </td>
         <td class="postseason a">
         </td>
         <td class="postseason tp">
         </td>
         <td class="postseason pim">
         </td>
         <td class="postseason pm">
         </td>
        </tr>
        <tr class="team-continent-NA">
         <td class="season sorted">
          2018-19
         </td>
         <td class="team">
          <i>
           <img src="images/flag.png"/>
          </i>
          <span class="txt-blue">
           <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2018-2019?tab=stats">
            MIT (Mass. Inst. of Tech.)
           </a>
          </span>
         </td>
         <td class="league">
          <a href="https://www.eliteprospects.com/league/acha-iii/stats/2018-2019">
           ACHA III
          </a>
         </td>
         <td class="regular gp">
          8
         </td>
         <td class="regular g">
          5
         </td>
         <td class="regular a">
          10
         </td>
         <td class="regular tp">
          15
         </td>
         <td class="regular pim">
          8
         </td>
         <td class="regular pm">
         </td>
         <td class="separator">
          |
         </td>
         <td class="postseason">
          <a href="https://www.eliteprospects.com/league/acha-iii/stats/2018-2019">
          </a>
         </td>
         <td class="postseason gp">
         </td>
         <td class="postseason g">
         </td>
         <td class="postseason a">
         </td>
         <td class="postseason tp">
         </td>
         <td class="postseason pim">
         </td>
         <td class="postseason pm">
         </td>
        </tr>
       </tbody>
      </table>
      <h2>
       Mystery Message Challenge!
      </h2>
      <p>
       If you scrape the links below grabbing the &lt;p&gt; tag with id="secret-word", you'll discover a secret message :)
      </p>
      <div width="50%">
       <div align="left" class="block">
        <ul>
         <li>
          <a href="challenge/file_1.html">
           File 1
          </a>
         </li>
         <li>
          <a href="challenge/file_2.html">
           File 2
          </a>
         </li>
         <li>
          <a href="challenge/file_3.html">
           File 3
          </a>
         </li>
         <li>
          <a href="challenge/file_4.html">
           File 4
          </a>
         </li>
         <li>
          <a href="challenge/file_5.html">
           File 5
          </a>
         </li>
        </ul>
       </div>
       <div align="center" class="block">
        <ul>
         <li>
          <a href="challenge/file_6.html">
           File 6
          </a>
         </li>
         <li>
          <a href="challenge/file_7.html">
           File 7
          </a>
         </li>
         <li>
          <a href="challenge/file_8.html">
           File 8
          </a>
         </li>
         <li>
          <a href="challenge/file_9.html">
           File 9
          </a>
         </li>
         <li>
          <a href="challenge/file_10.html">
           File 10
          </a>
         </li>
        </ul>
       </div>
      </div>
      <h2>
       Footnotes
      </h2>
      <p id="footer">
       1. This was actually a minivan that I named Debora. Maybe not my dream car, but I loved her nonetheless.
      </p>
     </body>
    </html>
    

## Grab All the social links from the website (3 diff ways)


```python
obj1.find_all("a") #But all this is not the proper answer we are getting a lot of stuff which are also not links..
#After inspecting the website, we find that they are all in a specific type of class
#in an unordered list called socials..

```




    [<a href="https://www.youtube.com/kgmit">youtube.com/kgmit</a>,
     <a href="#footer"><sup>1</sup></a>,
     <a href="https://www.instagram.com/keithgalli/">https://www.instagram.com/keithgalli/</a>,
     <a href="https://twitter.com/keithgalli">https://twitter.com/keithgalli</a>,
     <a href="https://www.linkedin.com/in/keithgalli/">https://www.linkedin.com/in/keithgalli/</a>,
     <a href="https://www.tiktok.com/@keithgalli">https://www.tiktok.com/@keithgalli</a>,
     <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2014-2015?tab=stats"> MIT (Mass. Inst. of Tech.) </a>,
     <a href="https://www.eliteprospects.com/league/acha-ii/stats/2014-2015"> ACHA II </a>,
     <a href="https://www.eliteprospects.com/league/acha-ii/stats/2014-2015"> </a>,
     <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2015-2016?tab=stats"> MIT (Mass. Inst. of Tech.) </a>,
     <a href="https://www.eliteprospects.com/league/acha-ii/stats/2015-2016"> ACHA II </a>,
     <a href="https://www.eliteprospects.com/league/acha-ii/stats/2015-2016"> </a>,
     <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2016-2017?tab=stats"> MIT (Mass. Inst. of Tech.) </a>,
     <a href="https://www.eliteprospects.com/league/acha-ii/stats/2016-2017"> ACHA II </a>,
     <a href="https://www.eliteprospects.com/stats"> </a>,
     <a href="https://www.eliteprospects.com/stats"> </a>,
     <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2018-2019?tab=stats"> MIT (Mass. Inst. of Tech.) </a>,
     <a href="https://www.eliteprospects.com/league/acha-iii/stats/2018-2019"> ACHA III </a>,
     <a href="https://www.eliteprospects.com/league/acha-iii/stats/2018-2019"> </a>,
     <a href="challenge/file_1.html">File 1</a>,
     <a href="challenge/file_2.html">File 2</a>,
     <a href="challenge/file_3.html">File 3</a>,
     <a href="challenge/file_4.html">File 4</a>,
     <a href="challenge/file_5.html">File 5</a>,
     <a href="challenge/file_6.html">File 6</a>,
     <a href="challenge/file_7.html">File 7</a>,
     <a href="challenge/file_8.html">File 8</a>,
     <a href="challenge/file_9.html">File 9</a>,
     <a href="challenge/file_10.html">File 10</a>]




```python
#THE FIRST METHOD
links=obj1.select("ul.socials a")
act_link=[link['href']for link in links]
print(act_link)
```

    ['https://www.instagram.com/keithgalli/', 'https://twitter.com/keithgalli', 'https://www.linkedin.com/in/keithgalli/', 'https://www.tiktok.com/@keithgalli']
    


```python
#THE SECOND METHOD

temp=obj1.find("ul",attrs={"class":"socials"}) #We have to use the attrs- dictionary.
outlinks=temp.find_all("a")
# outlinks['href'] doing this is wrong as (list indices must be integers or slices, not str) we are getting this type of error.

#So like previous stuff that we did we must do a list comprehension.

act_links=[link['href']for link in outlinks]
print(act_links)
```

    ['https://www.instagram.com/keithgalli/', 'https://twitter.com/keithgalli', 'https://www.linkedin.com/in/keithgalli/', 'https://www.tiktok.com/@keithgalli']
    


```python
#THE THIRD METHOD
#Looking into the links we see that they are stored in an li and have the class as social

obj1.select("li.social a")

```




    [<a href="https://www.instagram.com/keithgalli/">https://www.instagram.com/keithgalli/</a>,
     <a href="https://twitter.com/keithgalli">https://twitter.com/keithgalli</a>,
     <a href="https://www.linkedin.com/in/keithgalli/">https://www.linkedin.com/in/keithgalli/</a>,
     <a href="https://www.tiktok.com/@keithgalli">https://www.tiktok.com/@keithgalli</a>]



## Scrape an HTML table into Pandas data frame.


```python
#Will inspect the table at the website.
#My table has the class of hockey stats

table=obj1.select("table.hockey-stats")[0]
table #I don't want that list-ele thing, just show me the main thing with the tags, and as we know the table list will have only 1 element so this works.

#Now we need two things..
#Table column's name
#Table row's data

```




    [<tr class="team-continent-NA">
     <td class="season sorted">
                       2014-15
                   </td>
     <td class="team">
     <i><img src="images/flag.png"/></i>
     <span class="txt-blue">
     <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2014-2015?tab=stats"> MIT (Mass. Inst. of Tech.) </a>
     </span>
     </td>
     <td class="league"> <a href="https://www.eliteprospects.com/league/acha-ii/stats/2014-2015"> ACHA II </a> </td>
     <td class="regular gp">17</td>
     <td class="regular g">3</td>
     <td class="regular a">9</td>
     <td class="regular tp">12</td>
     <td class="regular pim">20</td>
     <td class="regular pm"></td>
     <td class="separator"> | </td>
     <td class="postseason">
     <a href="https://www.eliteprospects.com/league/acha-ii/stats/2014-2015"> </a>
     </td>
     <td class="postseason gp">
     </td>
     <td class="postseason g">
     </td>
     <td class="postseason a">
     </td>
     <td class="postseason tp">
     </td>
     <td class="postseason pim">
     </td>
     <td class="postseason pm">
     </td>
     </tr>,
     <tr class="team-continent-NA">
     <td class="season sorted">
                       2015-16
                   </td>
     <td class="team">
     <i><img src="images/flag.png"/></i>
     <span class="txt-blue">
     <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2015-2016?tab=stats"> MIT (Mass. Inst. of Tech.) </a>
     </span>
     </td>
     <td class="league"> <a href="https://www.eliteprospects.com/league/acha-ii/stats/2015-2016"> ACHA II </a> </td>
     <td class="regular gp">9</td>
     <td class="regular g">1</td>
     <td class="regular a">1</td>
     <td class="regular tp">2</td>
     <td class="regular pim">2</td>
     <td class="regular pm"></td>
     <td class="separator"> | </td>
     <td class="postseason">
     <a href="https://www.eliteprospects.com/league/acha-ii/stats/2015-2016"> </a>
     </td>
     <td class="postseason gp">
     </td>
     <td class="postseason g">
     </td>
     <td class="postseason a">
     </td>
     <td class="postseason tp">
     </td>
     <td class="postseason pim">
     </td>
     <td class="postseason pm">
     </td>
     </tr>,
     <tr class="team-continent-NA">
     <td class="season sorted">
                       2016-17
                   </td>
     <td class="team">
     <i><img src="images/flag.png"/></i>
     <span class="txt-blue">
     <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2016-2017?tab=stats"> MIT (Mass. Inst. of Tech.) </a>
     </span>
     </td>
     <td class="league"> <a href="https://www.eliteprospects.com/league/acha-ii/stats/2016-2017"> ACHA II </a> </td>
     <td class="regular gp">12</td>
     <td class="regular g">5</td>
     <td class="regular a">5</td>
     <td class="regular tp">10</td>
     <td class="regular pim">8</td>
     <td class="regular pm">0</td>
     <td class="separator"> | </td>
     <td class="postseason">
     </td>
     <td class="postseason gp">
     </td>
     <td class="postseason g">
     </td>
     <td class="postseason a">
     </td>
     <td class="postseason tp">
     </td>
     <td class="postseason pim">
     </td>
     <td class="postseason pm">
     </td>
     </tr>,
     <tr class="team-continent-EU">
     <td class="season sorted">
                       2017-18
                   </td>
     <td class="team">
                       Did not play
                   </td>
     <td class="league"> <a href="https://www.eliteprospects.com/stats"> </a> </td>
     <td class="regular gp"></td>
     <td class="regular g"></td>
     <td class="regular a"></td>
     <td class="regular tp"></td>
     <td class="regular pim"></td>
     <td class="regular pm"></td>
     <td class="separator"> | </td>
     <td class="postseason">
     <a href="https://www.eliteprospects.com/stats"> </a>
     </td>
     <td class="postseason gp">
     </td>
     <td class="postseason g">
     </td>
     <td class="postseason a">
     </td>
     <td class="postseason tp">
     </td>
     <td class="postseason pim">
     </td>
     <td class="postseason pm">
     </td>
     </tr>,
     <tr class="team-continent-NA">
     <td class="season sorted">
                       2018-19
                   </td>
     <td class="team">
     <i><img src="images/flag.png"/></i>
     <span class="txt-blue">
     <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2018-2019?tab=stats"> MIT (Mass. Inst. of Tech.) </a>
     </span>
     </td>
     <td class="league"> <a href="https://www.eliteprospects.com/league/acha-iii/stats/2018-2019"> ACHA III </a> </td>
     <td class="regular gp">8</td>
     <td class="regular g">5</td>
     <td class="regular a">10</td>
     <td class="regular tp">15</td>
     <td class="regular pim">8</td>
     <td class="regular pm"></td>
     <td class="separator"> | </td>
     <td class="postseason">
     <a href="https://www.eliteprospects.com/league/acha-iii/stats/2018-2019"> </a>
     </td>
     <td class="postseason gp">
     </td>
     <td class="postseason g">
     </td>
     <td class="postseason a">
     </td>
     <td class="postseason tp">
     </td>
     <td class="postseason pim">
     </td>
     <td class="postseason pm">
     </td>
     </tr>]




```python

#For table column's name

colname=table.find("thead").find_all("th") #Here thead is actually the tag for table head...
colname

#SO, RIGHT NOW WE HAVE ALL OF OUR COLUMN NAMES BUT THEY HAVE ALL TAGS AROUND IT, LET'S GET THE STRING PART


coln=[c.string for c in colname]
coln #Now, we have all of our col names in a proper list.
```




    ['S',
     'Team',
     'League',
     'GP',
     'G',
     'A',
     'TP',
     'PIM',
     '+/-',
     '\xa0',
     'POST',
     'GP',
     'G',
     'A',
     'TP',
     'PIM',
     '+/-']




```python
#Similarly for row name
rowdata=table.find("tbody").find_all("tr")
rowdata
```




    [<tr class="team-continent-NA">
     <td class="season sorted">
                       2014-15
                   </td>
     <td class="team">
     <i><img src="images/flag.png"/></i>
     <span class="txt-blue">
     <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2014-2015?tab=stats"> MIT (Mass. Inst. of Tech.) </a>
     </span>
     </td>
     <td class="league"> <a href="https://www.eliteprospects.com/league/acha-ii/stats/2014-2015"> ACHA II </a> </td>
     <td class="regular gp">17</td>
     <td class="regular g">3</td>
     <td class="regular a">9</td>
     <td class="regular tp">12</td>
     <td class="regular pim">20</td>
     <td class="regular pm"></td>
     <td class="separator"> | </td>
     <td class="postseason">
     <a href="https://www.eliteprospects.com/league/acha-ii/stats/2014-2015"> </a>
     </td>
     <td class="postseason gp">
     </td>
     <td class="postseason g">
     </td>
     <td class="postseason a">
     </td>
     <td class="postseason tp">
     </td>
     <td class="postseason pim">
     </td>
     <td class="postseason pm">
     </td>
     </tr>,
     <tr class="team-continent-NA">
     <td class="season sorted">
                       2015-16
                   </td>
     <td class="team">
     <i><img src="images/flag.png"/></i>
     <span class="txt-blue">
     <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2015-2016?tab=stats"> MIT (Mass. Inst. of Tech.) </a>
     </span>
     </td>
     <td class="league"> <a href="https://www.eliteprospects.com/league/acha-ii/stats/2015-2016"> ACHA II </a> </td>
     <td class="regular gp">9</td>
     <td class="regular g">1</td>
     <td class="regular a">1</td>
     <td class="regular tp">2</td>
     <td class="regular pim">2</td>
     <td class="regular pm"></td>
     <td class="separator"> | </td>
     <td class="postseason">
     <a href="https://www.eliteprospects.com/league/acha-ii/stats/2015-2016"> </a>
     </td>
     <td class="postseason gp">
     </td>
     <td class="postseason g">
     </td>
     <td class="postseason a">
     </td>
     <td class="postseason tp">
     </td>
     <td class="postseason pim">
     </td>
     <td class="postseason pm">
     </td>
     </tr>,
     <tr class="team-continent-NA">
     <td class="season sorted">
                       2016-17
                   </td>
     <td class="team">
     <i><img src="images/flag.png"/></i>
     <span class="txt-blue">
     <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2016-2017?tab=stats"> MIT (Mass. Inst. of Tech.) </a>
     </span>
     </td>
     <td class="league"> <a href="https://www.eliteprospects.com/league/acha-ii/stats/2016-2017"> ACHA II </a> </td>
     <td class="regular gp">12</td>
     <td class="regular g">5</td>
     <td class="regular a">5</td>
     <td class="regular tp">10</td>
     <td class="regular pim">8</td>
     <td class="regular pm">0</td>
     <td class="separator"> | </td>
     <td class="postseason">
     </td>
     <td class="postseason gp">
     </td>
     <td class="postseason g">
     </td>
     <td class="postseason a">
     </td>
     <td class="postseason tp">
     </td>
     <td class="postseason pim">
     </td>
     <td class="postseason pm">
     </td>
     </tr>,
     <tr class="team-continent-EU">
     <td class="season sorted">
                       2017-18
                   </td>
     <td class="team">
                       Did not play
                   </td>
     <td class="league"> <a href="https://www.eliteprospects.com/stats"> </a> </td>
     <td class="regular gp"></td>
     <td class="regular g"></td>
     <td class="regular a"></td>
     <td class="regular tp"></td>
     <td class="regular pim"></td>
     <td class="regular pm"></td>
     <td class="separator"> | </td>
     <td class="postseason">
     <a href="https://www.eliteprospects.com/stats"> </a>
     </td>
     <td class="postseason gp">
     </td>
     <td class="postseason g">
     </td>
     <td class="postseason a">
     </td>
     <td class="postseason tp">
     </td>
     <td class="postseason pim">
     </td>
     <td class="postseason pm">
     </td>
     </tr>,
     <tr class="team-continent-NA">
     <td class="season sorted">
                       2018-19
                   </td>
     <td class="team">
     <i><img src="images/flag.png"/></i>
     <span class="txt-blue">
     <a href="https://www.eliteprospects.com/team/10263/mit-mass.-inst.-of-tech./2018-2019?tab=stats"> MIT (Mass. Inst. of Tech.) </a>
     </span>
     </td>
     <td class="league"> <a href="https://www.eliteprospects.com/league/acha-iii/stats/2018-2019"> ACHA III </a> </td>
     <td class="regular gp">8</td>
     <td class="regular g">5</td>
     <td class="regular a">10</td>
     <td class="regular tp">15</td>
     <td class="regular pim">8</td>
     <td class="regular pm"></td>
     <td class="separator"> | </td>
     <td class="postseason">
     <a href="https://www.eliteprospects.com/league/acha-iii/stats/2018-2019"> </a>
     </td>
     <td class="postseason gp">
     </td>
     <td class="postseason g">
     </td>
     <td class="postseason a">
     </td>
     <td class="postseason tp">
     </td>
     <td class="postseason pim">
     </td>
     <td class="postseason pm">
     </td>
     </tr>]




```python
#SO WE HAVE OUR TABLE COLUMN NAME AND ROW VALUES..

#Now we have to put the required stuff in the list and then put it in the data frame using pandas.

l=[]

for tr in rowdata:
    td=tr.find_all("td") #We are going through each element of rowdata to find the table data denoted by "td" tag.
    
    row=[str(tr.get_text()).strip() for tr in td] #so we are trying to get the text part so tr.string but we also don't want any white space, so we are doing str(tr.get_text()).strip for tr in td...
    l.append(row)
    
l#Yup we have something here

#Call the panda

import pandas as pd

df=pd.DataFrame(l,columns=coln)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>S</th>
      <th>Team</th>
      <th>League</th>
      <th>GP</th>
      <th>G</th>
      <th>A</th>
      <th>TP</th>
      <th>PIM</th>
      <th>+/-</th>
      <th></th>
      <th>POST</th>
      <th>GP</th>
      <th>G</th>
      <th>A</th>
      <th>TP</th>
      <th>PIM</th>
      <th>+/-</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2014-15</td>
      <td>MIT (Mass. Inst. of Tech.)</td>
      <td>ACHA II</td>
      <td>17</td>
      <td>3</td>
      <td>9</td>
      <td>12</td>
      <td>20</td>
      <td></td>
      <td>|</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015-16</td>
      <td>MIT (Mass. Inst. of Tech.)</td>
      <td>ACHA II</td>
      <td>9</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td></td>
      <td>|</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-17</td>
      <td>MIT (Mass. Inst. of Tech.)</td>
      <td>ACHA II</td>
      <td>12</td>
      <td>5</td>
      <td>5</td>
      <td>10</td>
      <td>8</td>
      <td>0</td>
      <td>|</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>2017-18</td>
      <td>Did not play</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>|</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-19</td>
      <td>MIT (Mass. Inst. of Tech.)</td>
      <td>ACHA III</td>
      <td>8</td>
      <td>5</td>
      <td>10</td>
      <td>15</td>
      <td>8</td>
      <td></td>
      <td>|</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



## Grab all the fun facts which contain "is" string in it


```python
#I found all the fun facts were kept in a ul with the class of "fun-facts"
import re

ff=obj1.select("ul.fun-facts")[0]
print(ff)


# li=ff.find_all("li")
# li

# funf=[ele.get_text() for ele in li]
# funf

temp=ff.find_all(string=re.compile("is")) #When I was doing ff.find_all("li",string=re.compile("is")) I was only getting 2 sentence output instead of the actual one... 
# cause some li tags also had i tags in it too (italic) so I guess that was affecting it...
temp


# li.find_all()
```

    <ul class="fun-facts">
    <li>Owned my dream car in high school <a href="#footer"><sup>1</sup></a></li>
    <li>Middle name is Ronald</li>
    <li>Never had been on a plane until college</li>
    <li>Dunkin Donuts coffee is better than Starbucks</li>
    <li>A favorite book series of mine is <i>Ender's Game</i></li>
    <li>Current video game of choice is <i>Rocket League</i></li>
    <li>The band that I've seen the most times live is the <i>Zac Brown Band</i></li>
    </ul>
    




    ['Middle name is Ronald',
     'Dunkin Donuts coffee is better than Starbucks',
     'A favorite book series of mine is ',
     'Current video game of choice is ',
     "The band that I've seen the most times live is the "]




```python
#But the above list also has none values and we want to remove that, so another list comprehension

ans=[facts for facts in temp if facts] #Ele for ele in list if ele(meaning if true then only put ele in the list)
ans

```




    ['Middle name is Ronald',
     'Dunkin Donuts coffee is better than Starbucks',
     'A favorite book series of mine is ',
     'Current video game of choice is ',
     "The band that I've seen the most times live is the "]




```python
#BUT THE SENTENCE IS NOT COMPLETE THAT IS BACAUSE SOME PARTS HAVE ITALIC TAGS IN IT AND HENCE GET STRIPED OF.. SO, WE NEED THE PARENT ELEMENT.
final=[facts.find_parent() for facts in temp if facts] #Ele for ele in list if ele(meaning if true then only put ele in the list)
final
```




    [<li>Middle name is Ronald</li>,
     <li>Dunkin Donuts coffee is better than Starbucks</li>,
     <li>A favorite book series of mine is <i>Ender's Game</i></li>,
     <li>Current video game of choice is <i>Rocket League</i></li>,
     <li>The band that I've seen the most times live is the <i>Zac Brown Band</i></li>]



## Use beautiful soup to help download an image from a webpage


```python
#So the photos are located in div class ="row"


gallery=obj1.select("div.row")[0]
print(gallery)


source=gallery.find_all("img")
source
```

    <div class="row">
    <div class="column">
    <img alt="Lake Como" src="images/italy/lake_como.jpg" style="height:100%"/>
    </div>
    <div class="column">
    <img alt="Pontevecchio, Florence" src="images/italy/pontevecchio.jpg" style="height:100%"/>
    </div>
    <div class="column">
    <img alt="Riomaggiore, Cinque de Terre" src="images/italy/riomaggiore.jpg" style="height:100%"/>
    </div>
    </div>
    




    [<img alt="Lake Como" src="images/italy/lake_como.jpg" style="height:100%"/>,
     <img alt="Pontevecchio, Florence" src="images/italy/pontevecchio.jpg" style="height:100%"/>,
     <img alt="Riomaggiore, Cinque de Terre" src="images/italy/riomaggiore.jpg" style="height:100%"/>]



![image.png](attachment:image.png)

## Yep this is what they have done..  they went somewhere else, and their file got downloaded..


```python
#The main part was that we had to get the src, which turns out to be local path.. so we can just add it to the main url and then be able to download it..
#https://keithgalli.github.io/web-scraping/(+ that src path) + "webpage.html"
```

# Solve the mystery challenge


```python
secret=obj1.select("div.block a")
secret
```




    [<a href="challenge/file_1.html">File 1</a>,
     <a href="challenge/file_2.html">File 2</a>,
     <a href="challenge/file_3.html">File 3</a>,
     <a href="challenge/file_4.html">File 4</a>,
     <a href="challenge/file_5.html">File 5</a>,
     <a href="challenge/file_6.html">File 6</a>,
     <a href="challenge/file_7.html">File 7</a>,
     <a href="challenge/file_8.html">File 8</a>,
     <a href="challenge/file_9.html">File 9</a>,
     <a href="challenge/file_10.html">File 10</a>]




```python
#We need to open those files now, and then in there there will be paragraph



a=[ele['href']for ele in secret]
print(a)


#Now we need to add this local url to our main one

url="https://keithgalli.github.io/web-scraping/"
for source in a:
    link=url+source
    
    page=requests.get(link) #We combine the main url with the local path and try to get that web page
    content=bs(page.content)#We are converting that content into something for beautiful soup...
    ans=content.find("p",attrs={"id":"secret-word"}) #Now we will select the specific attributes and tag
    
    reveal=ans.string#Get the string part..
    print(reveal)
    
    
```

    ['challenge/file_1.html', 'challenge/file_2.html', 'challenge/file_3.html', 'challenge/file_4.html', 'challenge/file_5.html', 'challenge/file_6.html', 'challenge/file_7.html', 'challenge/file_8.html', 'challenge/file_9.html', 'challenge/file_10.html']
    Make
    sure
    to
    smash
    that
    like
    button
    and
    subscribe
    !!!
    

## Done


```python
#https://art-nail-studio-nail-salon.business.site/?utm_source=gmb&utm_medium=referral
```


```python
nail=requests.get("https://art-nail-studio-nail-salon.business.site/?utm_source=gmb&utm_medium=referral")
obj2=bs(nail.content)
print(obj2)
```

    <!DOCTYPE html>
    <html dir="ltr" itemscope="" itemtype="https://schema.org/LocalBusiness" lang="en-US"><head><base href="https://art-nail-studio-nail-salon.business.site/"/><meta content="origin" name="referrer"/><script data-id="_gd" nonce="hb4DHlYxmkpun5_gQ-nwRQ">window.WIZ_global_data = {"DpimGf":false,"E5zAXe":"https://workspace.google.com","EP1ykd":["/_/*","/posts/l/:listingId","/website/_/*","/website/demo","/website/demo/","/website/demo/*"],"FdrFJe":"6378962708678138721","Im6cmf":"/_/GeoMerchantPrestoSiteUi","LVIXXb":1,"LoQv7e":true,"MT7f9b":[],"MuJWjd":false,"NpZeBb":"%.@.3]","QrtxK":"","S06Grb":"","T44sLd":"","TmEjHd":0,"USwLPe":"https://business.google.com","W7VZyd":"","YYbdPc":"LEGACY_URL_PRESTO_NAME","Yllh3e":"%.@.1675778148926955,178652617,151370739]","aMvRme":"NO_CLICK_ID","cfb2h":"boq_geomerchantprestoserver_20230205.08_p0","eNnkwf":"","eptZe":"/_/GeoMerchantPrestoSiteUi/","fPDxwd":[1763433,1772879,45814370,47977019,48410021,48504704,48577232],"gGcLoe":false,"huG48":false,"iSyyv":[37,42,41],"itAxi":"https://business.google.com","kOzZQ":"https://ads.google.com/localservices","nQyAE":{"EPs7mc":"true","nn8wqe":"false","sH6IRc":"false","ItKUvc":"true","toTC0d":"true","NRSeob":"true","aPDse":"false","fKfWJb":"true","u9L1lf":"false","ocCrNd":"false","n4nYUe":"true","visWib":"false","sdhVsb":"false","nDqp1c":"false","DvPMDc":"false","bowC1e":"false","KB6yl":"false","Zouwyc":"false","LzNN5b":"true","i1GOPc":"20","r6hcne":"true","l9NlMd":"true"},"o5oPrb":"","qwAQke":"GeoMerchantPrestoSiteUi","qyaodc":false,"qymVe":"","rtQCxc":-330,"rvOlFd":"PAGE_SOURCE_UNKNOWN","tHwb2":false,"v9NS6b":"27176233722894735","vVkaEb":"","vXmutd":"%.@.\"IN\",\"ZZ\",\"ck+mkg\\u003d\\u003d\"]","w2btAe":"%.@.null,null,\"\",true,null,null,null,false]","zChJod":"%.@.]"};</script><script nonce="hb4DHlYxmkpun5_gQ-nwRQ">(function(){'use strict';var a=window,d=a.performance,l=k();a.cc_latency_start_time=d&&d.now?0:d&&d.timing&&d.timing.navigationStart?d.timing.navigationStart:l;function k(){return d&&d.now?d.now():(new Date).getTime()}function n(e){if(d&&d.now&&d.mark){var g=d.mark(e);if(g)return g.startTime;if(d.getEntriesByName&&(e=d.getEntriesByName(e).pop()))return e.startTime}return k()}a.onaft=function(){n("aft")};a._isLazyImage=function(e){return e.hasAttribute("data-src")||e.hasAttribute("data-ils")||"lazy"===e.getAttribute("loading")};
    a.l=function(e){function g(b){var c={};c[b]=k();a.cc_latency.push(c)}function m(b){var c=n("iml");b.setAttribute("data-iml",c);return c}a.cc_aid=e;a.iml_start=a.cc_latency_start_time;a.css_size=0;a.cc_latency=[];a.ccTick=g;a.onJsLoad=function(){g("jsl")};a.onCssLoad=function(){g("cssl")};a._isVisible=function(b,c){if(!c||"none"==c.style.display)return!1;var f=b.defaultView;if(f&&f.getComputedStyle&&(f=f.getComputedStyle(c),"0px"==f.height||"0px"==f.width||"hidden"==f.visibility))return!1;if(!c.getBoundingClientRect)return!0;
    var h=c.getBoundingClientRect();c=h.left+a.pageXOffset;f=h.top+a.pageYOffset;if(0>f+h.height||0>c+h.width||0>=h.height||0>=h.width)return!1;b=b.documentElement;return f<=(a.innerHeight||b.clientHeight)&&c<=(a.innerWidth||b.clientWidth)};a._recordImlEl=m;document.documentElement.addEventListener("load",function(b){b=b.target;var c;"IMG"!=b.tagName||b.hasAttribute("data-iid")||a._isLazyImage(b)||b.hasAttribute("data-noaft")||(c=m(b));if(a.aft_counter&&(b=a.aft_counter.indexOf(b),-1!==b&&(b=1===a.aft_counter.splice(b,
    1).length,0===a.aft_counter.length&&b&&c)))a.onaft(c)},!0);a.prt=-1;a.wiz_tick=function(){var b=n("prt");a.prt=b}};}).call(this);
    l('Zab0Ef')</script><script nonce="hb4DHlYxmkpun5_gQ-nwRQ">var _F_cssRowKey = 'boq-geo.GeoMerchantPrestoSiteUi.SS9xSG0tdCo.L.W.O';var _F_combinedSignature = 'AD4das3lOIDKfQNVBYRa_EdBgLJvMzZNCA';function _DumpException(e) {throw e;}</script><style data-href="https://www.gstatic.com/_/mss/boq-geo/_/ss/k=boq-geo.GeoMerchantPrestoSiteUi.SS9xSG0tdCo.L.W.O/am=6AwCAiAACAE/d=1/ed=1/rs=AD4das2IMxcWoee8-2DFdOohkv5EN17LXw/m=siteview,_b,_tp,_r" nonce="UT3CglvuxGZyJ2uBcXWJNQ">html{height:100%;overflow:hidden}body{height:100%;overflow:hidden;-webkit-font-smoothing:antialiased;color:#202124;font-family:Roboto,RobotoDraft,Helvetica,Arial,sans-serif;margin:0;-webkit-text-size-adjust:100%;-webkit-text-size-adjust:100%;text-size-adjust:100%}textarea{font-family:Roboto,RobotoDraft,Helvetica,Arial,sans-serif}a{text-decoration:none;color:#1967d2}img{border:none}*{-webkit-tap-highlight-color:transparent}#apps-debug-tracers{display:none}.oYxtQd,.PDllHc,.PDvGL{cursor:pointer}.oYxtQd:hover,.PDllHc:hover,.PDvGL:hover{text-decoration:none}.y2Yetb{cursor:default}.b0t70b{margin-left:auto;margin-right:auto;margin-bottom:40px;max-width:1080px;width:100%}.Igsabe{font-size:1em;margin:0 0 48px;text-align:center;text-transform:uppercase}.WUdCTb{border:1px solid black;margin:0 auto 16px;width:50px}.PDvGL{border-radius:2px;color:#fff;background-color:#696969;display:inline-block;text-transform:uppercase;padding:12px 20px;white-space:nowrap}.aPrNKd{text-align:center}.PDllHc{border-bottom:1px solid;font-weight:800;padding-bottom:8px;text-transform:uppercase}.s5wX1{margin-top:50px;padding:10% 15%}.Ukmiib{display:inline-block}.ZYIfFd{display:none}@media (min-width:640px){.PDvGL{padding:18px 54px}}.xH4iKf .T4LgNb{padding-top:70px;z-index:auto}@media (min-width:640px){.xH4iKf .T4LgNb{padding-top:64px}}.IMnvVe{display:block;height:2px;margin-bottom:3px;-webkit-transition:opacity .25s,-webkit-transform .25s;transition:opacity .25s,-webkit-transform .25s;transition:transform .25s,opacity .25s;transition:transform .25s,opacity .25s,-webkit-transform .25s;width:18px}.SVpPcd,.ihSjwf{-webkit-transform:none;transform:none}.y5Bz3{opacity:1}[dir] .w7WIGb .okoOGc{-webkit-transform:none;transform:none;opacity:1;pointer-events:auto}.w7WIGb .SVpPcd{-webkit-transform:translateY(-5px) rotate(-45deg);transform:translateY(-5px) rotate(-45deg)}.w7WIGb .y5Bz3{opacity:0}.w7WIGb .ihSjwf{-webkit-transform:translateY(5px) rotate(45deg);transform:translateY(5px) rotate(45deg)}.DPvwYc{font-family:"Material Icons Extended";font-weight:normal;font-style:normal;font-size:24px;line-height:1;letter-spacing:normal;text-rendering:optimizeLegibility;text-transform:none;display:inline-block;word-wrap:normal;direction:ltr;font-feature-settings:"liga" 1;-webkit-font-smoothing:antialiased}html[dir="rtl"] .sm8sCf{-webkit-transform:scaleX(-1);-webkit-transform:scaleX(-1);transform:scaleX(-1);filter:FlipH}.Ds6Zp{font-family:"Karla",sans-serif;font-weight:400}.Ds6Zp .Igsabe{font-family:"Vollkorn",serif;font-weight:700}.Ds6Zp .oYxtQd,.Ds6Zp .PDllHc{color:#60a5a5}.Ds6Zp .oYxtQd:hover,.Ds6Zp .PDllHc:hover{color:#667a7a}.Ds6Zp .IMnvVe,.Ds6Zp .PDvGL,.Ds6Zp .m5LbZb{background-color:#60a5a5}.Ds6Zp .PDvGL:hover{background-color:#667a7a}.Ds6Zp .stskkb{font-family:"Vollkorn",serif}.Ds6Zp .PDllHc{border-color:#60a5a5}.Ds6Zp .CoIOBe{color:#60a5a5;font-family:"Vollkorn",serif}.Ds6Zp .DxI8nd{background-color:#667a7a}.kFwPee{height:100%}.ydMMEb{width:100%}.SSPGKf{display:block;overflow-y:hidden;z-index:1}.eejsDc{overflow-y:auto;-webkit-overflow-scrolling:touch}.MCcOAc{bottom:0;left:0;position:absolute;right:0;top:0;overflow:hidden;z-index:1}.MCcOAc>.pGxpHc{-webkit-flex-shrink:0;flex-shrink:0;-webkit-box-flex:0;box-flex:0;-webkit-flex-grow:0;flex-grow:0}.IqBfM>.HLlAHb{-webkit-align-items:center;align-items:center;display:-webkit-box;display:-webkit-flex;display:flex;height:60px;position:absolute;right:16px;top:0;z-index:9999}.VUoKZ{display:none;position:absolute;top:0;left:0;right:0;height:3px;z-index:1001}.TRHLAc{position:absolute;top:0;left:0;width:25%;height:100%;background:#68e;-webkit-transform:scaleX(0);transform:scaleX(0);-webkit-transform-origin:0 0;transform-origin:0 0}.mIM26c .VUoKZ{display:block}.mIM26c .TRHLAc{-webkit-animation:boqChromeapiPageProgressAnimation 1s infinite;animation:boqChromeapiPageProgressAnimation 1s infinite;-webkit-animation-timing-function:cubic-bezier(0.4,0.0,1,1);animation-timing-function:cubic-bezier(0.4,0.0,1,1);-webkit-animation-delay:.1s;animation-delay:.1s}.ghyPEc .VUoKZ{position:fixed}@keyframes boqChromeapiPageProgressAnimation{0%{-webkit-transform:scaleX(0);transform:scaleX(0)}50%{-webkit-transform:scaleX(5);transform:scaleX(5)}to{-webkit-transform:scaleX(5) translateX(100%);transform:scaleX(5) translateX(100%)}}@-webkit-keyframes boqChromeapiPageProgressAnimation{0%{-webkit-transform:scaleX(0);transform:scaleX(0)}50%{-webkit-transform:scaleX(5);transform:scaleX(5)}to{-webkit-transform:scaleX(5) translateX(100%);transform:scaleX(5) translateX(100%)}}.QfEmjf{text-align:center}.OE6k1{display:none}.s9D8xb{display:inline-block}.T9hLVe{color:black;display:inline-block;line-height:2em;padding-left:10px;vertical-align:bottom}.tG0UAe{margin-top:20px}.zACorf{background:white;border-bottom:2px solid rgba(0,0,0,.1);bottom:56px;box-shadow:0 -4px 4px rgba(0,0,0,.1);font:400 14px/20px Roboto,Arial,sans-serif;height:72px;left:0;position:fixed;width:100%;z-index:200}.PHn8Rd{padding:6px 16px 0 0}.PHn8Rd,.PTmgAc{color:#757575;display:block}.ZUpe3e{cursor:pointer;font-size:24px}.s6myae,.rnMavc{display:-webkit-box;display:-webkit-flex;display:flex}.s6myae{-webkit-box-pack:justify;-webkit-justify-content:space-between;justify-content:space-between;margin-top:18px}.PTmgAc{font:500 14px/20px "Google Sans",Roboto,Arial,sans-serif;line-height:32px}.rnMavc{-webkit-box-orient:vertical;-webkit-box-direction:normal;-webkit-flex-direction:column;flex-direction:column;padding:0 20px}@media (min-width:640px){.zACorf{bottom:0}}.eOemif{display:-webkit-box;display:-webkit-flex;display:flex;-webkit-flex-basis:100%;flex-basis:100%;-webkit-box-orient:vertical;-webkit-box-direction:normal;-webkit-flex-direction:column;flex-direction:column;font-size:12px;line-height:17px;overflow:hidden;padding-top:6px;text-align:center}.eOemif .xPRkMe{font-size:24px;margin-bottom:2px}.dtKbfb{-webkit-box-align:center;-webkit-align-items:center;align-items:center;display:none;-webkit-box-flex:0;-webkit-flex-grow:0;flex-grow:0;-webkit-flex-shrink:0;flex-shrink:0;font-size:17px;padding:0 14px;text-transform:capitalize;white-space:nowrap}.dtKbfb .xPRkMe{font-size:17px;padding:0 12px}.hfmNEe{-webkit-box-align:center;-webkit-align-items:center;align-items:center;background-color:rgba(0,0,0,.5);bottom:0;display:none;-webkit-box-pack:center;-webkit-justify-content:center;justify-content:center;left:0;position:fixed;right:0;top:0;z-index:100}.tzdnMe{background:white;display:-webkit-box;display:-webkit-flex;display:flex;-webkit-box-orient:vertical;-webkit-box-direction:normal;-webkit-flex-direction:column;flex-direction:column;height:100%;position:relative;width:100%}.lud4Oc{-webkit-box-flex:1;-webkit-flex:1;flex:1;width:100%}.AhXpDe{-webkit-align-content:center;align-content:center;background:rgb(66,133,244);height:56px;text-shadow:none;width:100%}.CLFymb{-webkit-box-align:center;-webkit-align-items:center;align-items:center;color:white;display:-webkit-box;display:-webkit-flex;display:flex;font-weight:500;line-height:56px;padding-left:72px;position:absolute;top:0}.vxXi8b{-webkit-box-align:center;-webkit-align-items:center;align-items:center;color:white;cursor:pointer;display:-webkit-box;display:-webkit-flex;display:flex;left:0;line-height:56px;padding-left:24px;position:absolute}.jairkd{background-color:#323232;bottom:0;color:white;font-size:14px;left:0;margin:auto;min-width:288px;padding:14px 24px;position:fixed;text-align:left;right:0;visibility:hidden;width:100%;z-index:110}@media (min-width:640px){.dtKbfb{display:-webkit-box;display:-webkit-flex;display:flex}.tzdnMe{height:70%;margin:10% auto;max-height:580px;width:500px}.AhXpDe{background:white}.CLFymb{color:black;padding-left:24px}.vxXi8b{color:gray;left:auto;padding-right:24px;right:0}.jairkd{bottom:24px;max-width:568px}}.ZmaiX{background:white;box-shadow:0 0 5px grey;height:100%;max-width:80%;position:fixed;-webkit-transition:opacity .25s,-webkit-transform .25s;transition:opacity .25s,-webkit-transform .25s;transition:transform .25s,opacity .25s;transition:transform .25s,opacity .25s,-webkit-transform .25s;width:400px;z-index:45;top:70px}.ZmaiX{left:0;right:auto;-webkit-transform:translateX(-105%);transform:translateX(-105%)}.sAhjff{box-sizing:border-box;font-size:20px;padding-top:40px;width:100%;list-style:none}.sAhjff{text-align:left;padding-left:57px}.sAhjff .oYxtQd{padding:15px}.sWUWpe{box-sizing:border-box;margin:0;padding:0;width:100%}@media (min-width:640px){.ZmaiX{top:64px}.sAhjff{padding-left:119px}}.c2zzSe{display:inline-block;-webkit-box-flex:1;-webkit-flex-grow:1;flex-grow:1;font-size:18px;line-height:normal;min-width:22px;overflow:hidden;padding:0 12px;text-overflow:ellipsis;white-space:nowrap}.cfSov{display:inline-block;-webkit-box-flex:0;-webkit-flex-grow:0;flex-grow:0;-webkit-flex-shrink:0;flex-shrink:0;font-size:18px;padding:15px;text-align:left;-webkit-user-select:none;user-select:none}.MI9wmf{cursor:pointer}.CS81U{box-shadow:0 0 5px grey;position:fixed;z-index:50}.CS81U,.IeYwod{-webkit-box-align:center;-webkit-align-items:center;align-items:center;background:white;box-sizing:border-box;display:-webkit-box;display:-webkit-flex;display:flex;height:70px;left:0;padding:0 12px;top:0;width:100%}.IeYwod{box-shadow:0 5px 5px -5px grey;pointer-events:none;position:absolute}@media (min-width:640px){.c2zzSe{font-size:22px;padding:0 20px}.cfSov{font-size:23px}.CS81U,.IeYwod{height:64px;-webkit-box-pack:justify;-webkit-justify-content:space-between;justify-content:space-between;padding:0 66px}}.jY7uzd{-webkit-box-align:center;-webkit-align-items:center;align-items:center;box-sizing:border-box;display:-webkit-box;display:-webkit-flex;display:flex;-webkit-box-orient:vertical;-webkit-box-direction:normal;-webkit-flex-direction:column;flex-direction:column;line-height:1.5em;min-height:100%;padding:0 0 56px;position:relative}@media (min-width:480px){.jY7uzd{overflow-x:hidden}}@media (min-width:640px){.jY7uzd{padding:0 80px}}.UQyKne{background-color:white;bottom:0;left:0;box-shadow:0 0 5px grey;display:-webkit-box;display:-webkit-flex;display:flex;height:56px;position:fixed;width:100%}@media (min-width:640px){.UQyKne{display:none}}.T4LgNb{bottom:0;left:0;top:0;right:0;position:absolute;z-index:1}.QMEh5b{position:absolute;top:0;left:0;right:0;z-index:3}.AOq4tb{height:56px}.kFwPee{position:relative;z-index:1;height:100%}.ydMMEb{height:56px;width:100%}.SSPGKf{overflow-y:hidden;position:absolute;bottom:0;left:0;right:0;top:0}.ecJEib .AOq4tb,.ecJEib .ydMMEb{height:64px}.e2G3Fb.EWZcud .AOq4tb,.e2G3Fb.EWZcud .ydMMEb{height:48px}.e2G3Fb.b30Rkd .AOq4tb,.e2G3Fb.b30Rkd .ydMMEb{height:56px}c-wiz{contain:style}c-wiz>c-data{display:none}c-wiz.rETSD{contain:none}c-wiz.Ubi8Z{contain:layout style}@-webkit-keyframes quantumWizBoxInkSpread{0%{-webkit-transform:translate(-50%,-50%) scale(0.2);-webkit-transform:translate(-50%,-50%) scale(0.2);transform:translate(-50%,-50%) scale(0.2)}to{-webkit-transform:translate(-50%,-50%) scale(2.2);-webkit-transform:translate(-50%,-50%) scale(2.2);transform:translate(-50%,-50%) scale(2.2)}}@keyframes quantumWizBoxInkSpread{0%{-webkit-transform:translate(-50%,-50%) scale(0.2);-webkit-transform:translate(-50%,-50%) scale(0.2);transform:translate(-50%,-50%) scale(0.2)}to{-webkit-transform:translate(-50%,-50%) scale(2.2);-webkit-transform:translate(-50%,-50%) scale(2.2);transform:translate(-50%,-50%) scale(2.2)}}@-webkit-keyframes quantumWizIconFocusPulse{0%{-webkit-transform:translate(-50%,-50%) scale(1.5);-webkit-transform:translate(-50%,-50%) scale(1.5);transform:translate(-50%,-50%) scale(1.5);opacity:0}to{-webkit-transform:translate(-50%,-50%) scale(2);-webkit-transform:translate(-50%,-50%) scale(2);transform:translate(-50%,-50%) scale(2);opacity:1}}@keyframes quantumWizIconFocusPulse{0%{-webkit-transform:translate(-50%,-50%) scale(1.5);-webkit-transform:translate(-50%,-50%) scale(1.5);transform:translate(-50%,-50%) scale(1.5);opacity:0}to{-webkit-transform:translate(-50%,-50%) scale(2);-webkit-transform:translate(-50%,-50%) scale(2);transform:translate(-50%,-50%) scale(2);opacity:1}}@-webkit-keyframes quantumWizRadialInkSpread{0%{-webkit-transform:scale(1.5);-webkit-transform:scale(1.5);transform:scale(1.5);opacity:0}to{-webkit-transform:scale(2.5);-webkit-transform:scale(2.5);transform:scale(2.5);opacity:1}}@keyframes quantumWizRadialInkSpread{0%{-webkit-transform:scale(1.5);-webkit-transform:scale(1.5);transform:scale(1.5);opacity:0}to{-webkit-transform:scale(2.5);-webkit-transform:scale(2.5);transform:scale(2.5);opacity:1}}@-webkit-keyframes quantumWizRadialInkFocusPulse{0%{-webkit-transform:scale(2);-webkit-transform:scale(2);transform:scale(2);opacity:0}to{-webkit-transform:scale(2.5);-webkit-transform:scale(2.5);transform:scale(2.5);opacity:1}}@keyframes quantumWizRadialInkFocusPulse{0%{-webkit-transform:scale(2);-webkit-transform:scale(2);transform:scale(2);opacity:0}to{-webkit-transform:scale(2.5);-webkit-transform:scale(2.5);transform:scale(2.5);opacity:1}}.O0WRkf{-webkit-user-select:none;-webkit-transition:background .2s .1s;transition:background .2s .1s;border:0;-webkit-border-radius:3px;border-radius:3px;cursor:pointer;display:inline-block;font-size:14px;font-weight:500;min-width:4em;outline:none;overflow:hidden;position:relative;text-align:center;text-transform:uppercase;-webkit-tap-highlight-color:transparent;z-index:0}.A9jyad{font-size:13px;line-height:16px}.zZhnYe{-webkit-transition:box-shadow .28s cubic-bezier(0.4,0,0.2,1);transition:box-shadow .28s cubic-bezier(0.4,0,0.2,1);background:#dfdfdf;-webkit-box-shadow:0px 2px 2px 0px rgba(0,0,0,.14),0px 3px 1px -2px rgba(0,0,0,.12),0px 1px 5px 0px rgba(0,0,0,.2);box-shadow:0px 2px 2px 0px rgba(0,0,0,.14),0px 3px 1px -2px rgba(0,0,0,.12),0px 1px 5px 0px rgba(0,0,0,.2)}.zZhnYe.qs41qe{-webkit-transition:box-shadow .28s cubic-bezier(0.4,0,0.2,1);transition:box-shadow .28s cubic-bezier(0.4,0,0.2,1);-webkit-transition:background .8s;transition:background .8s;-webkit-box-shadow:0px 8px 10px 1px rgba(0,0,0,.14),0px 3px 14px 2px rgba(0,0,0,.12),0px 5px 5px -3px rgba(0,0,0,.2);box-shadow:0px 8px 10px 1px rgba(0,0,0,.14),0px 3px 14px 2px rgba(0,0,0,.12),0px 5px 5px -3px rgba(0,0,0,.2)}.e3Duub,.e3Duub a,.e3Duub a:hover,.e3Duub a:link,.e3Duub a:visited{background:#1a73e8;color:#fff}.HQ8yf,.HQ8yf a{color:#1a73e8}.UxubU,.UxubU a{color:#fff}.ZFr60d{position:absolute;top:0;right:0;bottom:0;left:0;background-color:transparent}.O0WRkf.u3bW4e .ZFr60d{background-color:rgba(0,0,0,.12)}.UxubU.u3bW4e .ZFr60d{background-color:rgba(255,255,255,0.239)}.e3Duub.u3bW4e .ZFr60d{background-color:rgba(255,255,255,0.239)}.HQ8yf.u3bW4e .ZFr60d{background-color:rgba(26,115,232,0.122)}.Vwe4Vb{-webkit-transform:translate(-50%,-50%) scale(0);transform:translate(-50%,-50%) scale(0);-webkit-transition:opacity .2s ease,visibility 0s ease .2s,-webkit-transform 0s ease .2s;-webkit-transition:opacity .2s ease,visibility 0s ease .2s,transform 0s ease .2s;transition:opacity .2s ease,visibility 0s ease .2s,transform 0s ease .2s;-webkit-transition:opacity .2s ease,visibility 0s ease .2s,transform 0s ease .2s,-webkit-transform 0s ease .2s;transition:opacity .2s ease,visibility 0s ease .2s,transform 0s ease .2s,-webkit-transform 0s ease .2s;-webkit-transition:opacity .2s ease,visibility 0s ease .2s,-webkit-transform 0s ease .2s;transition:opacity .2s ease,visibility 0s ease .2s,-webkit-transform 0s ease .2s;-webkit-background-size:cover;background-size:cover;left:0;opacity:0;pointer-events:none;position:absolute;top:0;visibility:hidden}.O0WRkf.qs41qe .Vwe4Vb{-webkit-transform:translate(-50%,-50%) scale(2.2);transform:translate(-50%,-50%) scale(2.2);opacity:1;visibility:visible}.O0WRkf.qs41qe.M9Bg4d .Vwe4Vb{transition:-webkit-transform 0.3s cubic-bezier(0,0,0.2,1),opacity .2s cubic-bezier(0,0,0.2,1);-webkit-transition:opacity .2s cubic-bezier(0,0,0.2,1),-webkit-transform 0.3s cubic-bezier(0,0,0.2,1);-webkit-transition:opacity .2s cubic-bezier(0,0,0.2,1),-webkit-transform 0.3s cubic-bezier(0,0,0.2,1);transition:opacity .2s cubic-bezier(0,0,0.2,1),-webkit-transform 0.3s cubic-bezier(0,0,0.2,1);-webkit-transition:transform 0.3s cubic-bezier(0,0,0.2,1),opacity .2s cubic-bezier(0,0,0.2,1);transition:transform 0.3s cubic-bezier(0,0,0.2,1),opacity .2s cubic-bezier(0,0,0.2,1);-webkit-transition:transform 0.3s cubic-bezier(0,0,0.2,1),opacity .2s cubic-bezier(0,0,0.2,1),-webkit-transform 0.3s cubic-bezier(0,0,0.2,1);transition:transform 0.3s cubic-bezier(0,0,0.2,1),opacity .2s cubic-bezier(0,0,0.2,1),-webkit-transform 0.3s cubic-bezier(0,0,0.2,1)}.O0WRkf.j7nIZb .Vwe4Vb{-webkit-transform:translate(-50%,-50%) scale(2.2);transform:translate(-50%,-50%) scale(2.2);visibility:visible}.oG5Srb .Vwe4Vb,.zZhnYe .Vwe4Vb{background-image:radial-gradient(circle farthest-side,rgba(0,0,0,.12),rgba(0,0,0,.12) 80%,rgba(0,0,0,0) 100%)}.HQ8yf .Vwe4Vb{background-image:radial-gradient(circle farthest-side,rgba(26,115,232,0.161),rgba(26,115,232,0.161) 80%,rgba(26,115,232,0) 100%)}.e3Duub .Vwe4Vb{background-image:radial-gradient(circle farthest-side,rgba(255,255,255,0.322),rgba(255,255,255,0.322) 80%,rgba(255,255,255,0) 100%)}.UxubU .Vwe4Vb{background-image:radial-gradient(circle farthest-side,rgba(255,255,255,0.322),rgba(255,255,255,0.322) 80%,rgba(255,255,255,0) 100%)}.O0WRkf.RDPZE{-webkit-box-shadow:none;box-shadow:none;color:rgba(68,68,68,0.502);cursor:default;fill:rgba(68,68,68,0.502)}.zZhnYe.RDPZE{background:rgba(153,153,153,.1)}.UxubU.RDPZE{color:rgba(255,255,255,0.502);fill:rgba(255,255,255,0.502)}.UxubU.zZhnYe.RDPZE{background:rgba(204,204,204,.1)}.CwaK9{position:relative}.RveJvd{display:inline-block;margin:.5em}.FKF6mc,.FKF6mc:focus{display:block;outline:none;text-decoration:none}.FKF6mc:visited{fill:inherit;stroke:inherit}.U26fgb.u3bW4e{outline:1px solid transparent}.JRtysb{-webkit-user-select:none;-webkit-transition:background .3s;transition:background .3s;border:0;-webkit-border-radius:50%;border-radius:50%;color:#444;cursor:pointer;display:inline-block;fill:#444;-webkit-flex-shrink:0;-webkit-flex-shrink:0;flex-shrink:0;height:48px;outline:none;overflow:hidden;position:relative;text-align:center;-webkit-tap-highlight-color:transparent;width:48px;z-index:0}.JRtysb.u3bW4e,.JRtysb.qs41qe,.JRtysb.j7nIZb{-webkit-transform:translateZ(0);-webkit-mask-image:-webkit-radial-gradient(circle,white 100%,black 100%)}.JRtysb.RDPZE{cursor:default}.ZDSs1{color:rgba(255,255,255,.75);fill:rgba(255,255,255,.75)}.WzwrXb.u3bW4e{background-color:rgba(153,153,153,.4)}.ZDSs1.u3bW4e{background-color:rgba(204,204,204,.25)}.NWlf3e{-webkit-transform:translate(-50%,-50%) scale(0);transform:translate(-50%,-50%) scale(0);-webkit-transition:opacity .2s ease;transition:opacity .2s ease;-webkit-background-size:cover;background-size:cover;left:0;opacity:0;pointer-events:none;position:absolute;top:0;visibility:hidden}.JRtysb.iWO5td>.NWlf3e{transition:-webkit-transform 0.3s cubic-bezier(0,0,0.2,1);-webkit-transition:-webkit-transform 0.3s cubic-bezier(0,0,0.2,1);-webkit-transition:transform 0.3s cubic-bezier(0,0,0.2,1);transition:transform 0.3s cubic-bezier(0,0,0.2,1);-webkit-transition:transform 0.3s cubic-bezier(0,0,0.2,1),-webkit-transform 0.3s cubic-bezier(0,0,0.2,1);transition:transform 0.3s cubic-bezier(0,0,0.2,1),-webkit-transform 0.3s cubic-bezier(0,0,0.2,1);-webkit-transform:translate(-50%,-50%) scale(2.2);transform:translate(-50%,-50%) scale(2.2);opacity:1;visibility:visible}.JRtysb.j7nIZb>.NWlf3e{-webkit-transform:translate(-50%,-50%) scale(2.2);transform:translate(-50%,-50%) scale(2.2);visibility:visible}.WzwrXb.iWO5td>.NWlf3e{background-image:radial-gradient(circle farthest-side,rgba(153,153,153,.4),rgba(153,153,153,.4) 80%,rgba(153,153,153,0) 100%)}.ZDSs1.iWO5td>.NWlf3e{background-image:radial-gradient(circle farthest-side,rgba(204,204,204,.25),rgba(204,204,204,.25) 80%,rgba(204,204,204,0) 100%)}.WzwrXb.RDPZE{color:rgba(68,68,68,0.502);fill:rgba(68,68,68,0.502)}.ZDSs1.RDPZE{color:rgba(255,255,255,0.502);fill:rgba(255,255,255,0.502)}.MhXXcc{line-height:44px;position:relative}.Lw7GHd{margin:8px;display:inline-block}.JPdR6b{-webkit-transform:translateZ(0);transform:translateZ(0);-webkit-transition:max-width 0.2s cubic-bezier(0,0,0.2,1),max-height 0.2s cubic-bezier(0,0,0.2,1),opacity 0.1s linear;transition:max-width 0.2s cubic-bezier(0,0,0.2,1),max-height 0.2s cubic-bezier(0,0,0.2,1),opacity 0.1s linear;background:#fff;border:0;-webkit-border-radius:2px;border-radius:2px;-webkit-box-shadow:0px 8px 10px 1px rgba(0,0,0,.14),0px 3px 14px 2px rgba(0,0,0,.12),0px 5px 5px -3px rgba(0,0,0,.2);box-shadow:0px 8px 10px 1px rgba(0,0,0,.14),0px 3px 14px 2px rgba(0,0,0,.12),0px 5px 5px -3px rgba(0,0,0,.2);-webkit-box-sizing:border-box;box-sizing:border-box;max-height:100%;max-width:100%;opacity:1;outline:1px solid transparent;z-index:2000}.XvhY1d{overflow-x:hidden;overflow-y:auto;-webkit-overflow-scrolling:touch}.JAPqpe{float:left;padding:16px 0}.JPdR6b.qjTEB{-webkit-transition:left 0.2s cubic-bezier(0,0,0.2,1),max-width 0.2s cubic-bezier(0,0,0.2,1),max-height 0.2s cubic-bezier(0,0,0.2,1),opacity 0.05s linear,top 0.2s cubic-bezier(0,0,0.2,1);transition:left 0.2s cubic-bezier(0,0,0.2,1),max-width 0.2s cubic-bezier(0,0,0.2,1),max-height 0.2s cubic-bezier(0,0,0.2,1),opacity 0.05s linear,top 0.2s cubic-bezier(0,0,0.2,1)}.JPdR6b.jVwmLb{max-height:56px;opacity:0}.JPdR6b.CAwICe{overflow:hidden}.JPdR6b.oXxKqf{-webkit-transition:none;transition:none}.z80M1{color:#222;cursor:pointer;display:block;outline:none;overflow:hidden;padding:0 24px;position:relative}.uyYuVb{display:-webkit-box;display:-webkit-flex;display:flex;font-size:14px;font-weight:400;line-height:40px;height:40px;position:relative;white-space:nowrap}.jO7h3c{-webkit-box-flex:1;box-flex:1;-webkit-flex-grow:1;flex-grow:1;min-width:0}.JPdR6b.e5Emjc .z80M1{padding-left:64px}.JPdR6b.CblTmf .z80M1{padding-right:48px}.PCdOIb{display:-webkit-box;display:-webkit-flex;display:flex;-webkit-flex-direction:column;flex-direction:column;-webkit-justify-content:center;justify-content:center;background-repeat:no-repeat;height:40px;left:24px;opacity:0.54;position:absolute}.z80M1.RDPZE .PCdOIb{opacity:0.26}.z80M1.FwR7Pc{outline:1px solid transparent;background-color:#eee}.z80M1.RDPZE{color:#b8b8b8;cursor:default}.z80M1.N2RpBe::before{-webkit-transform:rotate(45deg);transform:rotate(45deg);-webkit-transform-origin:left;transform-origin:left;content:"\0000a0";display:block;border-right:2px solid #222;border-bottom:2px solid #222;height:16px;left:24px;opacity:0.54;position:absolute;top:13%;width:7px;z-index:0}.JPdR6b.CblTmf .z80M1.N2RpBe::before{left:auto;right:16px}.z80M1.RDPZE::before{border-color:#b8b8b8;opacity:1}.aBBjbd{pointer-events:none;position:absolute}.z80M1.qs41qe>.aBBjbd{-webkit-animation:quantumWizBoxInkSpread 0.3s ease-out;animation:quantumWizBoxInkSpread 0.3s ease-out;-webkit-animation-fill-mode:forwards;animation-fill-mode:forwards;background-image:-webkit-radial-gradient(circle farthest-side,#bdc1c6,#bdc1c6 80%,rgba(189,193,198,0) 100%);background-image:-webkit-radial-gradient(circle farthest-side,#bdc1c6,#bdc1c6 80%,rgba(189,193,198,0) 100%);background-image:radial-gradient(circle farthest-side,#bdc1c6,#bdc1c6 80%,rgba(189,193,198,0) 100%);-webkit-background-size:cover;background-size:cover;opacity:1;top:0;left:0}.J0XlZe{color:inherit;line-height:40px;padding:0 6px 0 1em}.a9caSc{color:inherit;direction:ltr;padding:0 6px 0 1em}.kCtYwe{border-top:1px solid #dadce0;margin:7px 0}.B2l7lc{border-left:1px solid #dadce0;display:inline-block;height:48px}@media screen and (max-width:840px){.JAPqpe{padding:8px 0}.z80M1{padding:0 16px}.JPdR6b.e5Emjc .z80M1{padding-left:48px}.PCdOIb{left:12px}}.C0oVfc{line-height:20px;min-width:88px}.C0oVfc .RveJvd{margin:8px}.dXspqb{font-family:"Material Icons Extended";font-weight:normal;font-style:normal;font-size:24px;line-height:1;letter-spacing:normal;text-rendering:optimizeLegibility;text-transform:none;display:inline-block;word-wrap:normal;direction:ltr;font-feature-settings:"liga" 1;-webkit-font-smoothing:antialiased;vertical-align:middle}.qsBatc{font-family:"Material Icons Extended";font-weight:normal;font-style:normal;font-size:18px;line-height:1;letter-spacing:normal;text-rendering:optimizeLegibility;text-transform:none;display:inline-block;word-wrap:normal;direction:ltr;font-feature-settings:"liga" 1;-webkit-font-smoothing:antialiased;vertical-align:middle}.B580Hc{-webkit-border-radius:4px;border-radius:4px}.B580Hc .snByac{font:500 14px/20px "Google Sans",Roboto,Arial,sans-serif;-webkit-font-smoothing:antialiased;letter-spacing:.56px;text-transform:none;padding:0 8px}.B580Hc.URore{margin-left:-16px}.B580Hc.Ivk9L{margin-right:-16px}.JvxUib{background:#1a73e8}.JvxUib.RDPZE{background:#f1f3f4}.JvxUib .snByac{color:#fff}.JvxUib.RDPZE .snByac{color:#80868b}.XAza6c{border:1px solid #dadce0}.XAza6c .snByac{margin:7px}.XAza6c:not(.RDPZE):hover{background:rgba(26,115,232,0.039);border-color:#d2e3fc}.Rffzxc{font-size:18px;padding-right:8px;width:18px}.WxEVBf{font-size:18px;padding-right:8px;width:18px}.AxsIAb .TpQm9d{text-decoration:none}.AxsIAb .snByac{-webkit-align-items:center;align-items:center;display:-webkit-box;display:-webkit-flex;display:flex}.meqSyc{display:inline-block}.meqSyc:hover{color:#3b78e7;cursor:pointer;text-decoration:underline}.fGZQDc{display:-webkit-box;display:-webkit-flex;display:flex;-webkit-box-orient:vertical;-webkit-box-direction:normal;-webkit-flex-direction:column;flex-direction:column}.kCmrbf{-webkit-box-flex:1;-webkit-flex:1;flex:1;-webkit-flex-basis:auto;flex-basis:auto;margin:0 18px 42px}.q8cvFf,.Gou21b{margin:0 0 16px}.R7Di0e{list-style-type:none;margin:0;padding:0}.Gou21b{font-weight:bold;line-height:1em}.qhkvMe{font-style:normal}.IQ1KEb{margin-bottom:48px}.QMbmRe{background-position:center center;background-repeat:no-repeat;padding-top:56%;width:100%}.x2TOCf{font-weight:400;text-align:inherit}.o0m3Qb{padding:0 .5em}.WF8WNe{white-space:nowrap}@media (min-width:992px){.q8cvFf,.Gou21b{margin:0 0 24px}.fGZQDc{-webkit-box-orient:horizontal;-webkit-box-direction:normal;-webkit-flex-direction:row;flex-direction:row}.kCmrbf{-webkit-flex-basis:0;flex-basis:0}}.KqqKsf{-webkit-box-align:center;-webkit-align-items:center;align-items:center;border-top:.25px solid #b5b5b5;color:#b5b5b5;display:-webkit-box;display:-webkit-flex;display:flex;-webkit-box-orient:vertical;-webkit-box-direction:normal;-webkit-flex-direction:column;flex-direction:column;font-size:12px;padding:20px 0}.X3D5od{color:#b5b5b5}.D76t8b{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAHgAAAAsCAQAAAAEcrS4AAAABGdBTUEAAYagMeiWXwAAAAJiS0dEAP+Hj8y/AAAACXBIWXMAAABIAAAASABGyWs+AAAGkElEQVRo3u3ae5CWVR0H8M+uQBIN22WSW4EmIiQtl2VxQIJCRlFwsAsX0wIat0EDFSdNZJh0mqQAkTBhNAoQnLALEiAOZFCIZrDcVDC5FTVAcgmENOT29gdnD+/77rPv3nTYtb7/PL/zO9/nvOe7z3N+53d+z+alivxPoUGS87Q/WKHUTm85o8AlivTXN5lcz5CX/YSPmWG6vQnUVsb6tgvP94zT8KrPB+tITQU/Y3Si2DK09aQe51tnxCZdgpWq8j3558zT7vDlnHLZobcnzrfOWiEKPuUmj0Z3I4PNtsEe+2wy1xCNIu85Z873rGuBGIdG+1V0lnhQi9hqrpNv2Ot+czHI0+mvRb1DEDzf48HRxAIDE4gtzXG9hZ6Mz7p+Ii9VxEGX+xf4kBV6n+85VRk1DlpTglwerUdya4Z83jEzNHq59XzP531HA5Y6GhoT5NVqsN1K/RMtFPv0e8I8Y7NNDivQTWE1ZnfIn/xdSks9NcsWvCKYLfSrsdQzfmGqDWmeYt8xOGGKVWee8lOT/TW2Cz3iuNvBV02pcC4bPWBp3DrzDDBRx9ibz8Zg9q3xdvOmq92SIYJ1hrrWgRoz9+nt9jS5vKKfmXbbbbeDFcwkZaJuFqdlCilLFZkT2w3YFczPJg7RuUKZ33RHnNqORMbv9PKCizJEVI25Xx/bE+QsreRPP8EPErwnjNTQzWWCjwX3xxOH2Fzh4OvBaYOjiOZG6Ir1ZtsPthnq9+HNyWSO1EXKenMSmGcMi3JbGqHQCavN824lcp+NchsaboACW8zwOihRrB3yUkUXhoGmG5MwSMWBYpBFmKUktIf4mY8E+6iRFgZ7juFkMIealZM539eDZ5hZmgR7m+vtDPZwc2Tvw6e1D3/QVpYpDD3H3eB5MNAS5POJ0LVH9ZBCKgaPPp6KImhqgauCPSmL+QXzK2FODu3e5kW5tLMs5+F0eZDbwNIoN2VdjExLvY4GtA8npPWJw7xQzvPbMPUCbPFG8D6SVR5oaKorwVZvaF8N5navhJ6pWcx2StIOONlYEq7DQtw55ikzvJrGWKmDBvSyEqx2xEfLDdOrnOc34doapXEqXcrxil3sb2Ct9mnMzpUwXw7etspXn76SQ3DZbtMfW800N0YnmhphlA7I54bgPOHnqoJV4dqJeHrukMDLi9491WKWLa2kXSPp7jK8Ga6b9XWFn0S5nTxhrx+He/Mpim/8xJhTV4zNMW73di5pT97BLwjXVLWYZXtoUgUtV1Wt7Bcmx0fSyC1eslFJWiTIJ8/40Djo1koO9ynfDVYfzYhp2/ZE9rZwbV4j5s4E3i4VIzOFbGOif5inR9Y+kw+DYzHsGWNzSE75nuXBPpt0dA2t12JIOoctUUbXDOa2SphlK3dzRp51FrkSj67Rus4SO92XlshkCc4zW0FwTDe4ghf7P0b5frB7+hLorHXw3JN1Jk25N1iX6FQtZicXh/a4LOb+HCGLQdG618C4SGCP0Q6nC+ZSC+L6WOhyUxzKGOy4eT4Xy3cF5oYXJd9dwbfEnU5G/kmjLQv2WHkZzMXucion8+7QftqEtPftkBtzxphrYkj7mq1p/t2u8ZiOnkVGmXaRYWnJW0NX6qy5fAds9aJ/x54Pey6tTHBCt7jXXeE23VBqRvzJztZqWI7Z0W2KpJTG1C+deVJxDI3FRin0rtWmhSSU5EyLlfoFq7FRrnORA5Z7PMbrl/TIrEuvNTTshxWjpUWKMzw79IpbQjaae9FnasTsWe78lI5kwfzQuArvucckWbtEd5vdnbNId7NNWXJpa03aeTMdhdakiagec7XLEniVFaDuMz1x68r3kB8FKwNNPWyX+7Upd0tTI2ww3ycThmtrnUlphV1oaYq1Lq0xs72Nxmua5mnll0YH+wIVYYyNbsySda21xoWok5f89TBlm/V2OOK0Aq0V6hLWV8U47WWl9qCV7rrnmFTVme9YZaPDPqabqzU01whwp2k557LfKn9xVBOX+aJPpfXk1d3PpamEo+l4D4GHYySvLuroR4R9HtSu3IE1ZXGwav6U6qTgcVp7wA4jnMjwz/EaKKjFF8w6KbhLSEye1z9m1CdM861gl9Tic0+dXMMpQ/w62Pm6a+Mtf47JYTNbYpXmAyKYtw3wx8SexlYkFCWqjjr5StPEcmMSonR7a2olt84+4bPY4jHL7AYFrnKToZVmA/Va8Fm87ajGCmr53asM9eA/kZqkFWhqjzq6ht8//F/wBx3/BVeX85NynnDoAAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDE2LTA2LTEwVDEzOjU2OjE1LTA3OjAwm2JgPwAAACV0RVh0ZGF0ZTptb2RpZnkAMjAxNi0wNi0xMFQxMzo1NjoxMS0wNzowMB5w/JAAAAAASUVORK5CYII=);background-position:0 0;background-repeat:no-repeat;background-size:contain;color:transparent;display:inline-block;height:30px;width:60px}@media (min-width:640px){.KqqKsf{-webkit-box-orient:horizontal;-webkit-box-direction:normal;-webkit-flex-direction:row;flex-direction:row;-webkit-box-pack:justify;-webkit-justify-content:space-between;justify-content:space-between}}.pZxRx{display:-webkit-box;display:-webkit-flex;display:flex;-webkit-flex-flow:column;flex-flow:column;margin-bottom:0;text-align:center;width:100%}.xOSAn{margin-top:70px}.BLytye{margin:24px 0;-webkit-box-ordinal-group:2;-webkit-order:2;order:2}.nbVECf{border-top:1px solid #b5b5b5;margin-top:50px;-webkit-box-ordinal-group:3;-webkit-order:3;order:3;width:100%}.BiTRX{background-repeat:no-repeat;-webkit-background-size:cover;background-size:cover;max-width:1080px;-webkit-box-ordinal-group:1;-webkit-order:1;order:1}.hY9UDb{margin:0 0 16px}.nbOMh{font-size:32px;line-height:1.2;word-break:break-word}.n9wygc{display:block;height:auto;width:100%}.teQaN{margin-bottom:24px}.dq8oYd{cursor:pointer}@media (min-width:640px){.BLytye{margin:80px 0;-webkit-box-ordinal-group:1;-webkit-order:1;order:1}.nbOMh{font-size:56px}.pZxRx{margin-bottom:128px}}.xVyTkc{clear:both;display:-webkit-box;display:-webkit-flex;display:flex;-webkit-flex-wrap:wrap;flex-wrap:wrap;-webkit-justify-content:space-between;justify-content:space-between;padding:0 18px}.qVtErf{pointer-events:none}.LzG70e{margin-bottom:60px;width:100%}.K2aYk{margin:0}.fn4FVe .aWQIEc{border:2px solid #dadce0}.fn4FVe .NHc3Rd{font-weight:700;text-align:center}@media (min-width:640px){.LzG70e{width:48%}}@media (min-width:992px){.LzG70e{width:32%}.swW1N .LzG70e{width:100%}.HOmYGf .LzG70e{width:48%}.cpxwuf .LzG70e{width:32%}.F0rq3 .LzG70e{width:24%}.E6T9Td .LzG70e{width:19%}}.UEkdfe,.YZVx3{margin-bottom:24px}.xkrGZb,.czMoo{font-size:1.15em;font-weight:400;margin:0 0 18px;word-wrap:break-word}.fdMcf{margin-bottom:30px;position:relative}.XN6irf{-webkit-transform:translate(-50%,-50%);transform:translate(-50%,-50%);color:white;font-size:80px;left:50%;position:absolute;top:50%}.bGQFnc,.SglfTd{margin-bottom:30px}.PE7lZb{margin:0;overflow:hidden;text-overflow:ellipsis}.KbhWjc{display:inline-block;margin:18px 0 0}.aWQIEc{width:100%}.xDsCs{float:right;margin:0 0 15px;position:relative;z-index:1}.ABNIJd{float:left;display:inline-block;vertical-align:middle}.LUyeUe{float:right;display:inline-block;vertical-align:middle}.xDsCs .O0WRkf{font-family:Roboto,Arial,sans-serif;font-size:14px;font-weight:500}@media (max-width:350px){.xDsCs .O0WRkf{margin-left:0}}.zCbINb{background:url(//www.gstatic.com/bfe/apps/website/img/promo/localpost.png) no-repeat center;-webkit-background-size:contain;background-size:contain;padding-top:70%;width:70%}.yXdavd{border-radius:10px;box-shadow:0 2px 6px 1px rgba(0,0,0,.26);display:-webkit-box;display:-webkit-flex;display:flex;margin:auto;max-width:880px}.luQWad{-webkit-box-align:center;-webkit-align-items:center;align-items:center;display:-webkit-box;display:-webkit-flex;display:flex;-webkit-box-pack:center;-webkit-justify-content:center;justify-content:center;width:50%}.d1Nrrd{display:-webkit-box;display:-webkit-flex;display:flex;-webkit-box-orient:vertical;-webkit-box-direction:normal;-webkit-flex-direction:column;flex-direction:column;-webkit-box-flex:1;-webkit-flex-grow:1;flex-grow:1;padding:40px 24px 24px 24px;width:50%}.z9Qd5b{font:400 24px/32px "Google Sans",Roboto,Arial,sans-serif;font-weight:500;padding-bottom:20px}.eoBkrc{-webkit-box-flex:1;-webkit-flex:1;flex:1;font:400 16px/24px Roboto,Arial,sans-serif;min-height:90px}.cV6wgb{padding-top:24px}.eooj4d{padding:15px;position:absolute;right:0;top:0}.UxOK2e{padding:0 15px;float:right}@media (max-width:750px){.z9Qd5b{padding-bottom:0}.yXdavd{-webkit-box-orient:vertical;-webkit-box-direction:normal;-webkit-flex-direction:column;flex-direction:column;margin:5%}.d1Nrrd{padding:12px 24px 24px 24px}.eoBkrc{padding-bottom:12px;min-height:0}.luQWad{border-bottom:1px solid #dadce0;width:100%}.d1Nrrd{width:auto}}.POb9Qd{position:relative;text-align:center}.uTPl7d,.A8tyw{list-style-type:none;padding:0}.uTPl7d{clear:both;margin:0 5px}.A8tyw{display:-webkit-box;display:-webkit-flex;display:flex;-webkit-flex-wrap:wrap;flex-wrap:wrap;-webkit-box-pack:justify;-webkit-justify-content:space-between;justify-content:space-between}.Ya6MGe{font-weight:bold;margin:0;padding:24px 15px;text-align:center}.eJXTXb{-webkit-flex-basis:100%;flex-basis:100%;margin-bottom:30px}.uKXCDd{font-weight:bold}.uKXCDd,.AixKWd{margin-bottom:15px}@media (min-width:640px){.eJXTXb{-webkit-flex-basis:48%;flex-basis:48%}}@media (min-width:992px){.eJXTXb{-webkit-flex-basis:32%;flex-basis:32%;max-width:300px}}.GMBffe{background:url(//www.gstatic.com/bfe/apps/website/img/promo/menu.png) no-repeat center;background-size:contain;padding-top:70%;width:70%}.y72IR{-webkit-box-align:center;-webkit-align-items:center;align-items:center;display:-webkit-box;display:-webkit-flex;display:flex;-webkit-box-orient:vertical;-webkit-box-direction:normal;-webkit-flex-direction:column;flex-direction:column}.y72IR,.goIW2{padding:0 18px}.PWqJSb{margin:30px 0}.PWqJSb:first-child{position:relative}.kErOtf{margin:0}.d11Ssd{display:block;width:100%}.afsrV .d11Ssd{border:2px solid #dadce0}.qSlMTb{-webkit-box-align:center;-webkit-align-items:center;align-items:center;background:rgba(0,0,0,.25);bottom:0;color:#fff;cursor:pointer;display:-webkit-box;display:-webkit-flex;display:flex;-webkit-box-orient:vertical;-webkit-box-direction:normal;-webkit-flex-direction:column;flex-direction:column;font-size:14px;-webkit-box-pack:center;-webkit-justify-content:center;justify-content:center;left:0;position:absolute;right:0;text-align:center;text-transform:uppercase;top:0}.TPONh{background-color:#3973e0;background-image:url("data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2232%22%20height%3D%2232%22%20viewBox%3D%220%200%2032%2032%22%3E%0A%3Cpath%20fill%3D%22%23fff%22%20d%3D%22M30%2024h-20c-1.1%200-2-0.9-2-2v-20c0-1.1%200.9-2%202-2h20c1.1%200%202%200.9%202%202v20c0%201.1-0.9%202-2%202zM30%203c0-0.6-0.4-1-1-1h-18c-0.6%200-1%200.4-1%201v18c0%200.6%200.4%201%201%201h18c0.6%200%201-0.4%201-1v-18zM2%2011v18c0%200.6%200.4%201%201%201h18c0.6%200%201-0.4%201-1v-3h2v4c0%201.1-0.9%202-2%202h-20c-1.1%200-2-0.9-2-2v-20c0-1.1%200.9-2%202-2h4v2h-3c-0.6%200-1%200.4-1%201z%22%3E%3C/path%3E%0A%3C/svg%3E%0A");background-position:49% 50%;background-repeat:no-repeat;background-size:24px;border-radius:50%;color:#fff;font-size:24px;height:56px;line-height:56px;width:56px}@media (min-width:640px){.goIW2{-webkit-box-align:center;-webkit-align-items:center;align-items:center;-webkit-box-pack:center;-webkit-justify-content:center;justify-content:center;margin:0 auto;max-width:1080px}.UCecQ{display:-webkit-box;display:-webkit-flex;display:flex;-webkit-flex-wrap:wrap;flex-wrap:wrap;-webkit-box-pack:justify;-webkit-justify-content:space-between;justify-content:space-between}.PWqJSb{-webkit-box-flex:0;-webkit-flex:0 0 48%;flex:0 0 48%;margin:5px 0}}@media (min-width:992px){.PWqJSb{-webkit-box-flex:0;-webkit-flex:0 0 32%;flex:0 0 32%}.PWqJSb.yV2J1d{-webkit-box-flex:0;-webkit-flex:0 0 100%;flex:0 0 100%}.PWqJSb.GKgvLd{-webkit-box-flex:0;-webkit-flex:0 0 48%;flex:0 0 48%}.PWqJSb.ZdKHsd{-webkit-box-flex:0;-webkit-flex:0 0 32%;flex:0 0 32%}.PWqJSb.qu5YVb{-webkit-box-flex:0;-webkit-flex:0 0 24%;flex:0 0 24%}.PWqJSb.cisOjf{-webkit-box-flex:0;-webkit-flex:0 0 19%;flex:0 0 19%}}.y8qxgb{background:url(//www.gstatic.com/bfe/apps/website/img/promo/service.png) no-repeat center;background-size:contain;padding-top:70%;width:70%}.C33Nzc{position:relative;text-align:center}.SMsjy,.YXKoQe{list-style-type:none;padding:0}.SMsjy{clear:both;margin:0 5px}.YXKoQe{display:-webkit-box;display:-webkit-flex;display:flex;-webkit-flex-wrap:wrap;flex-wrap:wrap;-webkit-box-pack:justify;-webkit-justify-content:space-between;justify-content:space-between;margin:0 24px}.VC08se{font-weight:bold;margin:0;padding:24px 15px;text-align:center}.T5EIZe{-webkit-flex-basis:100%;flex-basis:100%;margin-bottom:30px}.wnVKb{font-weight:bold}.wnVKb,.zJFju{margin-bottom:15px}@media (min-width:640px){.T5EIZe{-webkit-flex-basis:48%;flex-basis:48%}}@media (min-width:992px){.T5EIZe{-webkit-flex-basis:32%;flex-basis:32%;max-width:300px}}.PTfVdf{background:#f6f6f6}.dlg3Sd{padding:18px}.Epq3vb{margin:0 0 18px}.PTfVdf ul,.PTfVdf ol{list-style-position:outside;margin:auto;-webkit-padding-start:40px}.PTfVdf ul{list-style-type:disc}.PTfVdf ol{list-style-type:decimal}.P3EGec{font-weight:700;text-align:center}@media (min-width:992px){.dlg3Sd{padding:120px}}.TpcqSc{color:rgb(218,220,224);z-index:-1;vertical-align:top;height:20px}.XZleqe{color:rgb(251,188,4);top:0;left:0;position:absolute;height:20px}.plHSId{color:rgb(251,188,4);top:0;overflow:hidden;width:50%;left:0;position:absolute;height:20px}.x5gs9{top:0;left:0;width:0;overflow:hidden;position:absolute}.CoamRb{width:20px;height:20px;vertical-align:text-bottom}.CoamRb .Pmm3mc{width:20px;height:20px}.CoamRb{position:relative;display:inline-block}.FrGbIb,.Dx6vx{background:url(//www.gstatic.com/bfe/images/reviews/empty_state_v2.svg) no-repeat center;background-size:contain;padding-top:70%;width:70%}.EIjale{display:-webkit-box;display:-webkit-flex;display:flex;-webkit-flex-wrap:wrap;flex-wrap:wrap;-webkit-box-pack:justify;-webkit-justify-content:space-between;justify-content:space-between}.zGTRLd{display:-webkit-box;display:-webkit-flex;display:flex;-webkit-box-orient:horizontal;-webkit-box-direction:normal;-webkit-flex-direction:row;flex-direction:row}.P0eG7b{color:gray;-webkit-box-flex:2;-webkit-flex-grow:2;flex-grow:2;margin-left:12px}.iTushb{-webkit-flex-basis:100%;flex-basis:100%;-webkit-box-orient:vertical;-webkit-box-direction:normal;-webkit-flex-direction:column;flex-direction:column;margin:20px 10px 60px}.Sc4eWe{margin:0 0 15px}.dogP7c{margin:0 15px}.YSOJuc{font-style:normal;font-weight:bold}@media (min-width:640px){.iTushb{-webkit-flex-basis:48%;flex-basis:48%;margin:0 0 60px}}@media (min-width:992px){.iTushb{-webkit-flex-basis:32%;flex-basis:32%;max-width:300px}}sentinel{}
    /*# sourceURL=/_/mss/boq-geo/_/ss/k=boq-geo.GeoMerchantPrestoSiteUi.SS9xSG0tdCo.L.W.O/am=6AwCAiAACAE/d=1/ed=1/rs=AD4das2IMxcWoee8-2DFdOohkv5EN17LXw/m=siteview,_b,_tp,_r */</style><script nonce="hb4DHlYxmkpun5_gQ-nwRQ">onCssLoad();</script><style nonce="UT3CglvuxGZyJ2uBcXWJNQ">@font-face{font-family:'Material Icons Extended';font-style:normal;font-weight:400;src:url(//fonts.gstatic.com/s/materialiconsextended/v149/kJEjBvgX7BgnkSrUwT8UnLVc38YydejYY-oE_LvM.ttf)format('truetype');}.material-icons-extended{font-family:'Material Icons Extended';font-weight:normal;font-style:normal;font-size:24px;line-height:1;letter-spacing:normal;text-transform:none;display:inline-block;white-space:nowrap;word-wrap:normal;direction:ltr;}@font-face{font-family:'Product Sans';font-style:normal;font-weight:400;src:url(//fonts.gstatic.com/s/productsans/v9/pxiDypQkot1TnFhsFMOfGShVF9eL.ttf)format('truetype');}</style><script nonce="hb4DHlYxmkpun5_gQ-nwRQ">(function(){/*
    
     Copyright The Closure Library Authors.
     SPDX-License-Identifier: Apache-2.0
    */
    'use strict';function aa(){var b=t,c=0;return function(){return c<b.length?{done:!1,value:b[c++]}:{done:!0}}}var w=this||self;/*
    
     Copyright 2013 Google LLC.
     SPDX-License-Identifier: Apache-2.0
    */
    var x={};function ba(b,c){if(null===c)return!1;if("contains"in b&&1==c.nodeType)return b.contains(c);if("compareDocumentPosition"in b)return b==c||!!(b.compareDocumentPosition(c)&16);for(;c&&b!=c;)c=c.parentNode;return c==b};/*
    
     Copyright 2011 Google LLC.
     SPDX-License-Identifier: Apache-2.0
    */
    function ca(b,c){return function(d){d||(d=window.event);return c.call(b,d)}}function z(b){b=b.target||b.srcElement;!b.getAttribute&&b.parentNode&&(b=b.parentNode);return b}var B="undefined"!=typeof navigator&&/Macintosh/.test(navigator.userAgent),da="undefined"!=typeof navigator&&!/Opera/.test(navigator.userAgent)&&/WebKit/.test(navigator.userAgent),ea={A:1,INPUT:1,TEXTAREA:1,SELECT:1,BUTTON:1};function fa(){this._mouseEventsPrevented=!0}
    var ha={Enter:13," ":32},C={A:13,BUTTON:0,CHECKBOX:32,COMBOBOX:13,FILE:0,GRIDCELL:13,LINK:13,LISTBOX:13,MENU:0,MENUBAR:0,MENUITEM:0,MENUITEMCHECKBOX:0,MENUITEMRADIO:0,OPTION:0,RADIO:32,RADIOGROUP:32,RESET:0,SUBMIT:0,SWITCH:32,TAB:0,TREE:13,TREEITEM:13},E={CHECKBOX:!0,FILE:!0,OPTION:!0,RADIO:!0},F={COLOR:!0,DATE:!0,DATETIME:!0,"DATETIME-LOCAL":!0,EMAIL:!0,MONTH:!0,NUMBER:!0,PASSWORD:!0,RANGE:!0,SEARCH:!0,TEL:!0,TEXT:!0,TEXTAREA:!0,TIME:!0,URL:!0,WEEK:!0},ia={A:!0,AREA:!0,BUTTON:!0,DIALOG:!0,IMG:!0,
    INPUT:!0,LINK:!0,MENU:!0,OPTGROUP:!0,OPTION:!0,PROGRESS:!0,SELECT:!0,TEXTAREA:!0};function ja(b){this.g=b;this.h=[]};var G=w._jsa||{};G._cfc=void 0;G._aeh=void 0;/*
    
     Copyright 2005 Google LLC.
     SPDX-License-Identifier: Apache-2.0
    */
    function L(){this.o=[];this.g=[];this.j=[];this.m={};this.h=null;this.l=[]}function M(b){return String.prototype.trim?b.trim():b.replace(/^\s+/,"").replace(/\s+$/,"")}
    function ka(b,c){return function n(a,k){k=void 0===k?!0:k;var e=c;if("click"==e&&(B&&a.metaKey||!B&&a.ctrlKey||2==a.which||null==a.which&&4==a.button||a.shiftKey))e="clickmod";else{var f=a.which||a.keyCode;!f&&a.key&&(f=ha[a.key]);da&&3==f&&(f=13);if(13!=f&&32!=f)f=!1;else{var g=z(a),h;(h="keydown"!=a.type||!!(!("getAttribute"in g)||(g.getAttribute("type")||g.tagName).toUpperCase()in F||"BUTTON"==g.tagName.toUpperCase()||g.type&&"FILE"==g.type.toUpperCase()||g.isContentEditable)||a.ctrlKey||a.shiftKey||
    a.altKey||a.metaKey||(g.getAttribute("type")||g.tagName).toUpperCase()in E&&32==f)||((h=g.tagName in ea)||(h=g.getAttributeNode("tabindex"),h=null!=h&&h.specified),h=!(h&&!g.disabled));if(h)f=!1;else{h=(g.getAttribute("role")||g.type||g.tagName).toUpperCase();var v=!(h in C)&&13==f;g="INPUT"!=g.tagName.toUpperCase()||!!g.type;f=(0==C[h]%f||v)&&g}}f&&(e="clickkey")}g=a.srcElement||a.target;f=N(e,a,g,"",null);var l,y;for(h=g;h&&h!=this;h=h.__owner||("#document-fragment"!==(null==(l=h.parentNode)?void 0:
    l.nodeName)?h.parentNode:null==(y=h.parentNode)?void 0:y.host)){var m=h;var q=void 0;v=m;var r=e,la=a;var p=v.__jsaction;if(!p){var D;p=null;"getAttribute"in v&&(p=v.getAttribute("jsaction"));if(D=p){p=x[D];if(!p){p={};for(var H=D.split(ma),na=H?H.length:0,I=0;I<na;I++){var A=H[I];if(A){var J=A.indexOf(":"),S=-1!=J;p[S?M(A.substr(0,J)):oa]=S?M(A.substr(J+1)):A}}x[D]=p}v.__jsaction=p}else p=pa,v.__jsaction=p}"maybe_click"==r&&p.click?(q=r,r="click"):"clickkey"==r?r="click":"click"!=r||p.click||(r=
    "clickonly");q=G._cfc&&p.click?G._cfc(v,la,p,r,q):{eventType:q?q:r,action:p[r]||"",event:null,ignore:!1};if(q.ignore||q.action)break}q&&(f=N(q.eventType,q.event||a,g,q.action||"",m,f.timeStamp));f&&"touchend"==f.eventType&&(f.event._preventMouseEvents=fa);if(q&&q.action){if(l="clickkey"==e)l=z(a),l=(l.type||l.tagName).toUpperCase(),(l=32==(a.which||a.keyCode)&&"CHECKBOX"!=l)||(l=z(a),y=l.tagName.toUpperCase(),g=(l.getAttribute("role")||"").toUpperCase(),l="BUTTON"===y||"BUTTON"===g?!0:!(l.tagName.toUpperCase()in
    ia)||"A"===y||"SELECT"===y||(l.getAttribute("type")||l.tagName).toUpperCase()in E||(l.getAttribute("type")||l.tagName).toUpperCase()in F?!1:!0);l&&(a.preventDefault?a.preventDefault():a.returnValue=!1);if("mouseenter"==e||"mouseleave"==e||"pointerenter"==e||"pointerleave"==e)if(l=a.relatedTarget,!("mouseover"==a.type&&"mouseenter"==e||"mouseout"==a.type&&"mouseleave"==e||"pointerover"==a.type&&"pointerenter"==e||"pointerout"==a.type&&"pointerleave"==e)||l&&(l===m||ba(m,l)))f.action="",f.actionElement=
    null;else{e={};for(var u in a)"function"!==typeof a[u]&&"srcElement"!==u&&"target"!==u&&(e[u]=a[u]);e.type="mouseover"==a.type?"mouseenter":"mouseout"==a.type?"mouseleave":"pointerover"==a.type?"pointerenter":"pointerleave";e.target=e.srcElement=m;e.bubbles=!1;f.event=e;f.targetElement=m}}else f.action="",f.actionElement=null;m=f;b.h&&!m.event.a11ysgd&&(u=N(m.eventType,m.event,m.targetElement,m.action,m.actionElement,m.timeStamp),"clickonly"==u.eventType&&(u.eventType="click"),b.h(u,!0));if(m.actionElement){if(b.h){if(!m.actionElement||
    "A"!=m.actionElement.tagName||"click"!=m.eventType&&"clickmod"!=m.eventType||(a.preventDefault?a.preventDefault():a.returnValue=!1),(a=b.h(m))&&k){n.call(this,a,!1);return}}else{if((k=w.document)&&!k.createEvent&&k.createEventObject)try{var K=k.createEventObject(a)}catch(ua){K=a}else K=a;m.event=K;b.l.push(m)}G._aeh&&G._aeh(m)}}}function N(b,c,d,a,k,n){return{eventType:b,event:c,targetElement:d,action:a,actionElement:k,timeStamp:n||Date.now()}}
    function qa(b,c){return function(d){var a=b,k=c,n=!1;"mouseenter"==a?a="mouseover":"mouseleave"==a?a="mouseout":"pointerenter"==a?a="pointerover":"pointerleave"==a&&(a="pointerout");if(d.addEventListener){if("focus"==a||"blur"==a||"error"==a||"load"==a||"toggle"==a)n=!0;d.addEventListener(a,k,n)}else d.attachEvent&&("focus"==a?a="focusin":"blur"==a&&(a="focusout"),k=ca(d,k),d.attachEvent("on"+a,k));return{eventType:a,i:k,capture:n}}}
    function O(b,c,d){if(!b.m.hasOwnProperty(c)){var a=ka(b,c);d=qa(d||c,a);b.m[c]=a;b.o.push(d);for(a=0;a<b.g.length;++a){var k=b.g[a];k.h.push(d.call(null,k.g))}"click"==c&&O(b,"keydown")}}L.prototype.i=function(b){return this.m[b]};
    function ra(b,c){var d=new ja(c);a:{for(var a=0;a<b.g.length;a++)if(P(b.g[a].g,c)){c=!0;break a}c=!1}if(c)return b.j.push(d),d;Q(b,d);b.g.push(d);c=b.j.concat(b.g);a=[];for(var k=[],n=0;n<b.g.length;++n){var e=b.g[n];if(R(e,c)){a.push(e);for(var f=0;f<e.h.length;++f){var g=e.g,h=e.h[f];g.removeEventListener?g.removeEventListener(h.eventType,h.i,h.capture):g.detachEvent&&g.detachEvent("on"+h.eventType,h.i)}e.h=[]}else k.push(e)}for(n=0;n<b.j.length;++n)e=b.j[n],R(e,c)?a.push(e):(k.push(e),Q(b,e));
    b.g=k;b.j=a;return d}function Q(b,c){var d=c.g;sa&&(d.style.cursor="pointer");for(d=0;d<b.o.length;++d)c.h.push(b.o[d].call(null,c.g))}function R(b,c){for(var d=0;d<c.length;++d)if(c[d].g!=b.g&&P(c[d].g,b.g))return!0;return!1}function P(b,c){for(;b!=c&&c.parentNode;)c=c.parentNode;return b==c}var sa="undefined"!=typeof navigator&&/iPhone|iPad|iPod/.test(navigator.userAgent),ma=/\s*;\s*/,oa="click",pa={};var t="click dblclick focus focusin blur error focusout keydown keyup keypress load mouseover mouseout mouseenter mouseleave submit toggle touchstart touchend touchmove touchcancel auxclick change compositionstart compositionupdate compositionend beforeinput input textinput copy cut paste mousedown mouseup wheel contextmenu dragover dragenter dragleave drop dragstart dragend pointerdown pointermove pointerup pointercancel pointerenter pointerleave pointerover pointerout gotpointercapture lostpointercapture ended loadedmetadata pagehide pageshow visibilitychange beforematch".split(" ");
    if(!(t instanceof Array)){var T,U="undefined"!=typeof Symbol&&Symbol.iterator&&t[Symbol.iterator];T=U?U.call(t):{next:aa()};for(var V,ta=[];!(V=T.next()).done;)ta.push(V.value)};var W=function(b){return{trigger:function(c){var d=b.i(c.type);d||(O(b,c.type),d=b.i(c.type));var a=c.target||c.srcElement;d&&d.call(a.ownerDocument.documentElement,c)},bind:function(c){b.h=c;b.l&&(0<b.l.length&&c(b.l),b.l=null)}}}(function(){var b=window,c=new L,d=ra(c,b.document.documentElement);t.forEach(function(n){return O(c,n)});var a,k;"onwebkitanimationend"in b&&(a="webkitAnimationEnd");O(c,"animationend",a);"onwebkittransitionend"in b&&(k="webkitTransitionEnd");O(c,"transitionend",k);return{s:c,
    u:d}}().s),X=["BOQ_wizbind"],Y=window||w;X[0]in Y||"undefined"==typeof Y.execScript||Y.execScript("var "+X[0]);for(var Z;X.length&&(Z=X.shift());)X.length||void 0===W?Y[Z]&&Y[Z]!==Object.prototype[Z]?Y=Y[Z]:Y=Y[Z]={}:Y[Z]=W;}).call(this);
    </script><script defer="" id="base-js" nocollect="" nonce="hb4DHlYxmkpun5_gQ-nwRQ" src="https://www.gstatic.com/_/mss/boq-geo/_/js/k=boq-geo.GeoMerchantPrestoSiteUi.en_US.l-1euQ2W90g.es5.O/am=6AwCAiAACAE/d=1/excm=_b,_r,_tp,siteview/ed=1/dg=0/wt=2/rs=AD4das1QvwEGsqihqVBWk7TRDSrBXnZx2Q/m=_b,_tp,_r"></script><script nonce="hb4DHlYxmkpun5_gQ-nwRQ">if (window.BOQ_loadedInitialJS) {onJsLoad();} else {document.getElementById('base-js').addEventListener('load', onJsLoad, false);}</script><script nonce="hb4DHlYxmkpun5_gQ-nwRQ">
        window['_wjdc'] = function (d) {window['_wjdd'] = d};
        </script><title>Art Nail Studio - Nail Salon</title><link href="https://art-nail-studio-nail-salon.business.site" rel="canonical"/><meta charset="utf-8"/><meta content="width=device-width, initial-scale=1" name="viewport"/><meta content="telephone=no" name="format-detection"/><meta description="Nail Salon"/><meta content="website" property="og:type"/><meta content="https://art-nail-studio-nail-salon.business.site" property="og:url"/><meta content="Art Nail Studio" property="og:title"/><meta content="Nail Salon" property="og:description"/><meta content="https://lh5.googleusercontent.com/p/AF1QipMXD7KvZFKdNW-i9HzbnxcC6dyaOmj-OvP_4OpA" property="og:image"/><meta content="314" property="og:image:width"/><meta content="176" property="og:image:height"/><script nonce="hb4DHlYxmkpun5_gQ-nwRQ">var AF_initDataKeys = ["ds:0","ds:1"]; var AF_dataServiceRequests = {'ds:0' : {id:'KUcOhc',request:["art-nail-studio-nail-salon",null,false,"https://art-nail-studio-nail-salon.business.site/?utm_source\u003dgmb\u0026utm_medium\u003dreferral",true,null,null,[]]},'ds:1' : {id:'k9FGZe',request:["art-nail-studio-nail-salon",null,null,5,3,null,[]]}}; var AF_initDataChunkQueue = []; var AF_initDataCallback; var AF_initDataInitializeCallback; if (AF_initDataInitializeCallback) {AF_initDataInitializeCallback(AF_initDataKeys, AF_initDataChunkQueue, AF_dataServiceRequests);}if (!AF_initDataCallback) {AF_initDataCallback = function(chunk) {AF_initDataChunkQueue.push(chunk);};}</script></head><body data-ih="600" data-iw="800" jsaction="rcuQ6b:npT2md; click:FAbpgf; auxclick:FAbpgf" jscontroller="pjICDe"><script aria-hidden="true" nonce="hb4DHlYxmkpun5_gQ-nwRQ">window.wiz_progress&&window.wiz_progress();</script><div class="MCcOAc IqBfM ecJEib EWZcud" id="yDmH0d"><div aria-hidden="true" class="VUoKZ"><div class="TRHLAc"></div></div><c-wiz c-wiz="" class="SSPGKf xH4iKf" data-node-index="0;0" data-ogpc="" data-p='%.@.null,null,null,null,null,null,null,["art-nail-studio-nail-salon",null,"https://art-nail-studio-nail-salon.business.site/?utm_source\u003dgmb\u0026utm_medium\u003dreferral",false,true,"/",null,false,true],null,null,null,null,null,null,null,null,false,true,null,[1]]' jsdata="deferred-i1" jslog="58867; track:impression" jsmodel="hc6Ubd" jsrenderer="jcTSCb" view=""><div class="T4LgNb eejsDc" jsname="a9kxte"><div class="kFwPee" jsname="qJTHM"><link href="//fonts.googleapis.com/icon?family=Material+Icons+Extended" nonce="UT3CglvuxGZyJ2uBcXWJNQ" rel="stylesheet" type="text/css"/><link href="//fonts.googleapis.com/css?family=Vollkorn:700|Karla:400" nonce="UT3CglvuxGZyJ2uBcXWJNQ" rel="stylesheet" type="text/css"/><div class="Ds6Zp jY7uzd TnsHTb" data-default-theme-name="Lagos" dir="ltr" id="sitewrapper" jslog="60325; track:impression" lang="en-GB"><div jsaction="click:PTeocc(S0QVxb),EOA6f(xl07Ob),W6ieke(AIbBic),GnquAf(wrzmHe),dwWqwf(HSrbLb),PY12L(dfq7Bc),k9jxC(WvFhjb),zzWmM(nn6Xee);" jscontroller="JbzNG" jsname="MzIxs"><nav class="CS81U" data-panel="navbar-panel" jslog="59798; track:impression"><div class="cfSov oYxtQd MI9wmf" jsname="nn6Xee"><span class="ihSjwf IMnvVe"></span><span class="y5Bz3 IMnvVe"></span><span class="SVpPcd IMnvVe"></span></div><div class="c2zzSe CoIOBe"><a class="oYxtQd" href="/">Art Nail Studio</a></div><a class="dtKbfb oYxtQd" data-enable-ga="true" data-ga-prefix="action-list" data-tracking-element-type="19" id="action-list-9" jslog="// LINT.IfChange(PostCTAType)56040; track:impression,click" target="_blank"><span aria-hidden="true" class="DPvwYc xPRkMe"></span><span>Get Quote</span></a><a class="dtKbfb oYxtQd" data-enable-ga="true" data-ga-prefix="action-list" data-tracking-element-type="2" href="tel:+91-98208-94378" id="action-list-1" itemprop="telephone" jslog="// LINT.IfChange(PostCTAType)56037; track:impression,click" target="_blank"><span aria-hidden="true" class="DPvwYc xPRkMe"></span><span>Call now</span></a></nav><div class="ZmaiX okoOGc"><ul class="sWUWpe"><li class="sAhjff"><a class="oYxtQd" jsname="S0QVxb">Updates</a></li><li class="sAhjff"><a class="oYxtQd" jsname="wrzmHe">Testimonials</a></li><li class="sAhjff"><a class="oYxtQd" jsname="dfq7Bc">Gallery</a></li><li class="sAhjff"><a class="oYxtQd" jsname="WvFhjb">Contact</a></li></ul></div></div><div class="pZxRx b0t70b" data-panel="header-panel" jslog="59797; track:impression"><div class="BLytye"><h1 class="hero__title hY9UDb"><span class="hero__title-content CoIOBe nbOMh" data-field="headline" itemprop="name">Art Nail Studio</span></h1><div class="notification"><span class="notification-content" data-field="announcement" itemprop="description">Nail Salon</span></div><div class="hero__strapline teQaN" data-field="hours-header"><div class="current-hours-content TEPfHc-R86cEd-bN97Pc temporal PTUDTb" id="hours_content">Opening at 10:00</div></div><div><a class="btn SKd3Ne btn--primary SKd3Ne-OWXEXe-ssJRIf site-cta-link jSFuyb-SU0ZEf-hSRGPd PDvGL" data-enable-ga="true" data-field="primary-cta" data-ga-prefix="primary" data-tracking-element-type="18" id="primary_cta" jslog="// LINT.IfChange(PostCTAType)56040; track:impression,click" tabindex="0" target="_blank"><span id="primary_cta_9">Get Quote</span><span class="ZYIfFd" id="primary_cta_1">Call <span dir="ltr">098208 94378</span></span><span class="ZYIfFd" id="primary_cta_11"><span dir="ltr">WhatsApp <span dir="ltr">098208 94378</span></span></span><span class="ZYIfFd" id="primary_cta_4">Message <span dir="ltr">098208 94378</span></span><span class="ZYIfFd" id="primary_cta_12">Contact Us</span><span class="ZYIfFd" id="primary_cta_6">Find Table</span><span class="ZYIfFd" id="primary_cta_10">Make Appointment</span><span class="ZYIfFd" id="primary_cta_7">Place Order</span><span class="ZYIfFd" id="primary_cta_5">View Menu</span></a></div></div><div class="BiTRX"><picture><source media="(max-width: 480px)" srcset="https://lh3.googleusercontent.com/p/AF1QipMXD7KvZFKdNW-i9HzbnxcC6dyaOmj-OvP_4OpA=w480-h270-p-no-v0"/><source media="(min-width: 481px) and (max-width: 768px)" srcset="https://lh3.googleusercontent.com/p/AF1QipMXD7KvZFKdNW-i9HzbnxcC6dyaOmj-OvP_4OpA=w768-h432-p-no-v0"/><img alt="Header image for the site" class="n9wygc" src="https://lh3.googleusercontent.com/p/AF1QipMXD7KvZFKdNW-i9HzbnxcC6dyaOmj-OvP_4OpA=w1080-h608-p-no-v0"/></picture></div></div><section class="El9K7d b0t70b" data-panel="local-post-panel" id="posts" jslog="64600; track:impression"><hr class="WUdCTb"/><h2 class="Igsabe">Updates</h2><div class="xVyTkc cpxwuf"><article class="LzG70e post"><div class="NHc3Rd"><a class="QBP2Td oYxtQd post-date" href="/posts/828031174995435334?hl=en-GB">Posted on Dec 22, 2021</a><div class="bGQFnc"><p class="PE7lZb">Get 20% off on Nail ExtentionğŸ’…ğŸ¥³ for more information call at 9820894378<br/><a href="http://www.instagram.com/art_nail_salon">www.instagram.com/art_nail_salon</a></p></div></div><a class="PDllHc post-cta" data-tracking-element-type="39" href="tel:+91-98208-94378">Call now</a></article><article class="LzG70e post"><div class="fdMcf"><a href="/posts/483076539709192687?hl=en-GB"><picture><source media="(max-width: 480px)" srcset="https://lh3.googleusercontent.com/p/AF1QipPOK7JP3KK9pY0TTqVYAU4KlLcMVZwKmVzVlDQu=s480-p-no-v1"/><source media="(min-width: 481px) and (max-width: 768px)" srcset="https://lh3.googleusercontent.com/p/AF1QipPOK7JP3KK9pY0TTqVYAU4KlLcMVZwKmVzVlDQu=s768-p-no-v1"/><source media="(min-width: 769px) and (max-width: 1024px)" srcset="https://lh3.googleusercontent.com/p/AF1QipPOK7JP3KK9pY0TTqVYAU4KlLcMVZwKmVzVlDQu=s1024-p-no-v1"/><source media="(min-width: 1025px)" srcset="https://lh3.googleusercontent.com/p/AF1QipPOK7JP3KK9pY0TTqVYAU4KlLcMVZwKmVzVlDQu=s1280-p-no-v1"/><img class="aWQIEc post-photo" src="https://lh3.googleusercontent.com/p/AF1QipPOK7JP3KK9pY0TTqVYAU4KlLcMVZwKmVzVlDQu=s1280-p-no-v1"/></picture></a></div><div class="NHc3Rd"><a class="QBP2Td oYxtQd post-date" href="/posts/483076539709192687?hl=en-GB">Posted on Mar 1, 2020</a><div class="bGQFnc"><p class="PE7lZb">Wedding nail art <br/>For appointment call at 9820894378</p></div></div></article><article class="LzG70e post"><div class="fdMcf"><a href="/posts/141270096499001079?hl=en-GB"><picture><source media="(max-width: 480px)" srcset="https://lh3.googleusercontent.com/p/AF1QipOeLHJ__hF__VSkM_7g3UgIY4wbfcy7yw-n5day=s480-p-no-v1"/><source media="(min-width: 481px) and (max-width: 768px)" srcset="https://lh3.googleusercontent.com/p/AF1QipOeLHJ__hF__VSkM_7g3UgIY4wbfcy7yw-n5day=s768-p-no-v1"/><source media="(min-width: 769px) and (max-width: 1024px)" srcset="https://lh3.googleusercontent.com/p/AF1QipOeLHJ__hF__VSkM_7g3UgIY4wbfcy7yw-n5day=s1024-p-no-v1"/><source media="(min-width: 1025px)" srcset="https://lh3.googleusercontent.com/p/AF1QipOeLHJ__hF__VSkM_7g3UgIY4wbfcy7yw-n5day=s1280-p-no-v1"/><img class="aWQIEc post-photo" src="https://lh3.googleusercontent.com/p/AF1QipOeLHJ__hF__VSkM_7g3UgIY4wbfcy7yw-n5day=s1280-p-no-v1"/></picture></a></div><div class="NHc3Rd"><a class="QBP2Td oYxtQd post-date" href="/posts/141270096499001079?hl=en-GB">Posted on Feb 20, 2020</a><div class="bGQFnc"><p class="PE7lZb">Call on 9820894378</p></div></div></article><article class="LzG70e post"><div class="fdMcf"><a href="/posts/4472552633277110029?hl=en-GB"><picture><source media="(max-width: 480px)" srcset="https://lh3.googleusercontent.com/p/AF1QipOR8fUlA9StXxXgxL4Ms3JABaUY9hxKWaU_I2Ed=s480-p-no-v1"/><source media="(min-width: 481px) and (max-width: 768px)" srcset="https://lh3.googleusercontent.com/p/AF1QipOR8fUlA9StXxXgxL4Ms3JABaUY9hxKWaU_I2Ed=s768-p-no-v1"/><source media="(min-width: 769px) and (max-width: 1024px)" srcset="https://lh3.googleusercontent.com/p/AF1QipOR8fUlA9StXxXgxL4Ms3JABaUY9hxKWaU_I2Ed=s1024-p-no-v1"/><source media="(min-width: 1025px)" srcset="https://lh3.googleusercontent.com/p/AF1QipOR8fUlA9StXxXgxL4Ms3JABaUY9hxKWaU_I2Ed=s1280-p-no-v1"/><img class="aWQIEc post-photo" src="https://lh3.googleusercontent.com/p/AF1QipOR8fUlA9StXxXgxL4Ms3JABaUY9hxKWaU_I2Ed=s1280-p-no-v1"/></picture></a></div><div class="NHc3Rd"><a class="QBP2Td oYxtQd post-date" href="/posts/4472552633277110029?hl=en-GB">Posted on Feb 12, 2020</a><div class="bGQFnc"><p class="PE7lZb">For appointment call at 9820894378</p></div></div></article><article class="LzG70e post"><div class="fdMcf"><a href="/posts/5678099036175645678?hl=en-GB"><picture><source media="(max-width: 480px)" srcset="https://lh3.googleusercontent.com/p/AF1QipOZASvcwwoRkbj5_WF556OI0HqARJQcczqweh8z=s480-p-no-v1"/><source media="(min-width: 481px) and (max-width: 768px)" srcset="https://lh3.googleusercontent.com/p/AF1QipOZASvcwwoRkbj5_WF556OI0HqARJQcczqweh8z=s768-p-no-v1"/><source media="(min-width: 769px) and (max-width: 1024px)" srcset="https://lh3.googleusercontent.com/p/AF1QipOZASvcwwoRkbj5_WF556OI0HqARJQcczqweh8z=s1024-p-no-v1"/><source media="(min-width: 1025px)" srcset="https://lh3.googleusercontent.com/p/AF1QipOZASvcwwoRkbj5_WF556OI0HqARJQcczqweh8z=s1280-p-no-v1"/><img class="aWQIEc post-photo" src="https://lh3.googleusercontent.com/p/AF1QipOZASvcwwoRkbj5_WF556OI0HqARJQcczqweh8z=s1280-p-no-v1"/></picture></a></div><div class="NHc3Rd"><a class="QBP2Td oYxtQd post-date" href="/posts/5678099036175645678?hl=en-GB">Posted on Jan 31, 2020</a><div class="bGQFnc"><p class="PE7lZb">Nail extension with gel polish</p></div></div></article><article class="LzG70e post"><div class="fdMcf"><a href="/posts/3957072362281867166?hl=en-GB"><picture><source media="(max-width: 480px)" srcset="https://lh3.googleusercontent.com/p/AF1QipMlCNDZ-sUsDQPx_sr5mqJA3WZvhSm_i_DJEV4-=s480-p-no-v1"/><source media="(min-width: 481px) and (max-width: 768px)" srcset="https://lh3.googleusercontent.com/p/AF1QipMlCNDZ-sUsDQPx_sr5mqJA3WZvhSm_i_DJEV4-=s768-p-no-v1"/><source media="(min-width: 769px) and (max-width: 1024px)" srcset="https://lh3.googleusercontent.com/p/AF1QipMlCNDZ-sUsDQPx_sr5mqJA3WZvhSm_i_DJEV4-=s1024-p-no-v1"/><source media="(min-width: 1025px)" srcset="https://lh3.googleusercontent.com/p/AF1QipMlCNDZ-sUsDQPx_sr5mqJA3WZvhSm_i_DJEV4-=s1280-p-no-v1"/><img class="aWQIEc post-photo" src="https://lh3.googleusercontent.com/p/AF1QipMlCNDZ-sUsDQPx_sr5mqJA3WZvhSm_i_DJEV4-=s1280-p-no-v1"/></picture></a></div><div class="NHc3Rd"><a class="QBP2Td oYxtQd post-date" href="/posts/3957072362281867166?hl=en-GB">Posted on Jan 29, 2020</a><div class="bGQFnc"><p class="PE7lZb">Nail extension ğŸ’…</p></div></div></article><article class="LzG70e post"><div class="fdMcf"><a href="/posts/2644715699915001302?hl=en-GB"><picture><source media="(max-width: 480px)" srcset="https://lh3.googleusercontent.com/p/AF1QipNF748iu5zZ_PaSdPFufTrdezM_ssddsh3nAUOK=s480-p-no-v1"/><source media="(min-width: 481px) and (max-width: 768px)" srcset="https://lh3.googleusercontent.com/p/AF1QipNF748iu5zZ_PaSdPFufTrdezM_ssddsh3nAUOK=s768-p-no-v1"/><source media="(min-width: 769px) and (max-width: 1024px)" srcset="https://lh3.googleusercontent.com/p/AF1QipNF748iu5zZ_PaSdPFufTrdezM_ssddsh3nAUOK=s1024-p-no-v1"/><source media="(min-width: 1025px)" srcset="https://lh3.googleusercontent.com/p/AF1QipNF748iu5zZ_PaSdPFufTrdezM_ssddsh3nAUOK=s1280-p-no-v1"/><img class="aWQIEc post-photo" src="https://lh3.googleusercontent.com/p/AF1QipNF748iu5zZ_PaSdPFufTrdezM_ssddsh3nAUOK=s1280-p-no-v1"/></picture></a></div><div class="NHc3Rd"><a class="QBP2Td oYxtQd post-date" href="/posts/2644715699915001302?hl=en-GB">Posted on Jan 24, 2020</a><div class="bGQFnc"><p class="PE7lZb">New offer for french nail extension...call at 9820894378</p></div></div></article><article class="LzG70e post"><div class="fdMcf"><a href="/posts/6792849031765063287?hl=en-GB"><picture><source media="(max-width: 480px)" srcset="https://lh3.googleusercontent.com/p/AF1QipO-B48QQ0TxpSvqxJMjHZEACXNxEoW-AD90fvG1=s480-p-no-v1"/><source media="(min-width: 481px) and (max-width: 768px)" srcset="https://lh3.googleusercontent.com/p/AF1QipO-B48QQ0TxpSvqxJMjHZEACXNxEoW-AD90fvG1=s768-p-no-v1"/><source media="(min-width: 769px) and (max-width: 1024px)" srcset="https://lh3.googleusercontent.com/p/AF1QipO-B48QQ0TxpSvqxJMjHZEACXNxEoW-AD90fvG1=s1024-p-no-v1"/><source media="(min-width: 1025px)" srcset="https://lh3.googleusercontent.com/p/AF1QipO-B48QQ0TxpSvqxJMjHZEACXNxEoW-AD90fvG1=s1280-p-no-v1"/><img class="aWQIEc post-photo" src="https://lh3.googleusercontent.com/p/AF1QipO-B48QQ0TxpSvqxJMjHZEACXNxEoW-AD90fvG1=s1280-p-no-v1"/></picture></a></div><div class="NHc3Rd"><a class="QBP2Td oYxtQd post-date" href="/posts/6792849031765063287?hl=en-GB">Posted on Jan 24, 2020</a><div class="bGQFnc"><p class="PE7lZb">Call for appointment 9820894378</p></div></div></article><article class="LzG70e post"><div class="fdMcf"><a href="/posts/5126993419328899211?hl=en-GB"><picture><source media="(max-width: 480px)" srcset="https://lh3.googleusercontent.com/p/AF1QipPWaGGwYZihb9_ggEEMRTY0LspGrsQT89uVHh6Q=s480-p-no-v1"/><source media="(min-width: 481px) and (max-width: 768px)" srcset="https://lh3.googleusercontent.com/p/AF1QipPWaGGwYZihb9_ggEEMRTY0LspGrsQT89uVHh6Q=s768-p-no-v1"/><source media="(min-width: 769px) and (max-width: 1024px)" srcset="https://lh3.googleusercontent.com/p/AF1QipPWaGGwYZihb9_ggEEMRTY0LspGrsQT89uVHh6Q=s1024-p-no-v1"/><source media="(min-width: 1025px)" srcset="https://lh3.googleusercontent.com/p/AF1QipPWaGGwYZihb9_ggEEMRTY0LspGrsQT89uVHh6Q=s1280-p-no-v1"/><img class="aWQIEc post-photo" src="https://lh3.googleusercontent.com/p/AF1QipPWaGGwYZihb9_ggEEMRTY0LspGrsQT89uVHh6Q=s1280-p-no-v1"/></picture></a></div><div class="NHc3Rd"><div class="UEkdfe"><h3 class="xkrGZb">Nail Extensions Offer</h3><a class="QBP2Td oYxtQd" href="/posts/5126993419328899211?hl=en-GB">8 Jan 2020 â€“ 10 Jan 2020</a></div><div class="bGQFnc"><p class="PE7lZb">Nail extensions with gel polish 2800rs<br/>&amp;</p></div></div></article></div></section></div></div></div></c-wiz></div></body></html>
    


```python

```


```python

```


```python

```
