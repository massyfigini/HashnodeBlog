## Coursera Data Science Specialization: Exploratory Data Analysis Course Project 2

My Course Project 2 of the [Exploratory Data Analysis course](https://www.coursera.org/learn/exploratory-data-analysis), part of the  [Data Science Specialization by Johns Hopkins University on Coursera](https://www.coursera.org/specializations/jhu-data-science) .  
You can find the data [here](https://d396qusza40orc.cloudfront.net/exdata%2Fdata%2FNEI_data.zip).  

----------------------------------------------------------------------------

The overall goal of this assignment is to explore the National Emissions Inventory database and see what it say about fine particulate matter pollution in the United states over the 10-year period 1999–2008. You may use any R package you want to support your analysis. For each question/task you will need to make a single plot.  
Unless specified, you can use any plotting system in R to make your plot.  


**QUESTION 1**: Have total emissions from PM2.5 decreased in the United States from 1999 to 2008?  
Using the base plotting system, make a plot showing the total PM2.5 emission from all sources for each of the years 1999, 2002, 2005, and 2008.  


```
#1. Read and prepare data
NEI <- readRDS("summarySCC_PM25.rds")
#I use dplyr
library(dplyr)
NEIdp <- tbl_df(NEI)
#table with emissions per year
EmissionsForYear = summarize(group_by(NEIdp, year), sum(Emissions))
#renamed columns
colnames(EmissionsForYear) <- c("Year", "Emissions")
#new column with emissions in million (for the y axis)
EmissionsForYear$EmissionsInMillions = EmissionsForYear$Emissions / 1000000

#2. Plot (png file)
png('plot1.png')
barplot(EmissionsForYear$EmissionsInMillions, names.arg=EmissionsForYear$Year, col="blue", xlab='Years', ylab='Emissions (PM 2.5) in millions', main =  'Emissions (PM 2.5) per year')
dev.off()
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627659542543/nVS1f54jB.png)

**QUESTION 2**: Have total emissions from PM2.5 decreased in the Baltimore City, Maryland (fips == "24510") from 1999 to 2008?  
Use the base plotting system to make a plot answering this question.  
``` 
#1. Read and prepare data
NEI <- readRDS("summarySCC_PM25.rds")
library(dplyr)
NEIdp <- tbl_df(NEI)
#table with emissions per year of Baltimore
BaltimoraEmissionsForYear = summarize(group_by(filter(NEIdp, fips=="24510"), year), sum(Emissions))
#renamed columns
colnames(BaltimoraEmissionsForYear) <- c("Year", "Emissions")

#2. Plot
png('plot2.png')
barplot(BaltimoraEmissionsForYear$Emissions, names.arg=EmissionsForYear$Year, col="red", xlab='Years', ylab='Emissions (PM 2.5)', main =  'Baltimore City: Emissions (PM 2.5) per year')
dev.off()
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627659600102/x_bz8H-Ux.png)

**QUESTION 3**: Of the four types of sources indicated by the type (point, nonpoint, onroad, nonroad) variable, which of these four sources have seen decreases in emissions from 1999-2008 for Baltimore City?  
Which have seen increases in emissions from 1999-2008?  
Use the ggplot2 plotting system to make a plot answer this question.  

``` 
#1. Read and prepare data
NEI <- readRDS("summarySCC_PM25.rds")
library(dplyr)
NEIdp <- tbl_df(NEI)
#Baltimore emissions per year and type
BaltimoraEmissionsTypeYears = summarize(group_by(filter(NEIdp, fips=="24510"),type,year),sum(Emissions))
#renamed columns
colnames(BaltimoraEmissionsTypeYears) <- c("Type","Year", "Emissions")
#year converted to string
BaltimoraEmissionsTypeYears$Year <- as.character(BaltimoraEmissionsTypeYears$Year)

#2. Plot
library(ggplot2)
png('plot3.png')
qplot(Year,data=BaltimoraEmissionsTypeYears, geom="bar", weight=Emissions, facets=.~Type, fill=Year, main='Baltimore City: Emissions (PM 2.5) per year and type', xlab='', ylab = 'Emissions (PM 2.5)')
dev.off()
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627659672724/-JOR9_rpS.png)

**QUESTION 4**: Across the United States, how have emissions from coal combustion-related sources changed from 1999–2008?

```
#1. Read and prepare data
NEI <- readRDS("summarySCC_PM25.rds")
SCC <- readRDS("Source_Classification_Code.rds")
#merge the tables
NEISCC <- merge(NEI, SCC, by="SCC")
library(dplyr)
NEISCCdp <- tbl_df(NEISCC)
#search for "coal" and create boolean column
NEISCCdp <- mutate(NEISCCdp, coal = grepl("coal", NEISCCdp$Short.Name, ignore.case=TRUE)) 
#renamed columns
EmissionsCoalYear <- summarize(group_by(filter(NEISCCdp, coal==TRUE),year),sum(Emissions))
colnames(EmissionsCoalYear) <- c("Year", "Emissions") 
#year converted to string
EmissionsCoalYear$Year <- as.character(EmissionsCoalYear$Year)
#new column with emissions in thousands (for the y axis)
EmissionsCoalYear$EmissionsInTousands = EmissionsCoalYear$Emissions/1000

#2. Plot
library(ggplot2)
png('plot4.png')
g <- ggplot(EmissionsCoalYear, aes(Year, EmissionsInTousands))
g+geom_bar(stat='identity')+labs(title="Emissions from coal combustion-related sources", x="Years",y="Emissions (PM 2.5) in thousands")
dev.off()
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627659742515/P373zMiYl.png)

