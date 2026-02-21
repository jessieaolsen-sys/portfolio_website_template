# Rshiny Tutorial Blog

As important as it is to wrangle data and get results, what use are those results if you can't communicate them? While verbal communication or static markdown is sufficient in certain scenarios, often it is helpful to utilize visuals in an interactive manner, such as through a dashboard, to communicate your findings. 

While there are many options for building an interactive dashboard, the easiest and most effective option when working with R code is **R shiny**. 

In this tutorial you will learn why you should use shiny, how to build a basic shiny app, and different ways you can begin to customize your dashboard. Hopefully after this tutorial

## Benefits of R Shiny

**Benefits as a statistician:** First of all, it is 100% R based and requires no html or any other language. This is helpful when your analysis are coded in R in the first place. In line with this, R shiny can also run complicated r code behind the scenes of your dashboard, giving it heavy computing power. Also, did I mention it's free?

**Benefits to the user:** It is a very user friendly interface to work with once published. The published dashboard itself hides all underwritten code from the user and provides simple interactive plots and calculations. It's very easy to understand for non technical managers or employees for example.

## How to install

In your IDE of choice, run the following code in the console:

*Example 1:*

``` R
install.packages("shiny")
library(shiny)
```
This will install R Shiny and then load it into your current working project.
Delete the first line of code (install.packages) once you've ran it, but leave the second line of code.

## Basic Code Structure

There are two main parts to a shiny app: the server and the ui. The server contains all the instructions to the dashboard behind the scenes, while the ui (user interface) controls what the user sees on the published dashboard.

### UI

The ui part of your app will be assigned as fluidPage. This is the one essential part of the ui code and controls the layout of the dashboard.

*Example 2:*
```R
ui <- fluidPage()
```
Within the fluidPage function the main components you can give a single paged dashboard include a title, main page, and sidebar. These will be the parameters you give fluidPage, coded as seen below.

*Example 3:*
```R
ui <- fluidPage(
    titlePanel('Title'),
    sidebarLayout(
        sidebarPanel()
    ),
    mainPanel()
)
```
When adding a component to your dashboard, nest it inside the section (function) you want it in. For example, if I want a slider in the sidebar of my dashboard, inside the parenthesis of sidebarPanel, I will include sliderInput(). See the following table for examples of ui functions and what they do.

|Function        |Where to use        |What it does                     |
|----------------|--------------------|---------------------------------|
|sliderInput()   |within sidebarLayout|creates an interactive slider    |
|dateRangeInput()|within sidebarLayout|lets users input a range of dates|
|selectInput()   |within sidebarLayout|creates a drop down menu         |
|plotOutput()    |within mainPanel    |displays a plot                  |
|textOutput()    |within mainPanel    |displays text                    |

Here is an example of a ui using each of these functions.

*Example 4:*
```R
ui <- fluidPage(
  titlePanel("Labor Predictor"),
  
  sidebarLayout(
    sidebarPanel(
      sliderInput(inputId = "reservations",
                  label = "Number of Reservations:",
                  min = 0,
                  max = 20,
                  value = 0),
      dateRangeInput(inputId = "dates",
                  label = "Select a date range",
                  start = Sys.Date(),
                  end = Sys.Date() + 6),
      selectInput(inputId = "location",
                  label = "Select Location",
                  c('Salt Lake', 'Dallas', 'Boston'))
    ),
  
    mainPanel(
      
      h3("Predicted Sales/Budget"), #h3 just makes a medium sized header (1 would be large 6 would be small)
      plotOutput("model_plot"),
      
      h3("Total Predictions"),
      textOutput("prediction")
    )
  )
)
```
### Server

The server is going to be assigned a function of input and output. This function interacts with the 'input' in the ui, and outputs the corresponding updates plots and calculations. The most basic set up looks like this:

*Example 5:*
```R
server <- function(input, output) {}
```
When writing code in the server you can refer to the input id's of the ui functions. For example, if I wanted to output a simple plot based on the locations dates and reservations in example 3, I might code it like this. 

*Example 6:*
```R
server <- function(input, output) {

  output$prediction <- renderText({
    paste("You selected", input$reservations, "reservations in", input$location,
          "from", input$dates[1], "to", input$dates[2])
  })

  output$model_plot <- renderPlot({
    barplot(
      height = input$reservations,
      names.arg = input$location,
      main = "Reservations (simple demo)",
      ylab = "Reservations"
    )
  })
}
```
Looking at example 6, you can see that output$prediction and output$model_plot come from the output functions in the mainPanel of example 4. Similarly, the input$'s pull from the input functions of the sidebarPanel in example 4. The renderPlot and renderText functions simply allow you to display plots and text in the dashboard.

### Putting it together

To finally put together your completed shiny app and launch it in your IDE, you simply run the line:

*Example 7:*
```R
shinyApp(ui, server)
```
The most basic functioning shiny app would look something like this:
*Example 8:*
```R
library(shiny)

ui <- fluidPage(
  "Hello, Shiny!"
)

server <- function(input, output) {}

shinyApp(ui, server)
```
Putting together the code used in examples 4 and 6, you get a functional (though maybe unhelpful) dashboard that looks like the below:

*image 1*

<img src="Screenshot (71).png" alt="R Shiny app visual" height="250">

## Launching your app

Once you have finished your app, you can publish it for free as a website using shinyapps.io. There are other methods for launching you app, such as through Posit Connect or a self-hosted shiny server, but shinyapps.io is the most beginner friendly. Simply press "publish" in the top right of your app preview (as seen in image 1) and follow the instructions to publish your interactive dashboard

## Next Steps

You now have the foundational tools to build a basic R Shiny app. You can create both plots and text output that changes with custom user input. Now it's time to try it on your own! Practice creating a basic app using your own data and models adjusted based on user input.

Once you're comfortable with these skills you can explore apps with multiple pages, tabs within pages, or more complicated outputs than pasted text. You've just unlocked the ability to build interactive webpages with just some simple r code. Now's the time to explore what you can do and build something with real data!

To learn more about R Shiny visit their [documentation page](https://shiny.posit.co/r/getstarted/shiny-basics/lesson1/).





