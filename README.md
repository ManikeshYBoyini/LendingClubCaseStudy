# Lending Club Case Study

## Table of Contents
1. Problem Statement
2. Objectives
3. Data Understanding
4. Loading Data
5. Data Cleaning and Imputation
6. Univariate Analysis
7. Bivariate Analysis
8. Summary

## Problem Statement

Lending Club is a consumer finance company providing various types of loans for the customers who are seeking a loan with investors looking to lend money and make profit from the their invested amount.

Like most other lending companies, *lending loans to ‘risky’* applicants is the highest cause of financial loss *(called credit loss)*. The credit loss is the total amount lost by the lender when the borrower do not pays back or runs away with the money owed. In other words, **borrowers** who **default** cause the largest amount of **loss to the lenders**. In this case, the customers labelled as *'charged-off' are the 'defaulters'*.

The main objective of this case study is to analyise the data provided and come up with constructive data that points out if a applicant is going to default or not.

There are two potential sources of **credit loss** are:
* Applicant 'likely to repay' such an applicant will bring in profit to the company with interest rates and rejecting such applicants will result in loss of business.
* Applicant who are most likley do not replay the loan, i.e. and will potentially default, then approving the loan may lead to a financial loss for the financial institute.

## Objectives
The goal of this case study is to to identify these risky loan applicants.
If one is able to identify these risky loan applicants, then such loans can be reduced thereby cutting down the amount of credit loss. Identification of such applicants using EDA is the one of the objective of this case study.
In other words, the company wants to understand the driving factors (or driver variables) behind loan default, the variables which are strong indicators of default. The company can utilise this knowledge for its portfolio and risk assessment.

### Analysis based on Domain Understanding

#### Leading Attribute (loan_status)
- *Loan Status* - Key Leading Attribute (*loan_status*). The column has three distinct values
    - Fully-Paid - The customer has successfuly paid the loan
    - Charged-Off - The customer is "Charged-Off" ir has "Defaulted"
    - Current - These customers, the loan is currently in progress and cannot contribute to conclusive evidence if the customer will default of pay in future
        - For the given case study, "Current" status rows will be ignored

#### Important Columns
The given columns are leading attributes, or **predictors**. These attributes are available at the time of the loan application and strongly helps in **prediction** of loan pass or rejection. Key attributes *Some of these columns may get dropped due to empty data in the dataset*
* **Customer Demographics**
1. annual_inc represents the self-reported annual income provided by the borrower during registration.
2. home_ownership represents the home ownership status provided by the borrower during registration. Our values are: RENT, OWN, MORTGAGE, OTHER.
3. emp_length represents employment length in years. Possible values are between 0 and 10 where 0 means less than one year and 10 means ten or more years. 
4. dti represents a ratio calculated using the borrower’s total monthly debt payments on the total debt obligations
5. addr_state represents the state provided by the borrower in the loan application

* **Loan Attributes**
  * Loan Ammount (loan_amt) 
  * Grade (grade)
  * Term (term)
  * Loan Date (issue_date)
  * Purpose of Loan (purpose)
  * Verification Status (verification_status)
  * Interest Rate (int_rate)
  * Installment (installment)
  * Public Records (public_rec) - Derogatory Public Records. The value adds to the risk to the loan. Higher the value, lower the success rate.
  * Public Records Bankruptcy  (public_rec_bankruptcy) - Number of bankruptcy records publocally available for the customer. Higher the value, lower is the success rate.
  
#### Ignored Columns
* The following types of columns will be ignored in the analysis. This is a generic categorization of the columns which will be ignored in our approach and not the full list.
   * **Customer Behaviour Columns** - Columns which describes customer behaviour will not contribute to the analysis. The current analysis is at the time of loan application but the customer behaviour variables generate post the approval of loan applications. Thus these attributes wil not be considered towards the loan approval/rejection process.
   * Granular Data - Columns which describe next level of details which may not be required for the analysis. For example grade may be relevant for creating business outcomes and visualizations, sub grade is be very granular and will not be used in the analysis

### Data Set Analysis based on understanding of EDA 

#### Rows Analysis
- Summary Rows: No summary rows were there in the dataset
- Header & Footer Rows - No header or footer rows in the dataset
- Extra Rows - No column number, indicators etc. found in the dataset
- Rows where the **loan_status = CURRENT will be dropped** as CURRENT loans are in progress and will not contribute in the decision making of pass or fail of the loan. The rows are dropped before the column analysis as it also cleans up unecessary column related to CURRENT early and columns with NA values can be cleaned in one go
- Find duplicate rows in the dataset and drop if there are

