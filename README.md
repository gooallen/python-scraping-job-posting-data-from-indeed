# python-scraping-job-posting-data-from-indeed
Scraping job posting data from Indeed with Selenium and BeautifulSoup


- **grab_job_links()** to get all the job posting links for a given search result page
- **get_urls()** to loop through all the search result pages and get all the job posting links
- **get_soup()** to obtain the BeautifulSoup soup object for a given url. The function can then be used by the **get_urls()** function for parsing.
- **get_posting()** to open a job posting page and parse out the text portion
- **get_data()** to loop through the url list created above and use the get_posting() function to gather all the texts

### Mimicking the Click

```python
def get_soup(url):
  driver = webdriver.Firefox()
  driver.get(url)
  html = driver.page_source
  soup = BeautifulSoup(html, 'html.parser')
  driver.close()
  return soup
```


### Grabbing Job Links

```python
def grab_job_links(soup):
  urls = []
  for link in soup.find_all('h2', {'class': 'jobtitle'}):
    partial_url = link.a.get('href')
    url = 'https://ca.indeed.com' + partial_url
    urls.append(url)
  return urls
```

Find page count,
```python
soup.find(name='div', attrs={'id':"searchCount"}).get_text()
```

### Getting All Job Links

```python
for i in range(2, num_pages+1):
  num = (i-1) * 10 # first pages has 0 to 9 job postins, second has 10 to 19, ..
  base_url = 'https://ca.indeed.com/jobs?q={}&l={}&start={}'.format(query, location, num)
    try:
      soup = get_soup(base_url)
      urls += grab_job_links(soup)
    except:
      continue
```

### Extracting Text

```python
def get_posting(url):
  soup = get_soup(url)
  title = soup.find)(name='h3').getText().lower()
  posting = soup.find(name='div', attrs={'class': "jobsearch-JobComponent"}).get_text()
    return title, posting.lower()
```

### Handling Exceptions

```python
for i, url in enumerate(urls):
  try:
    title, posting = get_posting(url)
    postings_dict[i] = {}
    postings_dict[i]['title'], postings_dict[i]['posting'], postings_dict[i]['url'] = title, posting, url
  except:
    continue
```

### Saving data

```python
file_name = query.replace('+','_'_ + '.json'
with open(file_name, 'w') as f:
  json.dump(postings_dict, f)
```





