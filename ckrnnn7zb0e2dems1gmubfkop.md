## Basic Bash Scripting

Begin with !/ and the bash location  

```
!/usr/bash  
``` 
extension .sh  

Two possibility to run a bash script:  

```
#1
bash script.sh

#2
./script.sh

bash script.sh One Two Three
#One, Two and Three are arguments. You can use then in the script
echo $1     #print first argument, One
echo $3     #print third argument, Three
echo $*     #print all the arguments
echo $#     #print arguments count, 3
``` 


### Variables  
``` 
var1="hello"     #create a variable (no space around =)
$var1     #reference the variable
echo "good morning, $var1 and hi"

#Variable in variable
var2="Hi equal to $(var1)"
``` 

### Array
``` 
myarray=(1 2 3 4 5)
echo ${myarray[@]}     #return all elements
echo ${myarray[@]:1:3}     #return three elements starting from second 
echo ${#myarray[@]}     #return length
echo ${myarray[2]}     #return tird element (zero-indexing)
myarray[0]=10     #change first element
myarray+=(8)     #add a new element at the end
``` 

### IF statement  

if [ condition ]; then  
&nbsp;&nbsp;&nbsp;&nbsp;[do something...]  
else  
&nbsp;&nbsp;&nbsp;&nbsp;[do something else...]  
fi  

``` 
if [ $x == "hello" ]; then
    echo "hello!"
else
    echo "hi!"
fi
``` 

-eq -> equal to  
-ne -> not equal to  
-gt -> greater then  
-lt -> less then  
-ge -> greater  thenor equal to  
-le -> less then or equal to  
-e -> if file exists  
-s -> if file exists and has size greater then 0  
-r -> if file exists and is readable  
-w -> if file exists and is writable  
|| -> OR  
&& -> AND  


### FOR loops

for x in {[START]..[STOP]..[INCREMENT]} [or] (([start];[end];[increment]))  
do  
&nbsp;&nbsp;&nbsp;&nbsp;[do something...]  
done  

``` 
for x in {1..5..2}     #{START..STOP..INCREMENT}
do
    echo $x
done

for ((x=2;x<=8;x+=2))     #((start;end;increment))
do
    echo $x
done

#list all file in dir with the py extension
for file in dir/*.py
do
    echo $file
done

#list all file in dir with the word hello in filename
for file in $(ls dir/ | grep -i 'hello')
do
    echo $file
done
``` 

### WHILE statements
``` 
x = 1
while [ $x -le 10]
do
    echo $x
    ((x+=1))
done
``` 

### CASE statements

case 'VAR' in  
&nbsp;&nbsp;&nbsp;&nbsp;PATTERN1)  
&nbsp;&nbsp;&nbsp;&nbsp;COMMAND1 ;;  
&nbsp;&nbsp;&nbsp;&nbsp;PATTERN2)  
&nbsp;&nbsp;&nbsp;&nbsp;COMMAND2 ;;  
&nbsp;&nbsp;&nbsp;&nbsp;*)  
&nbsp;&nbsp;&nbsp;&nbsp;DEFAULT COMMAND ;;  
esac  

``` 
# move file with name hello, hi in their folder, remove ciao and hola files and move all the other file in another folder
case $(cat $1) in
    *hello*)
    mv $1 hello/ ;;
    *hi*)
    mv $2 hi/ ;;
    *ciao*|*hola*)
    rm $1 ;;
    *)
    mv $1 others/ ;;
esac
``` 

### Basic Functions  

function_name () {  
&nbsp;&nbsp;&nbsp;&nbsp;[function code]  
&nbsp;&nbsp;&nbsp;&nbsp;return [something]  
}  

alternative syntax:  

function function_name {  
&nbsp;&nbsp;&nbsp;&nbsp;[function code]  
&nbsp;&nbsp;&nbsp;&nbsp;return [something]  
}  

call the function:  
function_name  


### Scheduling scripts with Cron  
cron: time-based job scheduler  

```
echo "* * * * * python create_model.py" | crontab  
``` 

echo scheduler command into cromtab  

5 stars to set, one for each time unit:  

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627486424938/2UH4h1541.png)  
``` 
#run every minutes forever
* * * * * bash script.sh

#run every sunday at 13:30
30 13 * * 7 bash script.sh

#run every 15, 30 and 45 minutes
15,30,45 * * * * bash script.sh

#run every 15 minutes
*/15 * * * * bash script.sh

#see job scheduled with cron
crontab -l

#edit your cronjob list
crontab -e
``` 