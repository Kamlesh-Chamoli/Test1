# Loan Dashboard
 
### Dashboard Link : https://app.powerbi.com/groups/me/reports/384d017e-e935-44dc-9e7d-1626c1a36de1/ReportSection

## Problem Statement

This dashboard helps the company understand their customers better.Through different charts, they get to know their critical areas like total loan taken for different purposes,default rate per year/employment type & year on year loan amount change be it total loan taken or number of loan taken.Therefore, they can improve their loan lending and receiving strategy by identifying these area.

Since, the default rate per year is more than 11% this imply that company needs to make sure that they get loan back which they are lending.

Also, out of 32 billion rupees of loan lended majority is given to high income bracket leading if company wants to expand their customer base then they should tap to medium and low income bracket. 


### Steps followed 

- Step 1 : Load data into Power BI Desktop from sql server, dataset is an csv file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".
- Step 4 : It was observed that in none of the column has errors.
- Step 5 : A Rounded rectangle is added at top of page using Inset>Shape>Rounded rectangle.Text written inside it is "Loan Default Overview" 
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
  Line graph added to show "Average Income By Employement Type".    



- Step 8 : DAX is used to find "Default rate vis-a-vis to total by employment type"
```bash

                   Default rate vis-a-vis to total by employment type = var total=COUNTROWS(ALL('Loan_default part 412'))     
                   var default=COUNTROWS(FILTER('Loan_default part 412','Loan_default part 412'[Default]=TRUE()))       
                   return CALCULATE(DIVIDE(default,total))*100
```
   Line graph added to show "Default Rate(%) by Employment Type"

   
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
Line graph added to show "Average Loan By Age Group"

- Step 10 : DAX is used to create new column "Year"
```bash
                  Year = YEAR('Loan_default part 412'[Loan_Date_DD_MM_YYYY])
```
Another DAX is used to find "Default Rate per year"
```bash

                 Default Rate per year = var total=CALCULATE(COUNTROWS('Loan_default part 412'),ALLEXCEPT('Loan_default part 412','Loan_default part 412'[Year]))
                 var default=CALCULATE(COUNTROWS(FILTER('Loan_default part 412','Loan_default part
                 412'[Default]=TRUE())),ALLEXCEPT('Loan_default part 412','Loan_default part 412'[Year]))
                 return DIVIDE(default,total)*100
```
Line graph is used to show "Default Rate(%) Per Year"

- Step 11 : A Rounded rectangle is added at top of page using Inset>Shape>Rounded rectangle.Text written inside it is "Applicant Demographic & Financial Profile"
- Step 12 : DAX is used to find "Credit Score bins"
```bash

                Credit Score bins = IF('Loan_default part 412'[CreditScore]<=400,"Very Low",
                IF('Loan_default part 412'[CreditScore]<=450,"Low",
                IF('Loan_default part 412'[CreditScore]<=650,"Medium","High")))
```
Another DAX is used to find "Median Loan amount"
```bash

               Median Loan amount = MEDIANX('Loan_default part 412','Loan_default part 412'[LoanAmount])
```
Line graph is used to show "Median Loan Amount by Credit Score Category"
- Step 13 : DAX is used to find "Average Loan Amount(High category)"
```bash

               Average Loan Amount(High category) = AVERAGEX(FILTER('Loan_default part 412',
              'Loan_default part 412'[Credit Score bins]="High"),'Loan_default part 412'[LoanAmount])
```
Donut chart is used to show "Average Loan Amount(High credit score) by Age group and MaritalStatus"

<img width="685" height="311" alt="Average Loan Amount(High credit score) by Age group and MaritalStatus" src="https://github.com/user-attachments/assets/f0dd9e17-cb18-49a3-99aa-eaebc3bc7835" />

- Step 14 : DAX is used to find "Total Loan(Adults)"
```bash

               Total Loan(Adults) = SUMX(FILTER('Loan_default part 412','Loan_default part 412'[Age group]="Adults"),
               'Loan_default part 412'[LoanAmount])
```
Line graph is used to show "Total Loan (Adults) by Credit Score Bins"
        
- Step 15 : DAX is used to find "Total Loan (Middle aged)"
```bash

              Total Loan (Middle aged) = SUMX(FILTER('Loan_default part 412','Loan_default part 412'[Age group]="Middle Aged"),
             'Loan_default part 412'[LoanAmount])
```
Culstered column chart is used to show "Total Loan (Middle aged) Having Mortgage /Dependents"
<img width="425" height="307" alt="Total Loan (Middle aged) Having Mortgage" src="https://github.com/user-attachments/assets/249dedd5-03a5-4977-b3c1-f664927e7c6e" />

 
 - Step 16 : DAX is used to find "Number of Loans"
```bash

             Number of Loans = COUNTROWS(FILTER('Loan_default part 412',NOT(ISBLANK('Loan_default part 412'[LoanID]))))
 ```
Line chart is used to show "Number of Loans By Education Type"
- Step 17 : A Rounded rectangle is added at top of page using Inset>Shape>Rounded rectangle.Text written inside it is "Financial Risk matrics"
 - Step 18 : DAX is used to find "YoY Loan amount change"
