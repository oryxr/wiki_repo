Script python de scraping de site WEB
=====================================

Exemple de scraping d'un site web:

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    import sys
    import requests
    import bs4
    import calendar
    import time
    import re
    
    """
    	This script can scrape table into html page.
    	You must analyse the structure of the page and modify the code to use it for your site
    """
    
    # Create list of date [year, month, day] for scrap data in multi pages site.
    cal = calendar.Calendar()
    first_year = 2013
    last_year = 2016
    dates=[]
    for year in range(first_year, last_year):
    	for month in range(1,13):
    		for d in cal.itermonthdays(year,month):
    			if (d!=0):
    				dates.append([year, month, d])
    
    
    # Scraping code, write data into file csv
    for date in dates:
    	url = 'http://www.epexspot.com/fr/donnees_de_marche/intradaycontinuous/intraday-table/'+\
    		  str(date[0])+'-'+str(date[1]).zfill(2)+'-'+str(date[2]).zfill(2)+'/FR'
    	response = requests.get(url)
    
    	soup = bs4.BeautifulSoup(response.text, "html.parser")
    	trs = soup.select('table tbody tr')[3:-2]
    	tds = [tr.select('td')[1:] for tr in trs]
    	hours = []
    	for td in tds:
    		hour = [val.text.replace('\n','').replace(' ','').replace('-',':') for val in td]
    		hour = hour[:1]+hour[9:]
    		hours.append(hour)
    
    	with open(str(date[0])+'_donnees_epex.csv', 'a', encoding='utf-8') as file:
    		for hour in hours:
    			hour[0] = hours.index(hour)
    			file.write(str(date[0])+'/'+str(date[1]).zfill(2)+'/'+str(date[2]).zfill(2)+' '+str(hour[0]).zfill(2)+':00; ')
    			for value in hour[1:-1]:
    				if re.search(r"^[0-9]+,*[0-9]+", value) is None:
    					value=''
    				# value = value.replace('\u2013','')
    				file.write(value+"; ")
    			if re.search(r"^[0-9]+,*[0-9]+", hour[-1]) is None:
    				hour[-1]=''
    			file.write(hour[-1])
    			file.write('\n')
    	print(url)
    	time.sleep(5)
    	
Source : [easy-web-scraping-with-python](http://blog.miguelgrinberg.com/post/easy-web-scraping-with-python)