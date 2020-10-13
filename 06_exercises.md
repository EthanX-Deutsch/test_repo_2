---
title: 'Weekly Exercises #6'
author: "Ethan Deutsch"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(googlesheets4) # for reading googlesheet data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(gifski)        # for creating the gif (don't need to load this library every time,but need it installed)
library(transformr)    # for "tweening" (gganimate)
library(shiny)         # for creating interactive apps
library(patchwork)     # for nicely combining ggplot2 graphs  
library(gt)            # for creating nice tables
library(rvest)         # for scraping data
library(robotstxt)     # for checking if you can scrape data
gs4_deauth()           # To not have to authorize each time you knit.
theme_set(theme_minimal())
```


```r
# Lisa's garden data
garden_harvest <- read_sheet("https://docs.google.com/spreadsheets/d/1DekSazCzKqPS2jnGhKue7tLxRU3GVL1oxi-4bEM5IWw/edit?usp=sharing") %>% 
  mutate(date = ymd(date))

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Your first `shiny` app 

  1. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
  Link: https://exdeutsch-shiny.shinyapps.io/app_files/
  
## Warm-up exercises from tutorial

  2. Read in the fake garden harvest data. Find the data [here](https://github.com/llendway/scraping_etc/blob/main/2020_harvest.csv) and click on the `Raw` button to get a direct link to the data. 
  

```r
fake_harvest_data <- read_csv("https://raw.githubusercontent.com/llendway/scraping_etc/main/2020_harvest.csv", 
    col_types = cols(X1 = col_skip(), X5 = col_number(), 
        weight = col_number()), na = "MISSING", 
    skip = 2)
```

  
  3. Read in this [data](https://www.kaggle.com/heeraldedhia/groceries-dataset) from the kaggle website. You will need to download the data first. Save it to your project/repo folder. Do some quick checks of the data to assure it has been read in appropriately.

  4. Write code to replicate the table shown below (open the .html file to see it) created from the `garden_harvest` data as best as you can. When you get to coloring the cells, I used the following line of code for the `colors` argument:

  5. Create a table using `gt` with data from your project or from the `garden_harvest` data if your project data aren't ready.
  

```r
garden_harvest_july <- garden_harvest %>%
  filter(month(date, label = TRUE) %in% "Jul") %>%
  filter(vegetable %in% "lettuce")

harvest_table <- 
  garden_harvest_july %>%
  gt(
    rowname_col = "Vegetable",
    groupname_col = "Weight"
  ) %>%
  fmt_date(
    columns = vars(date),
    date_style = 6
  ) %>%
  cols_hide(
    columns = vars(units)
    ) %>%
  tab_header(
    title = "The Weight of Lettuce",
    subtitle = md("A look at the weight of different types of lettuce picked in July")
  ) %>%
  tab_options(column_labels.background.color = "gold")

harvest_table
```

<!--html_preserve--><style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#cagrmyupuw .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#cagrmyupuw .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#cagrmyupuw .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#cagrmyupuw .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#cagrmyupuw .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#cagrmyupuw .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#cagrmyupuw .gt_col_heading {
  color: #333333;
  background-color: gold;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#cagrmyupuw .gt_column_spanner_outer {
  color: #333333;
  background-color: gold;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#cagrmyupuw .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#cagrmyupuw .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#cagrmyupuw .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#cagrmyupuw .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#cagrmyupuw .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#cagrmyupuw .gt_from_md > :first-child {
  margin-top: 0;
}

#cagrmyupuw .gt_from_md > :last-child {
  margin-bottom: 0;
}

#cagrmyupuw .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#cagrmyupuw .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#cagrmyupuw .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#cagrmyupuw .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#cagrmyupuw .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#cagrmyupuw .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#cagrmyupuw .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#cagrmyupuw .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#cagrmyupuw .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#cagrmyupuw .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#cagrmyupuw .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#cagrmyupuw .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#cagrmyupuw .gt_left {
  text-align: left;
}

#cagrmyupuw .gt_center {
  text-align: center;
}

#cagrmyupuw .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#cagrmyupuw .gt_font_normal {
  font-weight: normal;
}

#cagrmyupuw .gt_font_bold {
  font-weight: bold;
}

#cagrmyupuw .gt_font_italic {
  font-style: italic;
}

#cagrmyupuw .gt_super {
  font-size: 65%;
}