### Column analysis and clean up

#### Drop Columns 

1. All the columns with NA values should be dropped as this will not help in analysis.
2. There are 55 columns with all NA values. Columns that should be dropped are 
    (next_pymnt_d, mths_since_last_major_derog, annual_inc_joint, dti_joint, verification_status_joint, tot_coll_amt, tot_cur_bal, open_acc_6m, open_il_6m, open_il_12m, open_il_24m, mths_since_rcnt_il, total_bal_il, il_util, open_rv_12m, open_rv_24m, max_bal_bc, all_util, total_rev_hi_lim, inq_fi, total_cu_tl, inq_last_12m, acc_open_past_24mths, avg_cur_bal, bc_open_to_buy, bc_util, mo_sin_old_il_acct, mo_sin_old_rev_tl_op, mo_sin_rcnt_rev_tl_op, mo_sin_rcnt_tl, mort_acc, mths_since_recent_bc, mths_since_recent_bc_dlq, mths_since_recent_inq, mths_since_recent_revol_delinq, num_accts_ever_120_pd, num_actv_bc_tl, num_actv_rev_tl, num_bc_sats, num_bc_tl, num_il_tl, num_op_rev_tl, num_rev_accts, num_rev_tl_bal_gt_0, num_sats, num_tl_120dpd_2m, num_tl_30dpd, num_tl_90g_dpd_24m, num_tl_op_past_12m, pct_tl_nvr_dlq, percent_bc_gt_75, tot_hi_cred_lim, total_bal_ex_mort, total_bc_limit, total_il_high_credit_limit)
   
3. All the columns with zero values should be dropped.
4. The columns where more than 65% of data is empty (mths_since_last_delinq, mths_since_last_record) should be dropped.
5. Drop columns (emp_title, desc, title) as they are discriptive and text (nouns) and dont contribute to analysis.
6. Drop customer behaviour columns which show data post the approval of loan. They contribute to the behaviour of the customer. Behaviour of the customer is recorded post approval of loan and not available at the time of loan approval. Thus these variables will not be considered in analysis and thus dropped`(delinq_2yrs, earliest_cr_line, inq_last_6mths, open_acc, pub_rec, revol_bal, revol_util, total_acc, out_prncp, out_prncp_inv, total_pymnt, total_pymnt_inv, total_rec_prncp, total_rec_int, total_rec_late_fee, recoveries, collection_recovery_fee, last_pymnt_d, last_pymnt_amnt, last_credit_pull_d, application_type)`
    
#### Convert Column Format

1. loan_amnt, funded_amnt, funded_amnt_inv columns are object type and will be converted to respective data type
2. int_rate, installment, dti columns are object type and will be converted to float
3. Take out "month" text from `term` column and convert to integer

#### Standardise Values

1. All currency values in the columns should be rounded off to 2 decimal places.

#### Derived columns
- verification_status_n added. Considering domain knowledge of lending = Verified > Source Verified > Not Verified. verification_status_n correspond to {Verified: 3, Source Verified: 2. Not Verified: 1} for better analysis
- issue_y is year extracted from issue_d
- issue_m is month extracted from issue_d

## Approach
- Step 0 - Data Cleaning & Manipulation Checklist
- Step 1 - Dropping Rows - where loan_status = "Current"
- Step 2 - Dropping Columns based on EDA and Domain Knowledge
- Step 3 - Convert the data types
- Step 4 - Identify columns with blank values which need to be imputed
- Step 5 - Analysis of the dataset post cleanup
- Step 6 - Outlier Treatment
- Step 7 - **Analysis** - Univariate, Bivariate and Derived Metrics Analysis
- Step 8 - **Conclusions** Inferences and Recommendations

## Conclusions

Lending club should check borrowers grade in order to reduce risk.
Grade A applicants are more likely to pay full loan.
Lending club should reduce the loans for 60 months tenure as they are prone to loan default.
In contrast, 36 month term shows less loan default average
Home ownership with Other are likely to default the loan and Mortgage may have more chance to fully pay the loan.
Lower the dti ratio lower the change of loan default also, it is likely that low dti ratio borrower Likely to pay full loan.

Created by [@manikeshyboyini] - feel free to contact me!
