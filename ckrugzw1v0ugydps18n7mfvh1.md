## Data Science Specialization Capstone Project (2/2): The Next Word App

This is my Capstone Project for the [Data Science Specialization by Johns Hopkins University on Coursera](https://www.coursera.org/specializations/jhu-data-science).  
The goal of the Capstone Project was to implement a useful model for predict a word given one or more words and to develop a web app to use it.  
In [the last post](https://massyfigini.hashnode.dev/data-science-specialization-capstone-project-12-the-algorithm) you can find how I created the tables used for this app that I will use in this page.  
Below there is the code for the app written using R Studio and Shiny, in addiction to R there is a bit of HTML and CSS.  
You can find the entire code on [my Github](https://github.com/massyfigini/NextWordApp_CapstoneProject).  
You can try the app [here](https://massyfigini.shinyapps.io/nextwordappcp/).  

---------------------------------------------------------------------------------------------------

### ui.R file

```
library(shiny)

shinyUI(fluidPage(
  
  tags$head(tags$style(
    HTML('
         #word {
            color: gold;
            font-size: 300%;
         }
         #others {
            color: gold;
            font-size: 200%;
         }
        tabPanel{ 
          font-family: "Calibri";
          color: gold;
        }
        body, label, input, button, select { 
          font-family: "Calibri";
          background: #000000 url("http://massyfigini.github.io/assets/css/images/DocStrange.jpg")  bottom left;
          background-position: center center;
          background-repeat: no-repeat;
          background-attachment: fixed;
          background-size: cover;
        }'))),
  
  titlePanel(HTML("<font size=10 color=white><b><center>The Next Word App</center></b></font><br/>"),windowTitle="The Next Word App"),
  
  sidebarLayout(
    sidebarPanel(tags$style(".well {background-color:#d3d3d3;}"),
     tabsetPanel(
       
       # First tab (input)
       tabPanel(HTML("<font color=black>Input</font>"),
                textInput("Word", NULL, 
                          placeholder = "Write the words here...")
                ,actionButton("go","Go!", icon = icon("plane"))
                ,HTML("<br/><br/><br/><a href=http://www.massimilianofigini.com>&copy;massyfigini</a>")),
       
       # Second tab (Instruction)
       tabPanel(HTML("<font color=black>About the app</font>"),
                HTML("<br/><b>Instruction</b><br/>
This app predict the next word given one or more words.<br/>
In the input tab, you have to write one or more words and press the 'Go!' button, and the app will predict the next word.<br/>
<br/><b>Data</b><br/>
The English 'Corpora' data are the starting point for the algorithm, you can find more information 
<a href=https://web-beta.archive.org/web/20160930083655/http://www.corpora.heliohost.org/aboutcorpus.html>here</a>.
<br/><br/><b>Algorithm</b><br/>
The algorithm is created starting from the 'Corpora' data. The data are first divided in sentences, than I have
made the bigram, trigram and 4-gram data.
Every words you insert, the algorithm choose the most probabilities next word. You can find more information 
<a href=http://rpubs.com/massyfigini/NextWordApp>here</a>.
<br/><br/><a href=http://www.massimilianofigini.com>&copy;massyfigini</a>")))
      
    ,width=4
    ),
    
    mainPanel(
      HTML("<font size=5 color=white>Top probability next word</font><br/>"),
      htmlOutput("word"),
      HTML("<br/><br/>"),
      HTML("<font size=5 color=white>Other possibly words</font><br/>"),
      htmlOutput("others")
      )
  )
))
``` 


### server.R file
``` 
library(shiny)
library(dplyr)
load("WordBigram.RData")
load("WordTrigram.RData")
load("WordQuadrigram.RData")

Found <- 'N'
virgola <- ',\n'

shinyServer(function(input, output) {
  
  observeEvent(input$go, {
    a <- tolower(input$Word)
    a <- unlist(strsplit(a, " ", fixed=TRUE))

    # Algorithm
    if(length(a) > 2) {
      # more then 2 words: first in quadrigram, then trigram, then bigram
      c <- paste(a[length(a)-2], a[length(a)-1], a[length(a)])
      Next <- WordQuadrigram %>% filter(Start == c) %>% select(First,Second,Third)
      # search in trigram
      if(nrow(Next) == 0) {
        b <- paste(a[length(a)-1], a[length(a)])
        Next <- WordTrigram %>% filter(Start == b) %>% select(First,Second,Third)
        if(nrow(Next) == 0) {
          # word not found, search in bigram
          z <- a[length(a)]
          Next <- WordBigram %>% filter(Start == z) %>% select(First,Second,Third)
          if(nrow(Next) == 0) {
            # word not found in bigram
            Found <- 'N'
          } else {
            # found in bigram
            Found <- 'B'
            B1 <- Next[1]
            B2 <- Next[2]
            B3 <- Next[3]
          }
        } else {
          # found in trigram
          Found <- 'T'
          T1 <- Next[1]
          T2 <- Next[2]
          T3 <- Next[3]
          # search also in bigram
          z <- a[length(a)]
          Next <- WordBigram %>% filter(Start == z) %>% select(First,Second,Third)
          B1 <- Next[1]
          B2 <- Next[2]
          B3 <- Next[3]
        }
        # found in quadrigram
      } else {
        Found <- 'Q'
        Q1 <- Next[1]
        Q2 <- Next[2]
        Q3 <- Next[3]
        # search also in bigram
        z <- a[length(a)]
        Next <- WordBigram %>% filter(Start == z) %>% select(First,Second,Third)
        B1 <- Next[1]
        B2 <- Next[2]
        B3 <- Next[3]
      }
      
    } else if(length(a) == 2) {
      # if are two, search in trigram first
      b <- paste(a[1], a[2])
      Next <- WordTrigram %>% filter(Start == b) %>% select(First,Second,Third)
      if(nrow(Next) == 0) {
        # word not found, search in bigram
        z <- a[length(a)]
        Next <- WordBigram %>% filter(Start == z) %>% select(First,Second,Third)
        if(nrow(Next) == 0) {
          # word not found
          Found <- 'N'
        } else {
          # found in bigram
          Found <- 'B'
          B1 <- Next[1]
          B2 <- Next[2]
          B3 <- Next[3]
        }
      } else {
        # found in trigram
        Found <- 'T'
        T1 <- Next[1]
        T2 <- Next[2]
        T3 <- Next[3]
        # found also in bigram
        z <- a[length(a)]
        Next <- WordBigram %>% filter(Start == z) %>% select(First,Second,Third)
        B1 <- Next[1]
        B2 <- Next[2]
        B3 <- Next[3]
      }
      
    } else {
      # if only one go here
      z <- a[1]
      Next <- WordBigram %>% filter(Start == z) %>% select(First,Second,Third)
      if(nrow(Next) == 0) {
        # word not found
        Found <- 'N'
      } else {
        # found
        Found <- 'B'
        B1 <- Next[1]
        B2 <- Next[2]
        B3 <- Next[3]
      }
    }

    
    output$word <- renderPrint({
      if(Found == 'N') {
      HTML("<font size=5 color=red>Next word not found!</font>")
      } else if(Found == 'B' ) {
        print(unname(B1), row.names=FALSE)
      } else if (Found == 'T'){
        print(unname(T1), row.names=FALSE)
      } else if (Found == 'Q'){
        print(unname(Q1), row.names=FALSE)
      }
    })
  
  
    output$others <- renderPrint({
      if(Found == 'N') {
        HTML("<font size=5 color=red>Words not found!</font>")
      } else if(Found == 'B') {
        print(unname(B2), row.names=FALSE)
        cat(",\n")
        print(unname(B3), row.names=FALSE)
      } else if(Found == 'T'){
        if(as.character(T1$First[1]) != as.character(B1$First[1])) {
          print(unname(B1), row.names=FALSE)
          cat(",\n")
        }
        if(as.character(T1$First[1]) != as.character(B2$Second[1])) {
          print(unname(B2), row.names=FALSE)
          cat(",\n")
        }
        if(as.character(T1$First[1]) != as.character(B3$Third[1])) {
          print(unname(B3), row.names=FALSE)
        }
      } else if(Found == 'Q'){
        if(as.character(Q1$First[1]) != as.character(B1$First[1])) {
          print(unname(B1), row.names=FALSE)
          cat(",\n")
        }
        if(as.character(Q1$First[1]) != as.character(B2$Second[1])) {
          print(unname(B2), row.names=FALSE)
          cat(",\n")
        }
        if(as.character(Q1$First[1]) != as.character(B3$Third[1])) {
          print(unname(B3), row.names=FALSE)
        }
      }
    })
    
  })
})
```