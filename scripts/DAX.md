## Total Loan Applications
```dax
-- Creating Measure for Total Loan Applications
Total Loan Applications = COUNT ( bank_loan_analysis[id] )
```
## MTD Loan Applications
```dax
-- Creating Measure for MTD Loan Applications 
MTD Loan Applications = CALCULATE(   TOTALMTD([Total Loan Applications], DateTable[Date])   )
```
## PMTD Loan Applications
```dax
-- Calculating Previous Month to Date Loan Applications which is used to find MoM Loan Applications
PMTD Loan Applications = CALCULATE(   [Total Loan Applications], DATESMTD(DATEADD(DateTable[Date], -1, MONTH))   )
```
## MoM Loan Applications
```dax
-- Creating Measure for MoM Loan Applications
MoM Loan Applications = ([MTD Loan Applications]-[PMTD Loan Applications])/[PMTD Loan Applications]
```


## Total Funded Amount
```dax
-- Creating Measure for Total Funded Amount
Total Funded Amount = SUM(bank_loan_analysis[loan_amount])
```
## MTD Funded Amount
```dax
-- Creating Measure for MTD Funded Amount
MTD Funded Amount = CALCULATE(   TOTALMTD([Total Funded Amount], DateTable[Date])   )
```
## PMTD Funded Amount
```dax
-- Calculating Previous Month to Date Funded Amount which is used to find MoM Funded Amount
PMTD Funded Amount = CALCULATE(   [Total Funded Amount], DATESMTD(DATEADD(DateTable[Date], -1, MONTH))   )
```
## MoM Funded Amount
```dax
-- Creating Measure for MoM Funded Amount
MoM Funded Amount = ([MTD Funded Amount]-[PMTD Funded Amount])/[PMTD Funded Amount]
```


## Total Amount Recieved
```dax
-- Creating Measure for Total Amount Recieved 
Total Amount Recieved = SUM(bank_loan_analysis[total_payment])
```
## MTD Amount Recieved
```dax
-- Creating Measure for MTD Amount Recieved 
MTD Amount Recieved = CALCULATE(   TOTALMTD([Total Amount Recieved], DateTable[Date])   )
```
## PMTD Amount Recieved
```dax
-- Calculating Previous Month to Date Amount Recieved which is used to find MoM Amount Recieved 
PMTD Amount Recieved = CALCULATE(   [Total Amount Recieved], DATESMTD(DATEADD(DateTable[Date], -1, MONTH))   )
```
## MoM Amount Recieved
```dax
-- Creating Measure for MoM Amount Recieved 
MoM Amount Recieved = ([MTD Amount Recieved]-[PMTD Amount Recieved])/[PMTD Amount Recieved]
```


## Average Interest Rate
```dax
-- Creating Measure for Average Interest Rate
Average Interest Rate = AVERAGE(bank_loan_analysis[int_rate])
```
## MTD Average Interest Rate
```dax
-- Creating Measure for MTD Average Interest Rate
MTD Interest Rate = CALCULATE(   TOTALMTD([Average Interest Rate], DateTable[Date])   )
```
## PMTD Average Interest Rate
```dax
-- Calculating Previous Month to Date Average Interest Rate which is used to find MoM Average Interest Rate
PMTD Interest Rate = CALCULATE(   [Average Interest Rate], DATESMTD(DATEADD(DateTable[Date], -1, MONTH))   )
```
## MoM Average Interest Rate
```dax
-- Creating Measure for MoM Average Interest Rate
MoM Interest Rate = ([MTD Interest Rate]-[PMTD Interest Rate])/[PMTD Interest Rate]
```

## Average DTI
```dax
-- Creating Measure for Average DTI
Average DTI = AVERAGE(bank_loan_analysis[dti])
```
## MTD Average DTI
```dax
-- Creating Measure for MTD Average DTI
MTD DTI = CALCULATE(   TOTALMTD([Average DTI], DateTable[Date])   )
```
## PMTD Average DTI
```dax
-- Calculating Previous Month to Date Average DTI which is used to find MoM Average DTI
PMTD DTI = CALCULATE(   [Average DTI], DATESMTD(DATEADD(DateTable[Date], -1, MONTH))   )
```
## MoM Average DTI
```dax
-- Creating Measure for MoM Average DTI
MoM DTI = ([MTD DTI]-[PMTD DTI])/[PMTD DTI]
```


## Good Loans
```dax
-- Calculating count good loans
Good Loan Applications = CALCULATE(   [Total Loan Applications], bank_loan_analysis[loan_status (groups)]="Good Loan"   )
```
## Good Loan Percentage
```dax
-- Calculating percent of good loans to total loans.
Good Loan Percent = [Good Loan Applications]/[Total Loan Applications]
```
## Good Loan Funded Amount
```dax
-- Calculating Total Funded Amount for Good Loans.
Good Loan Funded Amount = CALCULATE(   [Total Funded Amount], bank_loan_analysis[loan_status (groups)]="Good Loan"   )
```
## Good Loan Amount Recieved
```dax
-- Calculating Total Amount Recieved for Good Loans.
Good Loan Amount Recieved = CALCULATE(   [Total Recieved Amount], bank_loan_analysis[loan_status (groups)]="Good Loan"   )
```


## Bad Loans
```dax
-- Calculating count bad loans
Bad Loan Applications = CALCULATE(   [Total Loan Applications], bank_loan_analysis[loan_status (groups)]="Bad Loan"   )
```
## Bad Loan Percentage
```dax
-- Calculating percent of bad loans to total loans.
Bad Loan Percent = [Bad Loan Applications]/[Total Loan Applications]
```
## Bad Loan Funded Amount
```dax
-- Calculating Total Funded Amount for Bad Loans.
Bad Loan Funded Amount = CALCULATE(   [Total Funded Amount], bank_loan_analysis[loan_status (groups)]="Bad Loan"   )
```
## Bad Loan Amount Recieved
```dax
-- Calculating Total Amount Recieved for Bad Loans.
Bad Loan Amount Recieved = CALCULATE(   [Total Recieved Amount], bank_loan_analysis[loan_status (groups)]="Bad Loan"   )
```


## Creating Date Table
```dax
-- Creating a Date Table out of our data.
DateTable = CALENDAR(  MIN(bank_loan_analysis[issue_date]), MAX(bank_loan_analysis[issue_date])   )
```


## Creating a Parameter Table 
```dax
-- Creating a Parameter Table to dynamically switch Measures in visuals using a slicer
-- Note: Parameter Table is created not measure.
Parameter = {
    ("Total Loan Applications", NAMEOF('bank_loan_analysis'[Total Loan Applications]), 0),
    ("Total Funded Amount", NAMEOF('bank_loan_analysis'[Total Funded Amount]), 1),
    ("Total Amount Recieved", NAMEOF('bank_loan_analysis'[Total Amount Recieved]), 2)
}
-- {...} --> Creates a Table
-- NAMEOF()--> Safely references a measure or column name, prevents error if the name changes.
-- 0,1,2(Sort Order) --> Sort Order decides the position of measures in slicers and visuals when using Field Parameters.
```


## Creating Dynamic slicers
# Creating Dynamic Titles that change based upon the value of slicer
```dax
-- Dynamic Title for Chart 1 (Month wise Analysis - Line Chart)
Dynamic Title 1 = 
SWITCH(
    TRUE(),
    MAX('Parameter'[Parameter Order]) = 0, "Total Loan Applications",
    MAX('Parameter'[Parameter Order]) = 1, "Total Funded Amount",
    MAX('Parameter'[Parameter Order]) = 2, "Total Amuont Recieved",
    "All"
) & " by Month"
```
