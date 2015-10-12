---
layout: post_tech
title:  "Bash Scripting with FTP" 
date:   2014-07-18 13:38:22 +0800
comments: true
categories: [tech]
tags: [python]
---

## Requirements

Introduction:

- Connect to ftp host 10.11.12.13 port 22 user=hello password=world
- Download 5 files called webtrekk_Marketing Report_2013-01-01_VN.zip

Requirements:

- Where 2013-01-01 is always the rolling date of 28 days ago regardless of the current date (e.g. if today is 2013-03-29 then the date should be 2013-03-01 and if today is 2013-03-30 then the date should be 2013-03-02)
- VN can be any of [SG, MY, TH, PH, VN]
- Into directory home/Marketing Report/Data

Processing:

- Run ZMR.py which is situated in home/Marketing Report
- Copy WT.csv from the local directory home/Marketing/Report to a directory that a standard mysql installation is allowed to use (google it if unsure)
- Log on to mysql with local-infile=1
- Run the query UpdateWebtrekk.sql


## Source Codes

```python
import os
import shutil
from ftplib import FTP
from datetime import date, timedelta

import MySQLdb

class Database:
    """ creating the class Database """

    def __init__(self, host, user, passwd, db):
        """ initializing database and connecting """
        
        self.host = host
        self.user = user
        self.passwd = passwd
        self.db = db

        self.conn = MySQLdb.connect(host=self.host, 
                                    user=self.user, 
                                    passwd=self.passwd, 
                                    db=self.db, 
                                    local_infile = 1)  # loading local file allowed
        self.cursor = self.conn.cursor()

    def insert(self, sql):
        """ inserting records into database """
        
        try:
            self.cursor.execute(sql)
            self.conn.commit()
        except:
            self.conn.rollback()

    def query(self, sql):
        """ querying data with sql """
        
        cursor = self.conn.cursor()
        cursor.execute(sql)

        return cursor.fetchall()

    def close(self):
        """ closing cursor and connection """
        
        self.cursor.close()
        self.conn.close()


def createFileName(countryList):
    """ generating the names of zip files """

    today = date.today()
    fileDate = today - timedelta(days=28)  # rolling 28 days ago

    fileNames = []
    for country in countryList:
        fileName = 'webtrekk_Marketing Report_%s_%s.zip' % (str(fileDate), str(country))
        fileNames.append(fileName)
    
    return fileNames


def ftpDownload(host, port, user, passwd, remotedir, localdir, filename):
    """ downloading zip files from ftp and then saving them to the directory """
    
    ftp = FTP()
    #ftp.set_debuglevel(2)  # 2: display, 0: not display
    ftp.connect(host, port)  # host, port
    ftp.login(user, passwd)  # user, passwd

    ftp.cwd(remotedir)  # connecting the directory of files
    #ftp.retrlines('LIST') # listing files under directory

    # creating local file path
    localPath = localdir + filename
    file_handler = open(localPath, 'wb').write

    # downloading files from ftp and saving to local path
    ftp.retrbinary("RETR %s" % filename, file_handler)
    print "File %s has been saved in %s." % (filename, localPath)

    ftp.quit()

    
def copyCSV(csvfile, csvdir, todir):
    """ copying csv file to mysql directory"""

    if os.path.exists(todir+csvfile):
        # if csv file exists, remove csv file and copy the new one
        os.remove(todir+csvfile)
        shutil.copy(csvdir+csvfile, todir)
    else:
        # if csv file does not exist, copy csv file to the directory
        shutil.copy(csvdir+csvfile, todir)
    
    print "File %s has been copied to the directory %s." % (str(csvfile), str(todir))
    
    
def main():

    # step1. creating file names
    countryList = ['SG', 'MY', 'TH', 'PH', 'VN']
    fileNames = createFileName(countryList)
    
    # step2. downloading files to local directory
    for fileName in fileNames:
        ftpDownload('10.11.12.13', '22', 'hello', 'world', 'XXX/XXX', 
                    'home/Marketing Report/Data/', fileName)

    # step3. running python script
    pyPath = 'home/Marketing Report/'
    execfile(pyPath + 'ZMR.py')

    # step4. copying csv file to mysql directory
    csvfile = 'WT.csv'
    csvdir = 'home/Marketing/Report'
    todir = 'mysql_direcotry'
    copyCSV(csvfile, csvdir, todir)
    
    # step5. log onmysql with local-infile=1 and run query
    db = Database('localhost', 'root', 'pw_root', 'test')
    for line in open('UpdateWebtrekk.sql', 'rb').readlines():
        db.insert(line)
    db.close()    


if __name__ == '__main__':
    main()
```


## Cron Job

schedule the above as a cron job every day at 2.35am

```
# cron: [minute] [hour] [day] [month] [weekday] [command] 
35 02 * * * /python_path /path/script.py
```

coding in python

```python
# step1. install python-crontab
# step2. write script in python

from crontab import CronTab

tab = CronTab(user='xxx', fake_tab='True')

# setting up command
cmd = '/python_path /path/script.py'
cron_job = tab.new(cmd, comment='xxxxx')
crob_job.minute().on(35)
crob_job.hour().on(2)

# writing content
tab.write()

print tab.render()
```
