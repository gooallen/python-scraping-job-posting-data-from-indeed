# python-scraping-job-posting-data-from-indeed
Scraping job posting data from Indeed with Selenium and BeautifulSoup

Reference: https://towardsdatascience.com/scraping-job-posting-data-from-indeed-using-selenium-and-beautifulsoup-dfc86230baac


- **grab_job_links()** function to get all the job posting links for a given search result page
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







