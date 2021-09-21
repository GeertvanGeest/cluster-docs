*In progress*

This document describes the best practices for code writing at IBU. The main
objective is to write clean code that is readable.

## Global Rules

1. Do not exceed line width of 80 characters. Add a line to your editor of 
choice.

2. DRY (Don't repeat yourself). If you write the same code more than two times,
consider writing a function.

3. Wrtie modular code which is organised in different files.

4. Explicit code is better than implicit code.

## R

We refer to the style guide from [tidyverse](https://style.tidyverse.org/) 
for a detailed explanation of accepted styles. 
Some of the most important style recommendations are listed here:
	
### Objects

#### Naming objects

As a general rule, we use `_` for separating words in object names. Object 
names should be nouns.

Good:

	mean_sepal_length <- ...

Bad:

	mean.sepal.lengths <- ...

The reason why a `.` can be a confusing delimiter for variable names
 is because it resembles the notation of S3 objects.

### Functions

#### Naming functions

Function names should be meaningful and give an understanding of what the 
functions does. Use verbs for functions

Good:

	calculate_mean()
	permute()

Bad:

	mean_calculator()
	permuter()


#### Long functions

If functions have many (longish) arguments, indent the second line
where the definition starts:

	calculate_mean <- function(x = "First long argument",
				   y = "second long argument",
				   z = "third long argument"){
	}

#### The curly braces

The closing curly braces belong to a new line.

#### Explaining Functions

If the functions are rather complicated (which is most often the case),
write down what the function does, explain what arguments are needed and
what the function returns.

	calculate_nt_identity <- function(sequence_1,
					  sequence_2,
					  method){
	# Function aligns two sequences and calculates the nucleotide identity 
	#  score.
	# Args:
	#  sequence_1: A DNA sequence of class DnaSeq.  
	#  sequence_2: A DNA sequence of class DnaSeq.
	#  method: alignment method ("method1", "method2")
	# Returns: A number between 0 and 1.

	   <code that does what the function claims>

	}

### Pipes

The pipe operator `%>%` should be preceeded by a space followed by a new line.

Good:

	iris %>% 
		group_by(Species) %>%
		summarize_if(is.numeric, mean) %>%
		ungroup() %>%
		gather(measure, value, -Species) %>%
		arrange(value)

Bad:

	iris %>% group_by(Species)%>% summarize_if(is.numeric, mean) %>%
		ungroup() %>%gather(measure, value, -Species) %>%
		arrange(value)

### Rmarkdown

Rmarkdown is a great tool for presenting/documenting research while including
code.

However, Rmarkdown files can quickly become very long and hard-to-read if
text is interrupted by large code chunks. Therefore, it is recommended to
source code from external files which contain functions.

#### Sourcing .R files

```
```{r, include=FALSE}
source("path/to/script.R", local= knitr::knit_global())
\```
```

#### Sourcing chunks


## Python

*in progress*