```bash

            YoY Loan amount change = Var currents=CALCULATE(SUM('Loan_default part 412'[LoanAmount]),'Loan_default part 412'[Year]=YEAR(MAX('Loan_default part 412'[Loan_Date_DD_MM_YYYY])))
            Var Previous=CALCULATE(SUM('Loan_default part 412'[LoanAmount]),'Loan_default part 412'[Year]=YEAR(MAX('Loan_default part 412'[Loan_Date_DD_MM_YYYY]))-1)
            RETURN    DIVIDE(currents-Previous,Previous,0)*100
```
Line chart is created to show "YOY % Change Sum Of Loan Amount Taken"

- Step 19 : DAX is used to find "YOY Default Loan change"
```bash

            YOY Default Loan change = var currents =CALCULATE(COUNTROWS(FILTER('Loan_default part 412','Loan_default part 412'[Default]=TRUE())),'Loan_default part 412'[Year]=YEAR(MAX('Loan_default part 412'[Loan_Date_DD_MM_YYYY])))
            var previous=CALCULATE(COUNTROWS(FILTER('Loan_default part 412','Loan_default part 412'[Default]=TRUE())),'Loan_default part 412'[Year]=YEAR(MAX('Loan_default part 412'[Loan_Date_DD_MM_YYYY]))-1)
            RETURN DIVIDE(currents-previous,previous,0)*100
```
Line chart is created to show "YOY % Change Of Number of Loan Taken"

- Step 20 : DAX is used to find "YTD Loan Amount"
```bash

            YTD Loan Amount = TOTALYTD(SUM('Loan_default part 412'[LoanAmount]),'Loan_default part 412'[Loan_Date_DD_MM_YYYY].[Date])
```
Ribbon chart is used to show "YTD Loan Amount by Credit Score bins and Marital Status"

- Step 21 : Decomposition tree is used to show relation between "Total Loan Taken" , "Income Bracket" and "Employement Type"
<img width="634" height="309" alt="Decomposition Tree" src="https://github.com/user-attachments/assets/90c26190-4873-42ee-a3c2-26326b6c0b7b" />

# Snapshot of all 3 pages of report 
# Page 1
<img width="1315" height="737" alt="Page 1" src="https://github.com/user-attachments/assets/acd2dfb0-a1ca-4068-80c8-ca4af25793f2" />

# Page 2
<img width="1318" height="747" alt="Page 2" src="https://github.com/user-attachments/assets/060389e5-355d-4491-80c8-8401f53b3f0a" />

# Page 3
<img width="1313" height="736" alt="Page 3" src="https://github.com/user-attachments/assets/5d5a2d0e-3d83-490b-8dae-2b3cc432b865" />


# Insights

A three page report is created adn it was published in power bi services.

Following inferences can be drawn from the Report;

### Page 1: Loan Default and overview
### A)
   Loan taken for purpose "Home"= 6545 Million
   
   Loan taken for purpose "Business"= 6522 Million
   
   Loan taken for purpose "Education"= 6511 Million
   
   Loan taken for purpose "Auto"= 6501 Million
   
   Loan taken for purpose "Other"= 6498 Million
```bash
       Top three purpose for loan are "Home" , "Business" and "Education"
```           
### [2] Average Ratings

    a) Baggage Handling - 3.63/5
    b) Check-in Service - 3.31/5
    c) Cleanliness - 3.29/5
    d) Ease of online booking - 2.88/5
    e) Food & Drink - 3.21/5
    f) In-flight Entertainment - 3.36/5
    g) In-flight service - 3.64/5
    h) In-flight Wifi service - 2.81/5
    i) Leg room service - 3.37/5
    j) On-board service - 3.38/5
    k) Online boarding - 3.33/5
    l) Seat comfort - 3.44/5
    m) Departure & arrival convenience - 3.22/5
  
  while calculating average rating, null values have been ignored as they were not relevant for some customers. 
  
  These ratings will change if different visual filters will be applied.  
  
  ### [3] Average Delay 
  
      a) Average delay in arrival(minutes) - 15.09
      b) Average delay in departure(minutes) - 14.71
Average delay will change if different visual filters will be applied.

 ### [4] Some other insights
 
 ### Class
 
 1.1) 47.87 % customers travelled by Business class.
 
 1.2) 44.89 % customers travelled by Economy class.
 
 1.3) 7.25 % customers travelled by Economy plus class.
 
         thus, maximum customers travelled by Business class.
 
 ### Age Group
 
 2.1)  21.69 % customers belong to '0-25' age group.
 
 2.2)  52.44 % customers belong to '25-50' age group.
 
 2.3)  25.57 % customers belong to '50-75' age group.
 
 2.4)  0.31 % customers belong to '75-100' age group.
 
         thus, maximum customers belong to '25-50' age group.
         
### Customer Type

3.1) 18.31 % customers have customer type 'First time'.

3.2) 81.69 % customers have customer type 'returning'.
       
       thus, more customers have customer type 'returning'.

### Type of travel

4.1) 69.06 % customers have travel type 'Business'.

4.2) 30.94 % customers have travel type 'Personal'.

        thus, more customers have travel type 'Business'.
Displaying # Airlines-Dashboard.md.txt.
