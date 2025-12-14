# Loan Dashboard
 
### Power bi Link : [https://app.powerbi.com/groups/me/reports/384d017e-e935-44dc-9e7d-1626c1a36de1/ReportSection](https://app.powerbi.com/links/8kE0wMvlct?ctid=12c1af42-a9a2-48e7-a26e-6a09f52dd431&pbi_source=linkShare)

## Problem Statement

This dashboard helps the company understand their customers better.Through different charts, they get to know their critical areas like total loan taken for different purposes,default rate per year/employment type & year on year loan amount change be it total loan taken or number of loan taken.Therefore, they can improve their loan lending and receiving strategy by identifying these area.

Since, the default rate per year is more than 11% this imply that company needs to make sure that they get loan back which they are lending. 

Also, out of 32 billion rupees of loan lended majority is given to high income bracket so if company wants to expand their customer base then they should tap to medium and low income bracket. 


### Steps followed 

- Step 1 : Load data into Power BI Desktop from sql server, dataset is an csv file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".
- Step 4 : It was observed that in none of the column has errors.
- Step 5 : A Rounded rectangle is added at top of page using Inset>Shape>Rounded rectangle.Text written inside it "Loan default and overview".
- Step 6 : DAX is used to find "Total loan"              
```bash
                     Loan amount by purpose = SUMX(FILTER('Loan_default part 412',NOT(ISBLANK           
                     ('Loan_default part 412'[LoanAmount]))),'Loan_default part 412'[LoanAmount])
```
 A line graph was used to show "Loan Amount Taken For Different Purposes".

<img width="417" height="303" alt="loan amount by purpose" src="https://github.com/user-attachments/assets/07dcef10-4df4-4527-8052-19b74606bf6d" />

  



- Step 7 : DAX is used to find "Average income by employment type"
```bash
                    Average income by employment type = CALCULATE(AVERAGE('Loan_default part  
                    412'[Income]),ALLEXCEPT('Loan_default part 412','Loan_default part 412'
                    [EmploymentType]))
```
  Line graph added to show "Average income by employement type".    



- Step 8 : DAX is used to find "Default rate %
```bash

                   Default rate vis-a-vis to total by employment type = var total=COUNTROWS(ALL('Loan_default part 412'))     
                   var default=COUNTROWS(FILTER('Loan_default part 412','Loan_default part 412'[Default]=TRUE()))       
                   return CALCULATE(DIVIDE(default,total))*100
```
   Line graph added to show "Default rate(%) by employment type"

   
- Step 9 : DAX is used to add new column "Age group"
 ```bash
                   Age group = IF('Loan_default part 412'[Age]<=19,"Teen",
                               IF('Loan_default part 412'[Age]<=39,"Adults",
                               IF('Loan_default part 412'[Age]<=59,"Middle Aged","Senior Citizens")))
 ```

  Another DAX is used to find "Average loan"
```bash
                  Average loan = AVERAGE('Loan_default part 412'[LoanAmount])
```
Line graph added to show "Average loan by age group"

- Step 10 : DAX is used to create new column "Year"
```bash
                  Year = YEAR('Loan_default part 412'[Loan_Date_DD_MM_YYYY])
```
Another DAX is used to find "Default rate per year"
```bash

                 Default Rate per year = var total=CALCULATE(COUNTROWS('Loan_default part 412'),ALLEXCEPT('Loan_default part 412','Loan_default part 412'[Year]))
                 var default=CALCULATE(COUNTROWS(FILTER('Loan_default part 412','Loan_default part
                 412'[Default]=TRUE())),ALLEXCEPT('Loan_default part 412','Loan_default part 412'[Year]))
                 return DIVIDE(default,total)*100
```
Line graph is used to show "Default rate(%) per Year"

- Step 11 : A Rounded rectangle is added at top of page using Inset>Shape>Rounded rectangle.Text written inside it is "Applicant Demographic & Financial Profile"
- Step 12 : DAX is used to find "Credit score bins"
```bash

                Credit Score bins = IF('Loan_default part 412'[CreditScore]<=400,"Very Low",
                IF('Loan_default part 412'[CreditScore]<=450,"Low",
                IF('Loan_default part 412'[CreditScore]<=650,"Medium","High")))
```
Another DAX is used to find "Median loan amount"
```bash

               Median Loan amount = MEDIANX('Loan_default part 412','Loan_default part 412'[LoanAmount])
```
Line graph is used to show "Median loan amount by credit score category"
- Step 13 : DAX is used to find "Average loan amount(high category)"
```bash

               Average Loan Amount(High category) = AVERAGEX(FILTER('Loan_default part 412',
              'Loan_default part 412'[Credit Score bins]="High"),'Loan_default part 412'[LoanAmount])
```
Donut chart is used to show "Average loan amount(High credit score) by age group and maritalStatus"

