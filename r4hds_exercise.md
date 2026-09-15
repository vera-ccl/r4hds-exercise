HDAT9700 Statistical Modelling II
================
Mark Hanly

Knit before you commit and push, you will see the .Rmd and .md file,
push both to hand in the assignment!

# Overview

The examples and exercises in this document are designed to help you you
revise some cores skills for using R for Health Data Science, in
particular

1.  Literate Programming
2.  Visualising data

Both of these skills will be essential in this course, to complete your
assessments, and in the workplace to effectively undertake and
communicate your analyses.

This is an R Markdown (.Rmd) file, which allows you to knit together
text and code to produce a nicely formatted document. Many different
output formats are possible ,e.g. `.docx`, `.pdf`, and `.html`. To run
an R Markdown document click the `knit` button in RStudio. You should
see the rendered output appear in the RStudio Viewer pane. As you read
this document you should look at both the raw code (this .Rmd file) and
the rendered output, and regularly knit to make sure things are
appearing as you expect.

# 1. Literate Programming

When you write in R Markdown, you’re working in a format that blends
text and code. The text is written in Markdown, while the code is
written in fenced sections we call code chunks.

In R Markdown, the YAML header is the section at the very top of your
document, enclosed by triple dashes (—). It defines the metadata and
configuration for your report, such as the title, author, date, and the
output format (HTML, PDF, Word, etc). You can also specify
bibliographies, table of contents settings, theme options, and other
global parameters in the YAML. Essentially, it acts as the “control
panel” for your document, letting you set options that affect the entire
report before any code or narrative content appears.

## Exercise (1 of 4)

1.  Try editing the `author` metadata field for this document to your
    own name.
2.  Try adding a new metadata field for date underneath `author`. For
    example `date: "15 September 2025"`

## Markdown

Markdown provides a simple syntax that allows us to format text, add
structure through headings quotes, and other features,

### Text formatting

- Italic — wrap text in single asterisks or underscores: *text* or
  *text*

- Bold — double asterisks or underscores: **text** or **text**

- Strikethrough — double tildes: ~~text~~

- Inline code — backticks: `text`

- Superscript<sup>2</sup> — x<sup>2</sup> (Pandoc extension)

- Subscript2 — H<sub>2</sub>O (Pandoc extension)

## Structure

- Headings: \#, \##, \### … for levels 1–6

- Paragraphs (blank line separates them)

- Blockquotes: \> quoted text

- Horizontal rules: \*\*\*

### Lists

- Unordered list: - item or \* item

- Ordered list: 1. item

- Nested lists by indenting with spaces

## Links & media

