### Overview
This Web_Exploration's goal is a Python script meant to grab a list of 250,000 URLs from a local Excel sheet, gather the redirect and scrape relevant information from the pages 
for each one, package it into an object and send the result object back to the Excel sheet.

### Procedure
#### Locations/Where to Start
URL_Processor houses all the relevant scripts
First part of script (redirects) handled in Redirect_Script_Send.ipynb therein. That work is now integrated into the scraping portion as well in Website_Information_Scraper.ipynb located there as well. \
Reports_Notes_Explorations.ipynb there is self-explanatory. \
The Trials folder contains mostly failed attempts. \
The Excel_Sheets_Public folder contains a csv file that has the start of one list of URLs to see the effect of script. \
The csv files will contain 9 columns: \
Company ID (18 Char) | Company Record Type | Company Name | Website | Website Redirect | Nav | Headers | Home Page | First Page

The place to start is integrated_main2() in Website_Information_Scraper.ipynb, it all stems from there. Incorporates the API for metadata, the asynchronous redirect capturing and the now asynchronous scraping.

#### Process
The process for scraping is as follows:
An HTTPRequest is sent via the Python requests library and upon success, the 'nav' setup for the website is scraped with the BeautifulSoup(bs4) library. \
The script then proceeds to scrape the home page (original URL) and the first relevant href found in the nav.

A 'relevant href' is defined as an href with the same netloc as the original URL, but is not the same URL entirely. In most cases, this returns the first page on the website 
besides the home page, which contains substantial information about the company and products it sells. \
If at any point in this process, an error is thrown, then the script defaults to Selenium scraping. This is the backup because Selenium starts its own driver - this allows it 
to bypass robot detection much more frequently, but it is also a much longer process. Selenium analogously scrapes for the nav, finds the first relevant href and scrapes both 
the home page and the first page. My next goal, to tackle the constraint of time, is to spread the asynchroneity from the beginning of the scraping lower down into it whereby 
the nav, the home page and the first page will each have their designated drivers. \
As of now, two drivers start for each URL visited by the Selenium route (one for the nav and a stealth driver to reliably scrape the two website pages). If I have three drivers 
in total, this reduces the overhead cost of repeatedly starting drivers and the third, 'first page', driver, can await the nav driver to finish the nav scraping/parsing so that 
it gets the first relevant href.

#### Appendix
0018X00002wGCv0QAG | Sponsor / Lender / IB | Sunstone Partners | https://sunstonepartners.com | https://sunstonepartners.com | [[[[[('Why Us', 'https://sunstonepartners.com/why-us/')], [('Team', 'https://sunstonepartners.com/team/')], [('Portfolio', 'https://sunstonepartners.com/portfolio/')], [('News', 'https://sunstonepartners.com/news/')], [('Contact', 'https://sunstonepartners.com/contact/'), [[('LP Relations', 'https://sunstonepartners.com/lprelations/')]]]]]], 'https://sunstonepartners.com/why-us/'] | ['A partner \nfor growth', 'Who We Are', 'What we do', 'About Sunstone Partners', 'contact us', 'CLEAR VISION FOR GROWTH', 'About Sunstone Partners', 'contact us'] | {'first_relevant': ['We are a leading growth oriented private equity firm that makes majority and minority investments in technology-enabled services and software businesses. We seek to partner with exceptional entrepreneurs, often as their first institutional capital partner, to help accelerate organic growth and fund acquisitions. We have over 70 years of collective experience investing and helping technology companies achieve successful outcomes.', 'Sunstone Partners was founded in 2015 and has $1.7 billion of committed capital under its first three funds. We invest in majority and minority transactions. We are entrepreneurs ourselves, so we appreciate and respect the journey of our entrepreneurs and are committed to delivering muscle to their businesses with humility and mutual respect.'], 'two_longest': ['We are a leading growth oriented private equity firm that makes majority and minority investments in technology-enabled services and software businesses. We seek to partner with exceptional entrepreneurs, often as their first institutional capital partner, to help accelerate organic growth and fund acquisitions. We have over 70 years of collective experience investing and helping technology companies achieve successful outcomes.', 'Awarded by Inc. Magazine in October 2020, 2021 and 2022. This determination was made following application from private equity / venture capital firms that have exited U.S.-based, founder-led, private portfolio companies. The submitted application included references from portfolio companies. This award reflects Inc. Magazineâ€™s determination each such year that Sunstone Partners is a founder friendly firm that entrepreneurs can collaborate and trust as they receive the financial support they need to help drive growth. A fee was paid to be considered for the award. The full extent of the range and scope of firms and data included in the evaluation is unknown. To compile the list, Inc. went straight to the source: entrepreneurs who have sold to private equity and venture capital firms. Founders filled out a questionnaire about their experiences partnering with private equity and venture capital firms and shared data on how their portfolio companies have grown during these partnerships'], 'headers': ['A partner \nfor growth', 'Who We Are', 'What we do', 'About Sunstone Partners', 'contact us']} | {'first_relevant': [', or sunstone, was said to reveal the true bearing of a hidden sun, even on overcast days. This optical compass provided guidance and clarity on trying voyages.', 'We look forward to providing guidance and a clear vision for growth to entrepreneurs.'], 'two_longest': ['Our collaborative portfolio engagement approach enables our companies to thrive and maximize their market opportunity. We seek to build on existing strengths of our companies and are focused on opportunities that provide leverage to our management teams â€“ in terms of strategic guidance, data driven insights, operational best practices and access to our rich network of industry leaders and partners. We provide resources and skills in the following areas.', 'We live and breathe technology-enabled services and software. We focus on key sectors that benefit from macro tailwinds and technology disruption, and in which we have had numerous historical successful investments. We take the time to understand growth trends and opportunities beyond the horizon and seek to identify future industry leaders. This thesis driven, sector focused approach allows us to have a unique perspective that benefits our portfolio companies.'], 'headers': ['CLEAR VISION FOR GROWTH', 'About Sunstone Partners', 'contact us']}