<img width="685" height="311" alt="Average Loan Amount(High credit score) by Age group and MaritalStatus" src="https://github.com/user-attachments/assets/f0dd9e17-cb18-49a3-99aa-eaebc3bc7835" />

- Step 14 : DAX is used to find "Total loan(adults)"
```bash

               Total Loan(Adults) = SUMX(FILTER('Loan_default part 412','Loan_default part 412'[Age group]="Adults"),
               'Loan_default part 412'[LoanAmount])
```
Line graph is used to show "Total loan (adults) by credit score bins"
        
- Step 15 : DAX is used to find "Total loan (middle aged)"
```bash

              Total Loan (Middle aged) = SUMX(FILTER('Loan_default part 412','Loan_default part 412'[Age group]="Middle Aged"),
             'Loan_default part 412'[LoanAmount])
```
Culstered column chart is used to show "Total loan (middle aged) having mortgage /dependents"
<img width="425" height="307" alt="Total Loan (Middle aged) Having Mortgage" src="https://github.com/user-attachments/assets/249dedd5-03a5-4977-b3c1-f664927e7c6e" />

 
 - Step 16 : DAX is used to find "Number of loans"
```bash

             Number of Loans = COUNTROWS(FILTER('Loan_default part 412',NOT(ISBLANK('Loan_default part 412'[LoanID]))))
 ```
Line chart is used to show "Number of loans by education type"
- Step 17 : A Rounded rectangle is added at top of page using Inset>Shape>Rounded rectangle.Text written inside it is "Financial Risk matrics"
 - Step 18 : DAX is used to find "YoY loan amount change"
```bash

            YoY Loan amount change = Var currents=CALCULATE(SUM('Loan_default part 412'[LoanAmount]),'Loan_default part 412'[Year]=YEAR(MAX('Loan_default part 412'[Loan_Date_DD_MM_YYYY])))
            Var Previous=CALCULATE(SUM('Loan_default part 412'[LoanAmount]),'Loan_default part 412'[Year]=YEAR(MAX('Loan_default part 412'[Loan_Date_DD_MM_YYYY]))-1)
            RETURN    DIVIDE(currents-Previous,Previous,0)*100
```
Line chart is created to show "YOY % change sum of loan amount taken"

- Step 19 : DAX is used to find "YOY default loan change"
```bash

            YOY Default Loan change = var currents =CALCULATE(COUNTROWS(FILTER('Loan_default part 412','Loan_default part 412'[Default]=TRUE())),'Loan_default part 412'[Year]=YEAR(MAX('Loan_default part 412'[Loan_Date_DD_MM_YYYY])))
            var previous=CALCULATE(COUNTROWS(FILTER('Loan_default part 412','Loan_default part 412'[Default]=TRUE())),'Loan_default part 412'[Year]=YEAR(MAX('Loan_default part 412'[Loan_Date_DD_MM_YYYY]))-1)
            RETURN DIVIDE(currents-previous,previous,0)*100
```
Line chart is created to show "YOY % change of number of loan taken"

- Step 20 : DAX is used to find "YTD loan amount"
```bash

            YTD Loan Amount = TOTALYTD(SUM('Loan_default part 412'[LoanAmount]),'Loan_default part 412'[Loan_Date_DD_MM_YYYY].[Date])
```
Ribbon chart is used to show "YTD loan amount by credit score bins and marital status"

- Step 21 : Decomposition tree is used to show relation between "Total loan taken" , "Income bracket" and "Employement type"
<img width="634" height="309" alt="Decomposition Tree" src="https://github.com/user-attachments/assets/90c26190-4873-42ee-a3c2-26326b6c0b7b" />

# Snapshot of all 3 pages of report 
# Page 1
<img width="1315" height="737" alt="Page 1" src="https://github.com/user-attachments/assets/acd2dfb0-a1ca-4068-80c8-ca4af25793f2" />

# Page 2
<img width="1318" height="747" alt="Page 2" src="https://github.com/user-attachments/assets/060389e5-355d-4491-80c8-8401f53b3f0a" />

# Page 3
<img width="1313" height="736" alt="Page 3" src="https://github.com/user-attachments/assets/5d5a2d0e-3d83-490b-8dae-2b3cc432b865" />


# Insights

A three page report is created and it was published in power bi services.

Following inferences can be drawn from the Report;

### Page 1: Loan default and overview
### A) Loan amount by purpose
   Loan taken for purpose "Home"= 6545 Million
   
   Loan taken for purpose "Business"= 6522 Million
   
   Loan taken for purpose "Education"= 6511 Million
   
   Loan taken for purpose "Auto"= 6501 Million
   
   Loan taken for purpose "Other"= 6498 Million
