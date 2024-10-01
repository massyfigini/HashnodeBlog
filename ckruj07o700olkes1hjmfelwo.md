---
title: "Maps in R with Leaflet"
datePublished: Sun Apr 02 2017 11:04:13 GMT+0000 (Coordinated Universal Time)
cuid: ckruj07o700olkes1hjmfelwo
slug: maps-in-r-with-leaflet
tags: r, maps, data-visualization

---

Leaflet = JavaScript open source library for interactive maps.  
You can find an example I made [here](https://massyfigini.github.io/Referendum_Course_Project_1.html), the entire code and the data to reproduce it are on [my Github](https://github.com/massyfigini/Developing_Data_Products_CP/tree/master/Course_Project_1).

```sql
#library
library(leaflet)

#empty map
leaflet() %>% addTiles()

#position
my_map %>% 
  addMarkers(lat=45.628255, 
                      lng=9.1464259, 
                      popup="Cesano Maderno")

#more positions
df <- data.frame(lat = runif(20, min = 45.5, max = 45.7),
                 lng = runif(20, min = 9.0, max = 9.2))
df %>%  
  leaflet() %>% 
  addTiles() %>%
  addMarkers()

#circle for markers
df %>% 
  leaflet() %>%
  addTiles() %>%
  addCircleMarkers()

#different dimension based on population
paesi <- data.frame(name = c("Monza", "Muggiò", "Villasanta", "Vimercate"),
         pop = c(122723, 23527, 13894, 25977),
         lat = c(45.5840057, 45.5929507, 45.6055555, 45.614008),
         lng = c(9.2730143, 9.2265162, 9.3033668, 9.3712627))
paesi %>%
  leaflet() %>%
  addTiles() %>%
  addCircles(weight = 1, radius = sqrt(paesi$pop) * 50)

#group markers if close
df %>% 
  leaflet() %>%
  addTiles() %>%
  addMarkers(clusterOptions = markerClusterOptions())

#icon as marker
hopkinsIcon <- makeIcon(
  iconUrl = "http://brand.jhu.edu/content/uploads/2014/06/university.shield.small_.blue_.png",
  iconWidth = 31*215/230, iconHeight = 31,
  iconAnchorX = 31*215/230/2, iconAnchorY = 16
)
hopkinsLatLong <- data.frame(
  lat = c(39.2973166, 39.3288851, 39.2906617),
  lng = c(-76.5929798, -76.6206598, -76.5469683))
hopkinsLatLong %>% 
  leaflet() %>%
  addTiles() %>%
  addMarkers(icon = hopkinsIcon)

#interactive logos
hopkinsSites <- c(
  "<a href='http://www.jhsph.edu/'>East Baltimore Campus</a>",
  "<a href='https://apply.jhu.edu/visit/homewood/'>Homewood Campus</a>",
  "<a href='http://www.hopkinsmedicine.org/johns_hopkins_bayview/'>Bayview Medical Center</a>",
  "<a href='http://www.peabody.jhu.edu/'>Peabody Institute</a>",
  "<a href='http://carey.jhu.edu/'>Carey Business School</a>"
)
hopkinsLatLong %>% 
  leaflet() %>%
  addTiles() %>%
  addMarkers(icon = hopkinsIcon, popup = hopkinsSites)

#rectangle
leaflet() %>%
  addTiles() %>%
  addRectangles(lat1 = 45.5, lng1 = 9.0, 
                lat2 = 45.7 , lng2 = 9.2)

#legend
df <- data.frame(lat = runif(20, min = 45.5, max = 45.7),
                 lng = runif(20, min = 9.0, max = 9.2)),
                 col = sample(c("red", "blue", "green"), 20, replace = TRUE),
                 stringsAsFactors = FALSE)
df %>%
  leaflet() %>%
  addTiles() %>%
  addCircleMarkers(color = df$col) %>%
  addLegend(labels = LETTERS[1:3], colors = c("blue", "red", "green"))
```