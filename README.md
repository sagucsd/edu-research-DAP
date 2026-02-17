# edu-research-DAP
Examining pre and posttest design 
# Introduction 
Educational research often uses pre- and posttest design to measure how well a new curriculum meets specific learning goals. By testing participants before and after instruction, researchers can determine how much knowledge was gained and evaluate both the efficacy of curriculum and instructional practices. This design is considered one of the best ways to assess the overall impact and quality of a curriculum.

This guide uses pre- and posttests to measure the impact of four digital citizenship lessons within Campus Connections, a 12-week mentoring program that paired youth (ages 12–14) with undergraduate student mentors. The digital citizenship lessons included: (1) digital wellness, (2) digital accessibility, (3) securing devices, and (4) digital footprint. In addition to knowledge-based surveys, students also completed the Developmental Assets Profile (DAP) regarding their academic, social, emotional, and psychological wellness. The DAP was completed before the start of programming and again at the end.  

This tutorial serves as an introduction to performing t-tests to compare two groups in R. You will need the `readr`, `psych`, and `ggplot2` packages.

# 1. Evaluating Learning Changes with Paired-sample t-tests 
In order to determine whether youth participants made significant learning gains during digital citizenship lessons, this study utilized social statistics with a pretest–posttest design. Participants (n = 8) completed a pretest prior to beginning each of the four lessons and a posttest at completion of each lesson.  

```
library(readr) # find, import, and read data
library(psych) # descriptive statistics
library(ggplot2) # plotting
library(ggrepel)

DigitalCitizenship <- read_csv("DigitalCitizenship.csv")
View(DigitalCitizenship)

min(DigitalCitizenship$preDAP)
max(DigitalCitizenship$preDAP)
mean(DigitalCitizenship$preDAP)
describe(DigitalCitizenship$preDAP) 

min(DigitalCitizenship$postDAP)
max(DigitalCitizenship$postDAP)
mean(DigitalCitizenship$postDAP)
describe(DigitalCitizenship$postDAP)

df_long <- data.frame(
  ID = rep(DigitalCitizenship$ID, 2),
  value = c(DigitalCitizenship$preDAP, DigitalCitizenship$postDAP),
  time = rep(c("PreDAP", "PostDAP"), each = nrow(DigitalCitizenship))
)
df_long$time <- factor(df_long$time, levels = c("PreDAP", "PostDAP"))
ggplot(df_long, aes(x = time, y = value, fill = time)) +
  geom_dotplot(binaxis = 'y', stackdir = 'center', dotsize = 1.2) + 
  #This magic layer fixes the overlapping and disappearing label issues
  geom_text_repel(aes(label = ID), 
            max.overlaps = Inf, #Forces all labels to appear
            box.padding = 0.5, #Adds space around the text
            point.padding = 0.5, #Keeps text from touching the dots
            size = 3 
            )+
  labs(
    title = "PreDAP vs PostDAP Scores",
    x = NULL,
    y = "DAP Score"
  ) +
  scale_fill_manual(values = c("#E60023", "#45A3FE")) +
  theme_minimal()  
```
