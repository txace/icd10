# TxAce ICD-10 Project

1. Working SQL and FreeTDS
  * FreeTDS
  * Configuration in /c0/dwsql/etc
  * LIB_freetds in CMHC

2. ICD10DST uscript  (which builds the dsts in CMHC)

3. DCTs that match the standard

  * Reason
    1 - Admission        New DX
    2 - Re-Evaluation    Re-Diagnosis
    3 - Death
    4 - Discharge

  * GAF DCT2085 
    * _ISSUE_:  DST that edit against DCT must be alpha and DX10 has C.DX10.A5.GAF is Binary
    * 00 -100   

  * Current ABL    GCC 9084
    0 - Not Retarded
    1 - Mild Retardation
    2 - Moderate
    3 - Severe
    4 - Profound

  *  Potential ABL     Starcare DCT 215
    0 - Not Retarded
    1 - Mild Retardation
    2 - Moderate
    3 - Severe
    4 - Profound
    
  * Primary Axis   DCT 2087
    1 - Axis I
    2 - Axis II

  * DX Status    DCT 2088 
    C  Confirmed
    P  Provisional

4. List of all your ICD 9 DSTs

5. ConICDSQL (which is used to copy & convert ICD9 to ICD10 dsts

  a. [uscript](https://raw.githubusercontent.com/txace/icd10-conversion/master/ConICDSQL.usc)
  b. [parm](https://raw.githubusercontent.com/argmonster/icd10-conversion/master/icdmap.example)

6. SQL Scrips that builds tables

  * [create 10-9](https://raw.githubusercontent.com/txace/icd10-sql/master/tables/CreateTable-ICD-10-to-ICD-9-GEM.sql)
  * [create 10-10](https://raw.githubusercontent.com/txace/icd10-sql/master/tables/CreateTable-ICD-9-to-ICD-10-GEM.sql)
  * Import Data - Two options ::

    1. `BULK INSERT [t_ICD-10_ICD-9_with_GEM_AXIS] FROM 'c:\ICD-9-GEM-Complete.txt' WITH (FIELDTERMINATOR = '|’)`
    2. [insert 10-10](https://raw.githubusercontent.com/txace/icd10-sql/master/insert/ICD-10-GEM-Complete.sql)
    3. [insert 10-9](https://raw.githubusercontent.com/txace/icd10-sql/master/insert/ICD-9-GEM-Complete.sql)

7. ICD10 DST Data Export  Couple of Options

  1. [ussqldx10  (exports a flat file for SQL to import)](https://github.com/txace/icd10-conversion/blob/master/ussqldx10.usc)
  2. SQL export  (It exports directly using freetds) 
    * [Script](https://github.com/txace/cmhc-export)
    * [Parm](https://github.com/txace/icd10-export)

8. DX10.sql    (T-SQL script to create SQL table to hold imported CMHC diagnosis)

  1. [Script](https://github.com/txace/icd10-export/blob/master/DX10.sql)

9. DX10.usc (Diagnosis entry Form used to enter new Diagnosis…writes to new ICD10 DSTs)    
  * Discuss Libs & Inculdes
    * [DX10.usc script](https://github.com/txace/icd10-sql/blob/master/uScript/DX10.usc)
    * `%include inc_DX10`
    * `%includeinc_ER`
    * `%includeinc_ISN`
    * `lib_UACOMMON`
    * `lib_COMMON`
    * `CONCVICICD10_9`  (Which lets you write to the old ICD9 DX record)
    * `inc_DX`

  * _ISSUE_:  `lib_DX10:PreviousGAF()` has hard coded HFC dsts for old DX record

10.   Care Batch

  * [care-submit uscript](https://github.com/txace/care-batch/blob/master/usc/care-submit.usc)

    1.  parm to map DSTs to Care Record

  * _ISSUE_:   We need a plan to work with various data / dst structures from different systems

    2.  LIB_DX10 Required

   * _NOTE_:  This changes the way you do all your Care Batch  Registrations, updates, New-gens

11.   Billing  :: TODO

  * uscript to deal with new dsts and date swithc
  * parms that call uscript for each payer
  * patch to fix 837 recor loop issue

12.   BUI DX History  - Rob

13.   Reports