- Links: [text](https://example.com)

- Images: ![alt text](images/formatted-text.png)

## Exercise (2 of 4)

Try formatting the plain text below to match the formatting in the image

The Role of Health Data Science  
Health data science is a multidisciplinary field that transforms raw
data into meaningful insights. It combines statistics, computing, and
domain knowledge to improve health outcomes.  
A data scientist is part statistician, part computer scientist, and part
storyteller — someone who turns data into decisions.

Why It Matters  
Early diagnosis — spotting hidden patterns in patient data.  
Predictive modelling — forecasting hospital admissions and resource
use.  
Evidence sharing — platforms like Our World in Data make global health
information accessible.

Tools and Methods  
Programming languages: R, Python  
Visualization techniques: interactive dashboards, static reports  
Reproducibility with R Markdown and Quarto

Looking Ahead  
The future of health data science will rely on:  
Outdated methods replaced by adaptive algorithms  
Integration of real-time health monitoring Greater transparency through
open science

------------------------------------------------------------------------

## Code chunks

A code chunk is introduced with three backticks, followed by {r}, and
then closed with three backticks. For example:

```` markdown
```{r demo}

2 + 2

```
````

You can type this out manually in your `.Rmd` file, or you can create a
new code black by clicking on the <img src='images/embed.png'> button.

You can give a chunk a name immediately after the r. In the example
above **demo** is the name. Naming code chunks is optional but can be
helpful for cross-referencing and debugging. You might notice that I am
being a bit lazy and haven’t named my code chunks in this document.
That’s not the end of the world because it is a relative short doc, but
if you were doing a detailed, sophisticated analysis it is a good idea.

If you look at the rendered version of this document you will see that
by default both the R code and the output of the code are printed.
Sometimes we want to show the code and the outcome but not always. If
the code is long, and not particularly relevant to the reader, we might
want to suppress that. Similarly, if the output is messy we might want
to exclude that.

Code chunk options are used to control what is displayed. For example,
we could set `echo=FALSE` to hide the R code, and just return the
result:

    ## [1] 4

To show the code but suppress the output we can use the `results='hide'`
option, as follows:

``` r
2 + 2
```

### Display options

- echo = TRUE/FALSE → show or hide the R code.

- eval = TRUE/FALSE → run or skip the code.

- include = TRUE/FALSE → include both code and results (set FALSE to run
  code but show nothing).

- results = “markup” \| “asis” \| “hide” → control text output.

- collapse = TRUE/FALSE → collapse source and output together.

- comment = “\#\>” → prefix for output lines.

### Messages and warnings

- message = TRUE/FALSE → show or suppress messages.

- warning = TRUE/FALSE → show or suppress warnings.

- error = TRUE/FALSE → allow errors to be shown (and continue
  rendering).

### Figures

- fig.width, fig.height → size in inches.

- fig.align = “left” \| “center” \| “right” → alignment.

- fig.cap = “caption text” → figure caption.

- out.width, out.height → output size (e.g. “70%”).

## Exercise (3 of 4)

Try updating the chunk options to (i) hide the code, (ii) suppress all
warnings, and (iii) plot the figure at 75% of the page width.

``` r
library(palmerpenguins)
library(ggplot2)

ggplot(
  data = penguins, 
  aes(x = bill_depth_mm, fill = species)) + 
  geom_density(alpha = 0.8) + 
  scale_fill_brewer(type = 'qual')
```

    ## Warning: Removed 2 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](r4hds_exercise_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

------------------------------------------------------------------------

# 2. Visualising data

Data visualisation is an important first step in all modelling analyses.
Visualising data is the best way to understand the distribution of
variables, and the relationships between them. When you undertake
exploratory data analysis, consider the following steps for the key
variables in your analyses:

1.  Univariate (the distribution of single variables)
2.  Bivariate (the relationship between pairs of variables)
3.  Multivariate (the relationship between 3 or more variables)

The type of chart that is most appropriate depends on the type of data
you are plotting (e.g. continuous, ordinal, categorical), and the number
of variables. If you are not sure what type of chart to plot, check out
this excellent decision tree [from data to
vis](https://www.data-to-viz.com/)

## Univariate plots

### A histogram (1 numeric variable)

``` r
p1 <- ggplot(
  data = penguins,
  aes(x = bill_depth_mm)) +
  geom_histogram()

p1
```

![](r4hds_exercise_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

### A boxplot (1 numeric variable)

``` r
p2 <- ggplot(
  data = penguins,
  aes(x = bill_depth_mm)) +
  geom_boxplot()

p2
```

![](r4hds_exercise_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

### A bar plot (1 categorical variable)

``` r
p3 <- ggplot(
  data = penguins,
  aes(x = species, y = after_stat(prop), group = 1)) +
  geom_bar() +
  geom_text(
    aes(label = scales::percent(after_stat(prop), accuracy = 1)),
    stat = "count",
    vjust = 2,
    color = 'whitesmoke',
    size = 6
  )

p3
```

![](r4hds_exercise_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

## Bivariate plots

### A grouped density plot (1 numeric and 1 categorical variable)

``` r
p4 <- ggplot(
  data = penguins,
  aes(x = bill_depth_mm, color = species, fill = species)) +
  geom_density(alpha = 0.8)

p4
```

![](r4hds_exercise_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

### A grouped boxplot (1 numeric and 1 categorical variable)

``` r
p5 <- ggplot(
  data = penguins,
  aes(x = species, y = bill_depth_mm)) +
  geom_boxplot()

p5
```

![](r4hds_exercise_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

### A scatterplot (2 numeric variables)

``` r
p6 <- ggplot(
  data = penguins,
  aes(x = body_mass_g, y = bill_length_mm)) +
  geom_point() 

p6
```

![](r4hds_exercise_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

## Multivariate plots

### A grouped scatter plot (2 numeric variables and 1 categorical variable)

``` r
p7 <- ggplot(
  data = penguins,
  aes(x = body_mass_g, y = bill_length_mm, color = species)) +
  geom_point(shape = 21, fill = 'white') # shape = 21 gives hollow circles

p7
```

![](r4hds_exercise_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

### A facetted scatter plot (2 numeric variables and 1 categorical variable)

``` r
p8 <- ggplot(
  data = penguins,
  aes(x = body_mass_g, y = bill_length_mm)) +
  geom_point(shape = 21, fill = 'white') +
    facet_wrap(~species)

p8
```

![](r4hds_exercise_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

## Combining plots

``` r
library(ggpubr)

ggarrange(p1, p2, p3, p4, labels = 'AUTO')
```

![](r4hds_exercise_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

## Labels

The charts above are fine for quick exploration, but if you are sharing
charts you should always include proper labels to guide the reader. For
`ggplot2` charts we can do this using the `labs()` function.

- Title (and subtitle if necessary). Note that often the title will be
  in the text of the report or article, rather than directly on the
  chart.
- Properly formatted axis titles, including units. Don’t use the default
  variables names, for example write out **Bill Depth (mm)** rather than
  **bill_depth_mm**.

``` r
p9 <- ggplot(
  data = penguins,
  aes(x = body_mass_g, y = bill_length_mm, color = species, shape = species)) +
  geom_point() + 
  labs(
    title = "The relationship between body mass and bill length in adult foraging penguins",
    x = "Body mass (g)",
    y = "Bill length (mm)",
    caption = "Data source: Palmer Penguins R Package"
  )

p9
```

![](r4hds_exercise_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

## Exercise (4 of 4)

Drawing off the examples above, can you recreate the chart shown here?

![](images/vis-exercise.png)

------------------------------------------------------------------------
