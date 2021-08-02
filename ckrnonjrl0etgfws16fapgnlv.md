## Data Processing in Shell

Download Data using curl  

[https://curl.haxx.se/download.html](https://curl.haxx.se/download.html)   

```
curl -O https://websitename.com/file001.txt     
#-O -> download file with it's name

curl -o newname.txt https://websitename.com/file001.txt     
#-o -> download file with new name

curl -O https://websitename.com/*.txt     

curl -O https://websitename.com/file[001-100].txt

curl -O https://websitename.com/file[001-100:10].txt
#download only file010.txt, file020.txt,....,file100.txt

curl -L -C -O https://websitename.com/file[001-100:10].txt
#-L -> redirects the http url if a 300 error code occurs
#-C -> resumes a previous file transfer if it times out
``` 
with curl you can also download data from 20+ protocols  

### Downloading Data using Wget  

[https://www.gnu.org/software/wget/](https://www.gnu.org/software/wget/)  

same to curl with this unique option flag:  
-b -> background download  
-q -> turn off output  
-c -> resume broken download  
```
#you can link the different option flags
wget -bqc https://websitename.com/file001.txt

#take all the file links in the txt and dowload them
wget -i list.txt

#other option flags must go before -i
#set download limit rate to 200k per second
wget --limit-rate=200k -i list.txt

#put a 1.5 second pause between each file download
wget --wait=1.5 -i list.txt
``` 
with wget you can also download data from (s)ftp  


### Data Cleaning and Munging with csvkit  
``` 
pip install csvkit
pip install --upgrade csvkit

#convert first sheet to csv
in2csv file.xlsx > file.csv     

#convert NameSheet2 sheet to csv
in2csv file.xlsx --sheet "NameSheet2" > file.csv    

#see csv file content
csvlook file.csv     

#print summary statistics of all columns
csvstat file.csv     
``` 
csvcut to filter colums  

``` 
csvcut -n     #see column names and positions
csvcut -c 1 file.csv     #return first column
csvcut -c "colname" file.csv     #return colname column 
csvcut -c 2,3 file.csv     #return second and third columns
csvcut -c "colname","colname2" file.csv     
``` 

csvgrep to filter rows  

must be paired with one of these options:  
-m -> exact row value to filter  
-r -> regex pattern  
-f -> path to a file  

``` 
#search in file.csv where col column equal to value
csvgrep -c "col" -m value file.csv

#search in file.csv where third column equal to value
csvgrep -c 3 -m value file.csv
``` 

csvstack for stacking multiple csv files, the files must have same columns, same order, same data types  
```
csvstack file1.csv file2.csv > fileall.csv

#make a group column with g1 if the record is from file1
csvstack -g "g1","g2" file1.csv file2.csv > fileall.csv

#same as before but choose a name for the group column
csvstack -g "g1","g2" -n "source" file1.csv file2.csv > fileall.csv
```

### Pulling data from database with csvkit  

sql2csv work with SQL Server, MySQL, Oracle, Postgresql, and others  
sql2csv exec a query and put the results in a csv  

```
#extract data from sqlite db to csv
sql2csv --db "sqlite:///dbname.db" --query "SELECT * FROM table" > file.csv
```

csvsql uses SQL for query csv files  
 
```
csvsql --query "SELECT * FROM file" file.csv
csvsql --query "SELECT * FROM file" file.csv | csvlook     #better printout
csvsql --query "SELECT * FROM file" file.csv > result.csv

#join two files
csvsql --query "SELECT * FROM file JOIN file2 ON ..." file.csv file2.csv

#use shell variable for store the query
sqlquery = "SELECT * FROM file JOIN file2 ON file.col = file2.col"
csvsql --query "$sqlquery" file.csv file2.csv
```

csvsql used also for querying a db, manipulate objects on it and push data  

```
#insert data in a new table called file in the mysql db dbs
csvsql --db "mysql:///dbs" -- insert file.csv

#use query
csvsql --db "mysql:///dbs" --query "SELECT * FROM file" -- insert file.csv
```

other useful attributes:  
--no-inference -> create all table columns in text format  
--no-constraints -> no length limits or null checks  

### Python on the command line
```
python     #start Python
echo "print('hello world!')"  > helloworld.py

python helloworld.py     #execute the script

pip install --upgrade pip

pip list     #see installed packages

pip install scikit-learn

pip install -r requirements.txt     #install packages listed in file
```

### cron: time-based job scheduler for scripts
```
#echo scheduler command into cromtab
echo "* * * * * python create_model.py" | crontab
```
\* \* \* \* \* run job every minutes forever  
\* meaning and order  
first \* -> minute (0-59)  
second \* -> hour (0-23)  
third \* -> day of month (1-31)  
fourth \* -> month (1-12)  
fifth \* -> day of week (0-6, Sunday = 0)  

```
# see job scheduled with cron
crontab -l     
```
