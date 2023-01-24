```python
pip install beautifulsoup4
```


```python
pip install MechanicalSoup

```

    Collecting MechanicalSoup
      Using cached MechanicalSoup-1.2.0-py3-none-any.whl (19 kB)
    Requirement already satisfied: requests>=2.22.0 in d:\software\anaconda\lib\site-packages (from MechanicalSoup) (2.26.0)
    Requirement already satisfied: beautifulsoup4>=4.7 in d:\software\anaconda\lib\site-packages (from MechanicalSoup) (4.10.0)
    Requirement already satisfied: lxml in d:\software\anaconda\lib\site-packages (from MechanicalSoup) (4.6.3)
    Requirement already satisfied: soupsieve>1.2 in d:\software\anaconda\lib\site-packages (from beautifulsoup4>=4.7->MechanicalSoup) (2.2.1)
    Requirement already satisfied: charset-normalizer~=2.0.0 in d:\software\anaconda\lib\site-packages (from requests>=2.22.0->MechanicalSoup) (2.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in d:\software\anaconda\lib\site-packages (from requests>=2.22.0->MechanicalSoup) (2021.10.8)
    Requirement already satisfied: idna<4,>=2.5 in d:\software\anaconda\lib\site-packages (from requests>=2.22.0->MechanicalSoup) (3.2)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in d:\software\anaconda\lib\site-packages (from requests>=2.22.0->MechanicalSoup) (1.26.7)
    Installing collected packages: MechanicalSoup
    Successfully installed MechanicalSoup-1.2.0
    Note: you may need to restart the kernel to use updated packages.
    


```python
from urllib.request import urlopen
import re
from bs4 import BeautifulSoup
import mechanicalsoup
```


```python
url = "http://olympus.realpython.org/profiles/aphrodite"
```

### URLopen（）returns an HTTPResponse object


```python
page = urlopen(url)
page
```


```python
html_bytes = page.read()
print(html_bytes)
html = html_bytes.decode('utf-8')
print(html)
print(type(html))
```


```python
### return the index of the first occurrence of a sbustring
```


```python
title_index = html.find('<title>')
print(title_index)
start_index = title_index + len("<title>")
print(start_index)
end_index = html.find("</title>")
print(end_index)
title = html[start_index:end_index]
print(title)
```


```python
url = "http://olympus.realpython.org/profiles/dionysus"
page = urlopen(url)
html = page.read().decode("utf-8")
print(html)

pattern = '<title.*?>.*?</title.*?>'
match_results = re.search(pattern, html, re.IGNORECASE)
title = match_results.group()
print(title)
title = re.sub('<.*?>',"",title)
print(title)
```


```python
url = "http://olympus.realpython.org/profiles/dionysus"
page = urlopen(url)
html = page.read().decode('utf-8')
print(html)
soup = BeautifulSoup(html, 'html.parser')
print(soup)
```


```python
print(soup.get_text())
```


```python
img1, img2 = soup.find_all('img')
```


```python
img1['src']
```


```python
soup.title.string
```

### MechanicalSoup

### create a browser instance and use it to request the URL , ass the HTML content to variable using .soup property


```python
browser = mechanicalsoup.Browser()
url = "http://olympus.realpython.org/login"
login_page = browser.get(url)
login_html = login_page.soup
print(login_html)
```

    <html>
    <head>
    <title>Log In</title>
    </head>
    <body bgcolor="yellow">
    <center>
    <br/><br/>
    <h2>Please log in to access Mount Olympus:</h2>
    <br/><br/>
    <form action="/login" method="post" name="login">
    Username: <input name="user" type="text"/><br/>
    Password: <input name="pwd" type="password"/><br/><br/>
    <input type="submit" value="Submit"/>
    </form>
    </center>
    </body>
    </html>
    
    


```python
form = login_html.select('form')[0]
print(form)
```

    <form action="/login" method="post" name="login">
    Username: <input name="user" type="text"/><br/>
    Password: <input name="pwd" type="password"/><br/><br/>
    <input type="submit" value="Submit"/>
    </form>
    

### select the username and password inouts and set the value


```python
form = login_html.select('form')[0]
print(form)
form.select('input')[0]['value'] = 'zeus'
form.select("input")[1]["value"] = "ThunderDude"
```

    <form action="/login" method="post" name="login">
    Username: <input name="user" type="text" value="zeus"/><br/>
    Password: <input name="pwd" type="password" value="ThunderDude"/><br/><br/>
    <input type="submit" value="Submit"/>
    </form>
    

### you submit the form, and pass two arguments to this method


```python
profiles_page = browser.submit(form, login_page.url)
```


```python
links = profiles_page.soup.select('a')
print(links)
```

    [<a href="/profiles/aphrodite">Aphrodite</a>, <a href="/profiles/poseidon">Poseidon</a>, <a href="/profiles/dionysus">Dionysus</a>]
    


```python
for link in links:
    address = link['href']
    text = link.text
```