```bash
   Top three purpose of loan are Home , Business and Education
```           
### B) Average income by employment type
   Average income of "Full time employed" = 82.89 Thousand

   Average income of "Self employed" = 82.44 Thousand

   Average income of "Part time employed" = 82.38 Thousand

   Average income of "Unemployed" = 82.27 Thousand

```bash
   Each employment category has average earning approximately same.
```  
 ### C) Default rate(%) by employment type 
  Default rate(%) of "Unemployed" = 3.39

  Default rate(%) of "Part time" = 3.01

  Default rate(%) of "Self employed" = 2.86

  Default rate(%) of "Full time" = 2.36

```bash
   Unemployed people are highest defaulter of loan.
``` 
### D) Average loan by age group
   Average loan taken by "Adults"= 127901

   Average loan taken by "Middle Aged"= 127459

   Average loan taken by "Senior Citizens"= 127355

   Average loan taken by "Teen"= 126674
   
```bash
   Each age group is having average loan borrowing approximately same.
``` 
### E) Default rate(%) per year  
Default rate in 2013 = 11.62

Default rate in 2013 = 11.50

Default rate in 2013 = 11.70

Default rate in 2013 = 11.75

Default rate in 2013 = 11.50

Default rate in 2013 = 11.60
```bash
   Default rate is more than 11% on each year so need to focus on this
```

### Page 2:Applicant Demographic & Financial Profile
### A) Median loan amount by credit score category
Median loan amount with "Low" credit score = 128397

Median loan amount with "Medium" credit score = 127764

Median loan amount with "Very Low" credit score = 127515

Median loan amount with "High" credit score = 127149
```bash
   People with low credit score buy on an average highest loan
```
### B) Average loan amount(high credit score) by age group and maritalstatus
Adults who are divorced = 127.65 thousand (8.37%), married = 128.35 thousand (8.42%) ,single= 128.13 thousand (8.4%)

Middle aged who are divorced = 128.12 thousand (8.4%), married = 126.74 thousand (8.31%) ,single= 127.14 thousand (8.34%)

Senior citizens who are divorced = 123.97 thousand (8.35%), married = 127.23 thousand (8.34%) ,single= 127.42 thousand (8.35%)

Teen who are divorced = 123.97 thousand (8.13%), married = 124.87 thousand (8.19%) ,single= 128.57 thousand (8.43%)

```bash
   Each age group person be single,married or divorced having high credit score taking
   approximately same amount of loan
```
### C) Total loan (Adults) by credit score bins
Loan taken by adults with "Medium" credit score = 4.6 Billion

Loan taken by adults with "High" credit score = 4.5 Billion

Loan taken by adults with "Very" credit score = 2.3 Billion

Loan taken by adults with "Low" credit score = 1.1 Billion
```bash
   Among adults "Medium" and "High" credit score people are highest borrower
```
### D) Total loan (middle aged) having mortgage /dependents
Loan taken by middle aged people with "Mortgage" and "Dependents = 3.1 Billion

Loan taken by middle aged people with "Mortgage" but without "Dependents" = 3.1 Billion

Loan taken by middle aged people without "Mortgage" and "Dependents = 3.1 Billion

Loan taken by middle aged people without "Mortgage" and but with "Dependents = 3.1 Billion

```bash
   All middled aged people with or without mortgage taking approximately same amount of loan.
```
### E) Number of loans by education type
Number of loan taken by "Bachelors" = 64366

Number of loan taken by "High School" = 63903

Number of loan taken by "Masters" = 63541

Number of loan taken by "Phd" = 63537
```bash
   The number of loans taken by bachelors is among highest
```
### Page 2:Financial Risk Matrics
### A) YOY % change on sum of loan amount taken
```bash
   The graph reflects that for the year 2014 and 2017 the sum of loan taken is in negative
   as compared to previous this may indicate market trend or company policy.
```
### B) YOY % change of number of loan taken
```bash
   The graph reflects that for the year 2014 and 2017 the number of loan taken is negative
   as compared to previous this may indicate market trend or company policy indicating (A) point above
```
### C) YTD loan amount by credit score bins and marital status

```bash
   If we look into the graph then we can interpret that people with medium and high credit score and 
   divorced take highest amount of loan which is 0.67 Billion.In very low category single people with
   0.36 Billion and low category married people with 0.17 Billion of loan taken are the highest.
```
### D) YTD loan amount by credit score bins and marital status

```bash
 If we expand the graph then we can see that out of total loan of amount 32.6 Billion given maximum is
 is given to high income bracket (66%), 7.2 Billion (22%) to medium income bracket and 3.6 Billion (11%)
 given to low income bracket people
```
