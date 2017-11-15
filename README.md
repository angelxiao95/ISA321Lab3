# ISA321Lab3
library(readxl)
library(dplyr)
library(tidyverse)

excel_sheets("Fantasy_Football_Data.xlsx")
FF_wk9 <- read_excel("Fantasy_Football_Data.xlsx", sheet=1, col_names = T)
FF_wk9$Xi <- {}
View(FF_wk9)

num.players <- length(FF_wk9$`Player Name`)

# Optimization
#setting up the problem
f.obj <- FF_wk9$`Projected Points`

var.types <- rep("B", num.players)

matrix_Lp <- c(FF_wk9$QB,
               FF_wk9$rb,
               FF_wk9$Wr,
               FF_wk9$TE,
               FF_wk9$DEF,
               FF_wk9$FLEX,
               FF_wk9$Cost)

matrix_Lp <- matrix(matrix_Lp,ncol = 200,byrow = TRUE)
colnames(matrix_Lp) <- FF_wk9$`Player Name`
DF <- as.data.frame(matrix_Lp)

View(matrix_Lp)

direction <- c("==",
               "==",
               "==",
               "==",
               "==",
               "==",
               "<=")

rhs <- c(1, # Quarterbacks
         2, # Running Backs
         3, # Wide Receivers
         1, # Tight Ends
         1, # Defense
         1, # Flex
         200)  # Budget

#time to solve
install.packages("lpSolve")
install.packages("lpSolveAPI")
library(lpSolve)
library(lpSolveAPI)

LP_Solution <- lp ("max", f.obj, matrix_Lp, direction, rhs, all.bin=TRUE)

?lp

LP_Solution$objval  
LP_Solution$solution

FF_wk9[LP_Solution$solution != 0,]











