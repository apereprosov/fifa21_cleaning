# Cleaning and Preparing the FIFA 21 Dataset for Analysis

## Introduction
### Description

A project to clean and prepare a dataset of FIFA 21 soccer players using various data processing methods. The aim of the project is to practice skills of working with datasets and prepare data for further analysis.

### Dataset structure

The dataset contains the following parameters:

<img width="679" alt="Screenshot 2024-01-07 at 15 42 35" src="https://github.com/apereprosov/fifa21_cleaning/assets/61319269/cb5f6fdd-f1ac-4a1a-8a91-530fadb6799d">

The dataset contains different types of data such as date, continuous and categorical.

### Project Objective

The purpose of this project was to practice skills in cleaning datasets and preparing data for analysis.

### Project Tasks

1. Bringing columns to a convenient format for further analysis.
2. Processing of parameters requiring additional cleaning and bringing to a common state
3. If necessary, create/delete parameters in the dataset

## STAGE 1: Data Understanding

We have 77 parameters, with no missing values.
We can divide them into subgroups:
- Basic information about the player
  ```
  'ID','Name','LongName','photoUrl','playerUrl','Nationality','Age','↓OVA','POT','Club','Contract'
    
  'Positions','Height','Weight','Preferred Foot','BOV','Best Position','Joined','Loan Date End
      
  'Value','Wage','Release Clause','Total Stats','Base Stats'
  ```
  
- Attributes on attack
  
      'Attacking','Crossing','Finishing','Heading Accuracy','Short Passing','Volleys'
  
- Defense Parameters:
  ```
  'Defending','Marking','Standing Tackle','Sliding Tackle'
  ```
- Skills & Dribbling
  ```
  'Skill','Dribbling','Curve','FK Accuracy','Long Passing','Ball Control'
  'W/F','SM','A/W','D/W','IR'
  ```
- Movement and physical parameters
    ```
    'Movement','Acceleration','Sprint Speed','Agility','Reactions','Balance'
    'Power','Shot Power','Jumping','Stamina','Strength','Long Shots'
    ```
- Mental Characteristics
    ```
    Mentality','Aggression','Interceptions','Positioning','Vision','Penalties','Composure'
    ```
- Goalkeeping skills:
  ```
  'Goalkeeping','GK Diving','GK Handling','GK Kicking','GK Positioning','GK Reflexes'
  'PAC','SHO','PAS','DRI','DEF','PHY'
  ```

## STAGE 2: Data Cleaning & Transforming 

After review, I have identified the parameters that need to be handled

```
'hits', 'height', 'weight', 'wage', 'value', 'release_clause'

'ir','w/f' ,'contract','club'

'loan_date_end','joined'
```
If the other parameters needed to be generalized to a common form/type e.g. 
```python
def to_cm(row):
  if 'cm' in row:
    return float(row.replace('cm','')))
  else:
    feet,inches = row.split('}'')
    inches = inches.replace(''',''')
    return int(feet)*30.48+int(inches)*2.54
```
then the contract column stores information about both the type of the contract and the time of its continuation. I decided to divide the beginning of the lease, beginning and end of the contract, contract type into 3 parameters.
```python
def contract_split(row):
  if '~' in row:
    start_date,end_date = map(float,row.split(' ~ '))
    return start_date,end_date,'Contract'
  else:
    return np.nan,np.nan,row
```