**QUESTION 5**: How have emissions from motor vehicle sources changed from 1999-2008 in Baltimore City?
``` 
#1. Read and prepare data
NEI <- readRDS("summarySCC_PM25.rds")
library(dplyr)
NEIdp <- tbl_df(NEI)
#table with Motor Vehicle emissions per year of Baltimore
BaltimoraEmissionsVehicle = summarize(group_by(filter(NEIdp, fips=="24510", type=='ON-ROAD'), year), sum(Emissions)) 
#renamed columns
colnames(BaltimoraEmissionsVehicle) <- c("Year", "Emissions")

#2. Plot
png('plot5.png')
g <- ggplot(BaltimoraEmissionsVehicle, aes(Year,Emissions))+scale_x_continuous(breaks = c(1999,2002,2005,2008))
g+geom_point(size=4, color='orange')+geom_line(size=1.5,color='orange')+labs(title="Baltimore City: Emissions of motor vehicle", x="Years",y="Emissions (PM 2.5)")
dev.off()
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627659798195/uJxkIZvsE.png)

**QUESTION 6**: Compare emissions from motor vehicle sources in Baltimore City with emissions from motor vehicle sources in Los Angeles County, California (fips == "06037"). Which city has seen greater changes over time in motor vehicle emissions?
``` 
#1. Read and prepare data
NEI <- readRDS("summarySCC_PM25.rds")
library(dplyr)
NEIdp <- tbl_df(NEI)
#table with Motor Vehicle emissions per year of Baltimore
BaltimoraEmissionsVehicle = summarize(group_by(filter(NEIdp, fips=='24510', type=='ON-ROAD'), year), sum(Emissions)) 
#add place name
BaltimoraEmissionsVehicle = mutate(BaltimoraEmissionsVehicle, Place = 'Baltimore City')
#table with Motor Vehicle emissions per year of LA County
LAEmissionsVehicle = summarize(group_by(filter(NEIdp, fips=='06037', type=='ON-ROAD'), year), sum(Emissions))
#add place name
LAEmissionsVehicle = mutate(LAEmissionsVehicle, Place = 'Los Angeles County')
#union of the tables
BaltimoreLA <- rbind(BaltimoraEmissionsVehicle,LAEmissionsVehicle)
#renamed columns
colnames(BaltimoreLA) <- c('Year', 'Emissions', 'Place')
#year converted to string
BaltimoreLA$Year <- as.character(BaltimoreLA$Year)

#2. Plot
png('plot6.png')
qplot(Year,data=BaltimoreLA, geom="bar", weight=Emissions, facets=.~Place, fill=Year, main='Baltimore City/Los Angeles County: Emissions of motor vehicle', xlab='', ylab = 'Emissions (PM 2.5)')
dev.off()
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627659848096/pL_HyeSys.png)

