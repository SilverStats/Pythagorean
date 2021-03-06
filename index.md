---
title       : Pythagorean Expectation in Baseball
subtitle    : A ShinyApp in R
author      : Steven Silverman
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
theme       : clean
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]     # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Pythagorean Expectation

* Originally developed by Bill James
* Simple formula: $\text{Winning Percentage} = \dfrac{Runs Scored^2}{Runs Scored^2 + Runs Allowed^2}$
* Gives an indicator of how lucky a team was
    - Above expectation: lucky
    - Below expectation: unlucky
* Not a perfect estimator, but usually within 3 or 4 games for the majority of teams
* There are usually a few outliers far from their expectation, like in any distribution    


---

## Accuracy

* Data from Lahman DB (http://www.seanlahman.com/baseball-archive/statistics)


```r
runs <- read.csv("runs.csv"); runs$Pythag <- (runs$R^2)/(runs$R^2 + runs$RA^2)
plot(runs$Pythag, runs$WPct, xlab = "Pythagorean Expectation",
     ylab = "Winning Percentage", main = "WPct vs Pythag since 1955; r = 0.94")
fit <- lm(runs$WPct ~ runs$Pythag); abline(fit, col = "red", lwd = 2)
```

<img src="assets/fig/unnamed-chunk-1.png" title="plot of chunk unnamed-chunk-1" alt="plot of chunk unnamed-chunk-1" style="display: block; margin: auto;" />

---

## Other Forms

* There are close or equivalent forms that some people prefer
* One example: $\dfrac{Wins}{Losses} = \dfrac{Runs Scored^2}{Runs Allowed^2}$
* Dispute over the exponent
    - 2 was James's original choice, and it's the most simplistic
    - 1.83 has been shown to be more accurate in general
    - Other versions (Pythagenport, Pythagenpat) change the exponent for each team
* In the app, I give the user the option of which exponent to use
* The user also inputs the runs scored and runs allowed numbers (default is 700 for both)

---

## Output

* Panel for explanation and user input; separate panel for displaying the user's choices and results
* Winning percentage as well as record over a 162-game season are calculated and displayed
* Sample calculation below for a team that scored 750 runs and allowed 700, using 2 as the exponent


```r
RS <- 750; RA <- 700; exponent <- 2
WPct <- (RS^exponent)/(RS^exponent + RA^exponent)
wins <- WPct * 162; losses <- (1 - WPct) * 162
wins; losses; format(round(WPct, 3), nsmall = 3) # show to 3 decimal places
## [1] 86.58
## [1] 75.42
## [1] "0.534"
```