#cagrmyupuw .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>
<div id="cagrmyupuw" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;"><table class="gt_table">
  <thead class="gt_header">
    <tr>
      <th colspan="4" class="gt_heading gt_title gt_font_normal" style>The Weight of Lettuce</th>
    </tr>
    <tr>
      <th colspan="4" class="gt_heading gt_subtitle gt_font_normal gt_bottom_border" style>A look at the weight of different types of lettuce picked in July</th>
    </tr>
  </thead>
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">vegetable</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">variety</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">date</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">weight</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Farmer's Market Blend</td>
      <td class="gt_row gt_left">Jul 1, 2020</td>
      <td class="gt_row gt_right">60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Tatsoi</td>
      <td class="gt_row gt_left">Jul 2, 2020</td>
      <td class="gt_row gt_right">144</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Farmer's Market Blend</td>
      <td class="gt_row gt_left">Jul 3, 2020</td>
      <td class="gt_row gt_right">217</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Tatsoi</td>
      <td class="gt_row gt_left">Jul 3, 2020</td>
      <td class="gt_row gt_right">216</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Farmer's Market Blend</td>
      <td class="gt_row gt_left">Jul 4, 2020</td>
      <td class="gt_row gt_right">147</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Tatsoi</td>
      <td class="gt_row gt_left">Jul 6, 2020</td>
      <td class="gt_row gt_right">189</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Farmer's Market Blend</td>
      <td class="gt_row gt_left">Jul 7, 2020</td>
      <td class="gt_row gt_right">67</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Farmer's Market Blend</td>
      <td class="gt_row gt_left">Jul 7, 2020</td>
      <td class="gt_row gt_right">13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Farmer's Market Blend</td>
      <td class="gt_row gt_left">Jul 8, 2020</td>
      <td class="gt_row gt_right">39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Tatsoi</td>
      <td class="gt_row gt_left">Jul 8, 2020</td>
      <td class="gt_row gt_right">75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Farmer's Market Blend</td>
      <td class="gt_row gt_left">Jul 9, 2020</td>
      <td class="gt_row gt_right">61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Farmer's Market Blend</td>
      <td class="gt_row gt_left">Jul 11, 2020</td>
      <td class="gt_row gt_right">79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Farmer's Market Blend</td>
      <td class="gt_row gt_left">Jul 12, 2020</td>
      <td class="gt_row gt_right">83</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Farmer's Market Blend</td>
      <td class="gt_row gt_left">Jul 13, 2020</td>
      <td class="gt_row gt_right">53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Tatsoi</td>
      <td class="gt_row gt_left">Jul 13, 2020</td>
      <td class="gt_row gt_right">137</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Farmer's Market Blend</td>
      <td class="gt_row gt_left">Jul 16, 2020</td>
      <td class="gt_row gt_right">61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Tatsoi</td>
      <td class="gt_row gt_left">Jul 20, 2020</td>
      <td class="gt_row gt_right">123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Lettuce Mixture</td>
      <td class="gt_row gt_left">Jul 22, 2020</td>
      <td class="gt_row gt_right">23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Lettuce Mixture</td>
      <td class="gt_row gt_left">Jul 23, 2020</td>
      <td class="gt_row gt_right">130</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Lettuce Mixture</td>
      <td class="gt_row gt_left">Jul 24, 2020</td>
      <td class="gt_row gt_right">16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Lettuce Mixture</td>
      <td class="gt_row gt_left">Jul 26, 2020</td>
      <td class="gt_row gt_right">81</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Lettuce Mixture</td>
      <td class="gt_row gt_left">Jul 27, 2020</td>
      <td class="gt_row gt_right">99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Lettuce Mixture</td>
      <td class="gt_row gt_left">Jul 28, 2020</td>
      <td class="gt_row gt_right">91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Lettuce Mixture</td>
      <td class="gt_row gt_left">Jul 29, 2020</td>
      <td class="gt_row gt_right">73</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Lettuce Mixture</td>
      <td class="gt_row gt_left">Jul 30, 2020</td>
      <td class="gt_row gt_right">94</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_left">Lettuce Mixture</td>
      <td class="gt_row gt_left">Jul 31, 2020</td>
      <td class="gt_row gt_right">107</td>
    </tr>
  </tbody>
  
  
</table></div><!--/html_preserve-->
  
  
  6. Use `patchwork` operators and functions to combine at least two graphs using your project data or `garden_harvest` data if your project data aren't read.
  

```r
graph_1 <- garden_harvest %>%
  filter(vegetable %in% c("lettuce", "carrots", "tomatoes", "peas")) %>%
  group_by(vegetable, variety) %>%
  summarise(avg_weight = mean(weight)) %>%
  ggplot(aes(x = avg_weight, y = fct_reorder(variety, avg_weight, .desc = FALSE), fill = vegetable)) +
  geom_col() +
  labs(x = "Average Weight (g)", y = "", title = "Average Weight of Vegetables by Variety")

graph_2 <- garden_harvest %>%
  filter(vegetable %in% c("lettuce", "carrots", "tomatoes", "peas")) %>%
  group_by(vegetable, variety, date) %>%
  summarise(daily_harvest = sum(weight)) %>%
  mutate(cumulative_daily = cumsum(daily_harvest)) %>%
  ggplot(aes(x = cumulative_daily, y = fct_reorder(variety, cumulative_daily, .desc = FALSE), color = vegetable)) +
  geom_boxplot() +
  labs(x = "Cumulative Totals (g)", y = "", title = "Cumulative Total Weight of Vegetables by Variety")

(graph_1 / graph_2)
```

![](06_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
  
  
  7. COMING SOON! Web scraping problem.

  
**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
