-------------------------------------------------------------------------------------------------------------------------------------
## Total Loan Applications
```dax
-- Creating Measure for Total Loan Applications
Total Loan Applications = COUNT ( bank_loan_analysis[id] )
```

## MTD Loan Applications
```dax
-- Creating Measure for MTD Loan Applications --
MTD Loan Applications = CALCULATE(   TOTALMTD([Total Loan Applications], DateTable[Date])   )
```

-- Calculating Previous Month to Date Loan Applications which is used to find MoM Loan Applications --
PMTD Loan Applications = CALCULATE(   [Total Loan Applications], DATESMTD(DATEADD(DateTable[Date], -1, MONTH))   )

-- Creating Measure for MoM Loan Applications --
MoM Loan Applications = ([MTD Loan Applications]-[PMTD Loan Applications])/[PMTD Loan Applications]

-------------------------------------------------------------------------------------------------------------------------------------

-- Creating Measure for Total Funded Amount --
Formula: Total Funded Amount = SUM(bank_loan_analysis[loan_amount])

-- Creating Measure for MTD Funded Amount --
Formula: MTD Funded Amount = CALCULATE(   TOTALMTD([Total Funded Amount], DateTable[Date])   )

-- Calculating Previous Month to Date Funded Amount which is used to find MoM Funded Amount --
Formula: PMTD Funded Amount = CALCULATE(   [Total Funded Amount], DATESMTD(DATEADD(DateTable[Date], -1, MONTH))   )

-- Creating Measure for MoM Funded Amount --
Formula: MoM Funded Amount = ([MTD Funded Amount]-[PMTD Funded Amount])/[PMTD Funded Amount]

-------------------------------------------------------------------------------------------------------------------------------------

-- Creating Measure for Total Amount Recieved --
Formula: Total Amount Recieved = SUM(bank_loan_analysis[total_payment])

-- Creating Measure for MTD Amount Recieved --
Formula: MTD Amount Recieved = CALCULATE(   TOTALMTD([Total Amount Recieved], DateTable[Date])   )

-- Calculating Previous Month to Date Amount Recieved which is used to find MoM Amount Recieved --
Formula: PMTD Amount Recieved = CALCULATE(   [Total Amount Recieved], DATESMTD(DATEADD(DateTable[Date], -1, MONTH))   )

-- Creating Measure for MoM Amount Recieved --
Formula: MoM Amount Recieved = ([MTD Amount Recieved]-[PMTD Amount Recieved])/[PMTD Amount Recieved]

-------------------------------------------------------------------------------------------------------------------------------------

-- Creating Measure for Average Interest Rate --
Formula: Average Interest Rate = AVERAGE(bank_loan_analysis[int_rate])

-- Creating Measure for MTD Average Interest Rate --
Formula: MTD Interest Rate = CALCULATE(   TOTALMTD([Average Interest Rate], DateTable[Date])   )

-- Calculating Previous Month to Date Average Interest Rate which is used to find MoM Average Interest Rate --
Formula: PMTD Interest Rate = CALCULATE(   [Average Interest Rate], DATESMTD(DATEADD(DateTable[Date], -1, MONTH))   )

-- Creating Measure for MoM Average Interest Rate --
Formula: MoM Interest Rate = ([MTD Interest Rate]-[PMTD Interest Rate])/[PMTD Interest Rate]

-------------------------------------------------------------------------------------------------------------------------------------

-- Creating Measure for Average DTI --
Formula: Average DTI = AVERAGE(bank_loan_analysis[dti])

-- Creating Measure for MTD Average DTI --
Formula: MTD DTI = CALCULATE(   TOTALMTD([Average DTI], DateTable[Date])   )

-- Calculating Previous Month to Date Average DTI which is used to find MoM Average DTI --
Formula: PMTD DTI = CALCULATE(   [Average DTI], DATESMTD(DATEADD(DateTable[Date], -1, MONTH))   )

-- Creating Measure for MoM Average DTI --
Formula: MoM DTI = ([MTD DTI]-[PMTD DTI])/[PMTD DTI]

-------------------------------------------------------------------------------------------------------------------------------------



















