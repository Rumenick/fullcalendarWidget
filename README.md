
[![Travis-CI Build Status](https://travis-ci.org/CannaData/fullcalendarWidget.svg?branch=master)](https://travis-ci.org/CannaData/fullcalendarWidget) <!-- README.md is generated from README.Rmd. Please edit that file -->

fullcalendarWidget
==================

The goal of fullcalendarWidget is to provide interactive calendars as an HTMLWidget using the [fullcalendar library](https://fullcalendar.io).

Installation
------------

You can install fullcalendarWidget from github with:

``` r
# install.packages("devtools")
devtools::install_github("CannaData/fullcalendarWidget")
```

Example
-------

You can run `fullcalendar` in the console (it works in RStudio at least)

``` r
library(fullcalendarWidget)
# blank calendar of current month
fullcalendar()
# with events
fullcalendar(events = data.frame(title = "Event", date = Sys.Date()))
# with options and callbacks
fullcalendar(options = list(header = list(left = "", center = "", right = "prev,next")),
            callbacks = list(dayClick = DT::JS(
            "function(date, jsEvent, view) {
              alert(date.format());
            }"
            )))
```

You can also use in `shiny`

``` r
library(shiny)
library(fullcalendarWidget)

ui <- fluidPage(textInput("text","Edit Text Here", "Example Text"), fullcalendarOutput("test"))

server <- function(input, output, server) {
  output$test <- renderFullcalendar({
    fullcalendar(
      events = data.frame(
        id = 1:2,
        title = c("Event 1", input$text),
        start = Sys.Date() + 0:1,
        color = c("blue", "orange")
      ),
      options = list(header = list(
        left = "",
        center = "",
        right = "prev,next"
      )),
      callbacks = list(
        dayClick = DT::JS(
          "function(date, jsEvent, view) {
          alert('Clicked on: ' + date.format());
  }"
        )
      )
    )
  })
}

shinyApp(ui, server)
```
