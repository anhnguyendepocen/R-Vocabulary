# Quantifiers in R

[R Vocab Topics](index) &#187; [Character Strings](characters) &#187; [Regular Expressions](regex) &#187; [Regex Syntax](regex_syntax) &#187; Quantifiers

When we want to match a **certain number** of characters that meet a certain criteria we can apply quantifiers to our pattern searches.  The quantifiers we can use are:

<center>
<img src="images/quantifier.png" alt="Quantifiers in R">
</center>    


<br>

The following provides examples to show how to use the quantifier syntax to match a **certain number** of characters patterns:


```r
x <- "I like beer! #beer, @wheres_my_beer, I like R (v3.2.2) #rrrrrrr2015"

grep(pattern = "z+", state.name, value = TRUE)
## [1] "Arizona"

grep(pattern = "s{2}", state.name, value = TRUE)
## [1] "Massachusetts" "Mississippi"   "Missouri"      "Tennessee"

grep(pattern = "s{1,2}", state.name, value = TRUE)
##  [1] "Alaska"        "Arkansas"      "Illinois"      "Kansas"       
##  [5] "Louisiana"     "Massachusetts" "Minnesota"     "Mississippi"  
##  [9] "Missouri"      "Nebraska"      "New Hampshire" "New Jersey"   
## [13] "Pennsylvania"  "Rhode Island"  "Tennessee"     "Texas"        
## [17] "Washington"    "West Virginia" "Wisconsin"
```
