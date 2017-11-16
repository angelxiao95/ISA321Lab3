# ISA321Lab3
#Parta
library(readxl)
library(dplyr)
library(tidyverse)

install.packages("lpSolve")
install.packages("lpSolveAPI")
library(lpSolve)
library(lpSolveAPI)

excel_sheets("Fantasy_Football_Data.xlsx")
FF_wk9 <- read_excel("Fantasy_Football_Data.xlsx", sheet=1, col_names = T)
FF_wk9$Xi <- {}
View(FF_wk9)

colnames(FF_wk9)[which(names(FF_wk9) == "Player Name")] <- "Player_Name"
num.players <- length(FF_wk9$Player_Name)

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
               FF_wk9$Cost,
               FF_wk9$Player_Name == FF_wk9$Player_Name[1],
               FF_wk9$Player_Name == FF_wk9$Player_Name[2],
               FF_wk9$Player_Name == FF_wk9$Player_Name[35],
               FF_wk9$Player_Name == FF_wk9$Player_Name[3],
               FF_wk9$Player_Name == FF_wk9$Player_Name[186],
               FF_wk9$Player_Name == FF_wk9$Player_Name[160],
               FF_wk9$Player_Name == FF_wk9$Player_Name[26],
               FF_wk9$Player_Name == FF_wk9$Player_Name[43],
               FF_wk9$Player_Name == FF_wk9$Player_Name[101])

FF_wk9$Player_Name[2]

matrix_Lp <- matrix(matrix_Lp,ncol = 200,byrow = TRUE)
colnames(matrix_Lp) <- FF_wk9$Player_Name
DF <- as.data.frame(matrix_Lp)

View(matrix_Lp)

direction <- c("==",
               "==",
               "==",
               "==",
               "==",
               "==",
               "<=",
               "==",
               "==",
               "==",
               "==",
               "==",
               "==",
               "==",
               "==",
               "==")

rhs <- c(1, # Quarterbacks
         2, # Running Backs
         3, # Wide Receivers
         1, # Tight Ends
         1, # Defense 
         1, # Flex
         200, #Budget
         0,
         0,
         0,
         0,
         0,
         1,
         1,
         0,
         0)  # Zero out player 

#time to solve 

LP_Solution <- lp ("max", f.obj, matrix_Lp, direction, rhs, all.bin=TRUE)

LP_Solution$objval
LP_Solution$solution

FF_wk9[LP_Solution$solution != 0,]












