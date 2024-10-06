---
title: "Basic Shell"
datePublished: Sat Jul 03 2021 16:26:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrnp9wqf0ezvfws1cs5aevee
slug: basic-shell
tags: unix, bash, shell, beginners

---

```sql
pwd     #print working directory

ls     #list content

ls /dir/subdir     #starts with / -> absolute path

ls dir/subdir     #doesn't start with / -> relative path

ls -R    #see also content in the subdirectory

ls -F     #see also a * after runnable programs, / after directories

cd     #change directory

..     #parent directory

cd ..     #go to parent directory

ls ..     #list parent directory content

~     #home directory

cd ~     #go to home directory

ls ~     #list home directory content

cp     #copy files

cp original.txt duplicate.txt     #copy original.txt and name it duplicate.txt
```

NB: use quotes if there are spaces in files or directories names

```sql
cp original.txt duplicate.txt backup     #copy the 2 files in the backup directory

#mv: move or rename files or directories
#move the files from backup to the parent directory
mv backup/original.txt backup/duplicate.txt ..    
 
mv original.txt old.txt     #rename original.txt in old.txt

mv backup backuptwo     #rename the backup directory

rm     #remove files

rm old.txt duplicate.txt     #remove the 2 files

rmdir     #remove a directory only if it's empty

rmdir backuptwo

mkdir     #make a new directory

mkdir backup

cat     #print file content

cat test.txt

less     #display file content one page at time

less test.txt     #now we can use spaceboard for go to next page or q to quit

less test.txt new.txt
```

:n to go to the next file, :p for go to the previous one, :q to quit

```sql
head     #display first 10 lines

head -n 3 test.txt     #only first 3 lines
```

NB: with tab you have auto completion, double tab in case of ambiguity

```sql
man     #find out what a command do

man less

cut      #select column from a file

#select columns from 2 to 5 and 8 using comma as separator
cut -f 2-5,8 -d , file.csv     #-f = fields, -d = delimiter

grep     #select a line in file according to what contain

grep hello file.csv     #select lines with hello
```

**grep patterns**  
\-c: print a count of matching lines rather than the lines themselves  
\-h: do not print the names of files when searching multiple files  
\-i: ignore case (e.g., treat "Regression" and "regression" as matches)  
\-l: print the names of files that contain matches, not the matches  
\-n: print line numbers for matching lines  
\-v: invert the match, i.e., only show lines that don't match

```sql
grep -n -v hello test.csv     #select lines with lines number without hello

grep -c hello file.csv two.csv     #how many lines with hello in the two files 

sed     #replace

sed 's/hello/hi/g' test.txt     #replace hello with hi in test.txt
```

**\&gt;** redirects output to a file

```sql
head -n 3 test.txt > test2.txt
```

**|** create pipe

```sql
#take first 9 rows, then take the last two of the result (8-9)
head -n 9 test.txt > tail -n2

#1. select first column of the comma delimited file.csv in dir
#2. remove lines with "Date" (maybe the header)
#3. take the first ten lines
cut -d , -f 1 dir/file.csv | grep -v Date | head -n 10
```

**wc** count of the character (-c), words (-w), lines (-l) in a file

```sql
grep hello file.csv | wc -l     #num of records with hello
```

**wildcards**  
\* -&gt; matches 0 or more characters  
? -&gt; matches a single character  
\[...\] -&gt; matches any one of the characters in the brackets  
{...} -&gt; matches any of the comma-separated patterns in the brackets

```sql
cut -d , -f 1 dir/*.csv     #first field of all csv files in dir

cut -d , -f 1 dir/c*.csv     #first field of all csv files in dir starting with c
```

**sort** put data in order (alphabetically without patterns)

**sort patterns**  
\-r -&gt; reverse alphabetical order  
\-r -&gt; sort numerically  
\-b -&gt; ignore leading blanks  
\-f -&gt; case insensitive

```sql
#take column 2 of the file, only the lines with hello, reverse order
cut -d , -f 2 dir/file.csv | grep -v hello| sort -r
```

**uniq** remove adjacent duplicate lines

```sql
# take second column of the file without duplicates
cut -d , -f2 file.csv | sort -r | uniq

# same but with count of how often each occours
cut -d , -f2 file.csv | sort -r | uniq -c

# same the result in new.csv
cut -d , -f2 file.csv | sort -r | uniq -c > new.csv
```

**ctrl+c** for stop a running program

**some environment variables**  
HOME -&gt; User's home directory  
PWD -&gt; Present working directory  
SHELL -&gt; Which shell program is being used  
USER -&gt; User's ID

```sql
echo     #prints its argument

echo hello     #print hello

echo $USER      #print the value of the variable

testvar=dir/file.csv    #assign dir/file.csv to the variable testvar

head -n 1 testvar
```

**expr** for numeric calculation but without decimal

```sql
expr 1+3
```

**bc** is a calculator program, you can use it in a pipe

```sql
echo "5 + 7.5" | bc

echo "scale = 3; 10 / 3" | bc     #scale for how many decimals
```

**for loops**  
for \[variable\] in \[list\] ; do \[body\] ; done

```sql
# print second line of each csv in directory dir
for file in dir/*.csv; do head -n 2 $file | tail -n 1; done
```

```sql
nano file.txt     #edit file.txt with nano text editor

history     #see your command history

history | tail -n 3 > steps.sh    #save your last 3 steps to file

head -n 1 dir/*.csv > header.sh     #save command in sh file

bash headers.sh     #tell shell to run commands in the file

$@     #all of the command-line parameters given to the script
```

**Es.** if unique-lines.sh contains sort $@ | uniq, executing:

```sql
bash unique-lines.sh dir/file.csv
```

will run this command:

```sql
sort dir/file.csv | uniq
```

if you execute this:

```sql
bash unique-lines.sh dir/file.csv dir/file2.csv
```

it processes both the files.  
Use $1, $2, and so on to refer to specific command-line parameters:

```sql
head -n $2 $1 | tail -n 1 | cut -d , -f $3 > get-field.sh
#take a filename, the row to select, the column to select, and print
bash get-field.sh dir/file.csv 4 2
```

you can write for loops in shell scripts without semicolon

```sql
# Print the first and last data records of each file.
for filename in $@
do
    head -n 2 $filename | tail -n 1
    tail -n 1 $filename
done
```

in shell scripts use # for comments

use \\ to go to new line