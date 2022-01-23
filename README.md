# Egypt Payroll Calculations Jan 2022
*It can be used in Odoo payroll*

This code is calculating the Egyptian taxes according to the tax updated tax laws according to the Egyptian law.


Author: **Tamer ElSherif**
       
        17 Jan 2022
        
**How to add in Odoo Payroll v15 or Online**

Go to:
*  Configuration
*  Rules
*  in Salary Structure Rules
*  Create a new rule
*  name it Taxes  


**Conditions**
Condition Based on	Always True
  
  
**Computation**
Amount Type	Python Code
  
**Python Code**

result=0
Duration=1
gross=contract.wage
InsuranceWage= 9400 if contract.wage>9400 else contract.wage
CommunityParticipation=0.0005*gross*Duration
EmployeeSocialInsuranceShare=0.11*InsuranceWage*Duration
PersonalExemption=Duration*9000/12
TaxableAmountEstimate=gross-CommunityParticipation-EmployeeSocialInsuranceShare-PersonalExemption
AnnualTaxableAmount= 0 if TaxableAmountEstimate<=0 else TaxableAmountEstimate*12
TAXABLE=AnnualTaxableAmount

Level8_tax= 375 if 600001<=TAXABLE<700000 else 2625 if 700001<=TAXABLE<800000 else 4875 if 800001<=TAXABLE<900000 else 7875 if 900001<=TAXABLE<1000001 else 12875 if TAXABLE>1000001 else 0

if TAXABLE <= 30000 :
  result = -(TAXABLE-15000 * 0.025)/12
elif 30000 < TAXABLE <= 45000:
  result = -(375 + (TAXABLE - 30000)*0.10)/12
elif 45000 < TAXABLE <= 60000 :
  result = -(1500 +375+(TAXABLE - 45000)*0.15)/12
elif 60000 < TAXABLE <= 200000 :
  result = (2250 + 1500 +375+ (TAXABLE - 60000)*0.2 )/12
elif 200000 < TAXABLE <= 400000 : 
  result = -(28000 + 2250 + 1500 +375+ (TAXABLE - 400000)*0.225 )/12
elif TAXABLE > 600000 :
  result = -(Level8_tax + 45000+28000 + 2250 + 1500 +375+ (TAXABLE - 400000)*0.25 )/12
else:
  result = 0 
