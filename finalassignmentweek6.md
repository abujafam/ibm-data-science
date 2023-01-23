## Honors Peer-graded Assignment: Advanced SQL for Data Engineers


My submission : mohammed abu jaafar 



```python
# These libraries are pre-installed in SN Labs. If running in another environment please uncomment lines below to install them:
# !pip install --force-reinstall ibm_db==3.1.0 ibm_db_sa==0.3.3
# Ensure we don't load_ext with sqlalchemy>=1.4 (incompadible)
# !pip uninstall sqlalchemy==1.4 -y && pip install sqlalchemy==1.3.24
# !pip install ipython-sql
```


```python
%load_ext sql
```

    The sql extension is already loaded. To reload it, use:
      %reload_ext sql



```python
# Remember the connection string is of the format:
# %sql ibm_db_sa://my-username:my-password@my-hostname:my-port/my-db-name?security=SSL
# Enter the connection string for your Db2 on Cloud database instance below
%sql ibm_db_sa://qfh63100:y2ctce02XuBTGUv4@824dfd4d-99de-440d-9991-629c01b3832d.bs2io90l08kqb1od8lcg.databases.appdomain.cloud:30119/BLUDB?security=SSL
```




    'Connected: qfh63100@BLUDB'



## Exercise 1: Using Joins

### Question 1
Write and execute a SQL query to list the school names, community names and average attendance for communities with a hardship index of 98





```sql
%%sql
select S.NAME_OF_SCHOOl, S.COMMUNITY_AREA_NAME, S.AVERAGE_STUDENT_ATTENDANCE, CS.HARDSHIP_INDEX
FROM CHICAGOSCHOOLS S LEFT JOIN  CHICAGOCENSUS CS ON  S.COMMUNITY_AREA_NUMBER =CS.COMMUNITY_AREA_NUMBER 
WHERE CS.HARDSHIP_INDEX = 98

```

     * ibm_db_sa://qfh63100:***@824dfd4d-99de-440d-9991-629c01b3832d.bs2io90l08kqb1od8lcg.databases.appdomain.cloud:30119/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>name_of_school</th>
            <th>community_area_name</th>
            <th>average_student_attendance</th>
            <th>hardship_index</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>George Washington Carver Military Academy High School</td>
            <td>RIVERDALE</td>
            <td>91.60%</td>
            <td>98</td>
        </tr>
        <tr>
            <td>George Washington Carver Primary School</td>
            <td>RIVERDALE</td>
            <td>90.90%</td>
            <td>98</td>
        </tr>
        <tr>
            <td>Ira F Aldridge Elementary School</td>
            <td>RIVERDALE</td>
            <td>92.90%</td>
            <td>98</td>
        </tr>
        <tr>
            <td>William E B Dubois Elementary School</td>
            <td>RIVERDALE</td>
            <td>93.30%</td>
            <td>98</td>
        </tr>
    </tbody>
</table>



### Question 2
 

Write and execute a SQL query to list all crimes that took place at a school. Include case number, crime type and community name.



```sql
%%sql
SELECT C.CASE_NUMBER, C.PRIMARY_TYPE, CS.COMMUNITY_AREA_NAME 
FROM CRIME C LEFT OUTER JOIN CHICAGOCENSUS CS 
ON C.COMMUNITY_AREA_NUMBER = CS.COMMUNITY_AREA_NUMBER 
WHERE C.LOCATION_DESCRIPTION LIKE '%SCHOOL%'

```

     * ibm_db_sa://qfh63100:***@824dfd4d-99de-440d-9991-629c01b3832d.bs2io90l08kqb1od8lcg.databases.appdomain.cloud:30119/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>case_number</th>
            <th>primary_type</th>
            <th>community_area_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HK577020</td>
            <td>NARCOTICS</td>
            <td>Rogers Park</td>
        </tr>
        <tr>
            <td>HL725506</td>
            <td>BATTERY</td>
            <td>Lincoln Square</td>
        </tr>
        <tr>
            <td>HH639427</td>
            <td>BATTERY</td>
            <td>Austin</td>
        </tr>
        <tr>
            <td>HS200939</td>
            <td>CRIMINAL DAMAGE</td>
            <td>Austin</td>
        </tr>
        <tr>
            <td>HT315369</td>
            <td>ASSAULT</td>
            <td>East Garfield Park</td>
        </tr>
        <tr>
            <td>HP716225</td>
            <td>BATTERY</td>
            <td>Douglas</td>
        </tr>
        <tr>
            <td>HL353697</td>
            <td>BATTERY</td>
            <td>South Shore</td>
        </tr>
        <tr>
            <td>HS305355</td>
            <td>NARCOTICS</td>
            <td>Brighton Park</td>
        </tr>
        <tr>
            <td>JA460432</td>
            <td>BATTERY</td>
            <td>Ashburn</td>
        </tr>
        <tr>
            <td>HR585012</td>
            <td>CRIMINAL TRESPA</td>
            <td>Ashburn</td>
        </tr>
        <tr>
            <td>HH292682</td>
            <td>PUBLIC PEACE VI</td>
            <td>None</td>
        </tr>
        <tr>
            <td>G635735</td>
            <td>PUBLIC PEACE VI</td>
            <td>None</td>
        </tr>
    </tbody>
</table>



## Exercise 2: Creating a View
For privacy reasons, you have been asked to create a view that enables users to select just the school name and the icon fields from the CHICAGO_PUBLIC_SCHOOLS table. By providing a view, you can ensure that users cannot see the actual scores given to a school, just the icon associated with their score. You should define new names for the view columns to obscure the use of scores and icons in the original table.

### Question 1
Write and execute a SQL statement to create a view showing the columns listed in the following table, with new column names as shown in the second column.
 Column name in CHICAGO_PUBLIC_SCHOOLS	Column name in view



```sql
%%sql 
DROP VIEW SCHOOL_VIEW
```

     * ibm_db_sa://qfh63100:***@824dfd4d-99de-440d-9991-629c01b3832d.bs2io90l08kqb1od8lcg.databases.appdomain.cloud:30119/BLUDB
    Done.





    []




```sql
%%sql 

CREATE VIEW SCHOOL_VIEW(School_Name, Safety_Rating,Family_Rating,Environment_Rating,Instruction_Rating,Leaders_Rating,Teachers_Rating) 
AS SELECT NAME_OF_SCHOOL,Safety_Icon,Family_Involvement_Icon, Environment_Icon,  Instruction_Icon, Leaders_Icon, Teachers_Icon 
FROM CHICAGOSCHOOLS;

```

     * ibm_db_sa://qfh63100:***@824dfd4d-99de-440d-9991-629c01b3832d.bs2io90l08kqb1od8lcg.databases.appdomain.cloud:30119/BLUDB
    Done.





    []




```sql
%%sql  --Write and execute a SQL statement that returns all of the columns from the view.
select COLNAME, TYPENAME from SYSCAT.COLUMNS WHERE TABNAME = 'SCHOOL_VIEW'
```

     * ibm_db_sa://qfh63100:***@824dfd4d-99de-440d-9991-629c01b3832d.bs2io90l08kqb1od8lcg.databases.appdomain.cloud:30119/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>colname</th>
            <th>typename</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>SCHOOL_NAME</td>
            <td>VARCHAR</td>
        </tr>
        <tr>
            <td>SAFETY_RATING</td>
            <td>VARCHAR</td>
        </tr>
        <tr>
            <td>FAMILY_RATING</td>
            <td>VARCHAR</td>
        </tr>
        <tr>
            <td>ENVIRONMENT_RATING</td>
            <td>VARCHAR</td>
        </tr>
        <tr>
            <td>INSTRUCTION_RATING</td>
            <td>VARCHAR</td>
        </tr>
        <tr>
            <td>LEADERS_RATING</td>
            <td>VARCHAR</td>
        </tr>
        <tr>
            <td>TEACHERS_RATING</td>
            <td>VARCHAR</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
--Write and execute a SQL statement that returns just the school name and leaders rating from the view
select school_name,leaders_rating FROM SCHOOL_VIEW LIMIT 5
```

     * ibm_db_sa://qfh63100:***@824dfd4d-99de-440d-9991-629c01b3832d.bs2io90l08kqb1od8lcg.databases.appdomain.cloud:30119/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>school_name</th>
            <th>leaders_rating</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Abraham Lincoln Elementary School</td>
            <td>Weak</td>
        </tr>
        <tr>
            <td>Adam Clayton Powell Paideia Community Academy Elementary School</td>
            <td>Weak</td>
        </tr>
        <tr>
            <td>Adlai E Stevenson Elementary School</td>
            <td>Weak</td>
        </tr>
        <tr>
            <td>Agustin Lara Elementary Academy</td>
            <td>Weak</td>
        </tr>
        <tr>
            <td>Air Force Academy High School</td>
            <td>Weak</td>
        </tr>
    </tbody>
</table>



## Exercise 3: Creating a Stored Procedure
### Question 1
Write the structure of a query to create or replace a stored procedure called UPDATE_LEADERS_SCORE that takes a in_School_ID parameter as an integer and a in_Leader_Score parameter as an integer. Don’t forget to use the #SET TERMINATOR statement to use the @ for the CREATE statement terminator.

### Question 2
Inside your stored procedure, write a SQL statement to update the Leaders_Score field in the CHICAGO_PUBLIC_SCHOOLS table for the school identified by in_School_ID to the value in the in_Leader_Score parameter.
Take a screenshot showing the SQL query.

### Question 3
Inside your stored procedure, write a SQL IF statement to update the Leaders_Icon field in the CHICAGO_PUBLIC_SCHOOLS table for the school identified by in_School_ID using the following information.





```sql
%%sql
--#SET TERMINATOR @
CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
    IN in_School_ID  INTEGER, IN in_Leader_Score INTEGER) 
LANGUAGE SQL 
MODIFIES SQL DATA
  BEGIN
    UPDATE "CHICAGOSCHOOLS"
    SET Leaders_Score = in_Leader_Score
    WHERE School_ID = in_School_ID;
    IF in_Leader_Score >=  80 THEN 
        UPDATE "CHICAGOSCHOOLS"
        SET Leaders_Icon = 'Very_Strong'
        WHERE School_ID = in_School_ID;
    ELSEIF in_Leader_Score>= 60 and in_Leader_Score <= 79  THEN
        UPDATE "CHICAGOSCHOOLS"
        SET Leaders_Icon = 'Strong'
        WHERE School_ID = in_School_ID;
    ELSEIF in_Leader_Score >=  40 and in_Leader_Score <= 59  THEN
        UPDATE "CHICAGOSCHOOLS"
        SET Leaders_Icon = 'Average'
        WHERE School_ID = in_School_ID;
    ELSEIF in_Leader_Score >=  20 and in_Leader_Score <= 39  THEN
        UPDATE "CHICAGOSCHOOLS"
        SET Leaders_Icon = 'Weak'
        WHERE School_ID = in_School_ID;
    ELSE
        UPDATE "CHICAGOSCHOOLS"
        SET Leaders_Icon = 'Very Weak'
        WHERE School_ID = in_School_ID;
    END IF;
 
        IF retcode < 0 THEN                                  --  SQLCODE returns negative value for error, zero for success, positive value for warning
      ROLLBACK WORK;
        
     ELSE
        COMMIT WORK;
        
      END IF;
 
 
 
 END 
  @
  
```

     * ibm_db_sa://qfh63100:***@824dfd4d-99de-440d-9991-629c01b3832d.bs2io90l08kqb1od8lcg.databases.appdomain.cloud:30119/BLUDB
    (ibm_db_dbi.ProgrammingError) ibm_db_dbi::ProgrammingError: Statement Execute Failed: [IBM][CLI Driver][DB2/LINUXX8664] SQL0206N  "RETCODE" is not valid in the context where it is used.  LINE NUMBER=32.  SQLSTATE=42703 SQLCODE=-206
    [SQL: --#SET TERMINATOR @
    CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
        IN in_School_ID  INTEGER, IN in_Leader_Score INTEGER) 
    LANGUAGE SQL 
    MODIFIES SQL DATA
      BEGIN
        UPDATE "CHICAGOSCHOOLS"
        SET Leaders_Score = in_Leader_Score
        WHERE School_ID = in_School_ID;
        IF in_Leader_Score >=  80 THEN 
            UPDATE "CHICAGOSCHOOLS"
            SET Leaders_Icon = 'Very_Strong'
            WHERE School_ID = in_School_ID;
        ELSEIF in_Leader_Score>= 60 and in_Leader_Score <= 79  THEN
            UPDATE "CHICAGOSCHOOLS"
            SET Leaders_Icon = 'Strong'
            WHERE School_ID = in_School_ID;
        ELSEIF in_Leader_Score >=  40 and in_Leader_Score <= 59  THEN
            UPDATE "CHICAGOSCHOOLS"
            SET Leaders_Icon = 'Average'
            WHERE School_ID = in_School_ID;
        ELSEIF in_Leader_Score >=  20 and in_Leader_Score <= 39  THEN
            UPDATE "CHICAGOSCHOOLS"
            SET Leaders_Icon = 'Weak'
            WHERE School_ID = in_School_ID;
        ELSE
            UPDATE "CHICAGOSCHOOLS"
            SET Leaders_Icon = 'Very Weak'
            WHERE School_ID = in_School_ID;
        END IF;
     
            IF retcode < 0 THEN                                  --  SQLCODE returns negative value for error, zero for success, positive value for warning
          ROLLBACK WORK;
            
         ELSE
            COMMIT WORK;
            
          END IF;
     
     
     
     END 
      @]
    (Background on this error at: http://sqlalche.me/e/13/f405)


### Question 4
 * Run your code to create the stored procedure.
 
**Take a screenshot showing the SQL query and its results.**

* Write a query to call the stored procedure, passing a valid school ID and a leader score of 50, to check that the procedure works as expected.


```sql
%%sql
SELECT SCHOOL_ID, LEADERS_SCORE, LEADERS_ICON FROM CHICAGOSCHOOLS 
WHERE LEADERS_SCORE = '38'
```

     * ibm_db_sa://qfh63100:***@824dfd4d-99de-440d-9991-629c01b3832d.bs2io90l08kqb1od8lcg.databases.appdomain.cloud:30119/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>school_id</th>
            <th>leaders_score</th>
            <th>leaders_icon</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>610038</td>
            <td>38</td>
            <td>Weak</td>
        </tr>
        <tr>
            <td>609709</td>
            <td>38</td>
            <td>Weak</td>
        </tr>
        <tr>
            <td>609861</td>
            <td>38</td>
            <td>Weak</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
    CALL UPDATE_LEADERS_SCORE (609974,90);
    SELECT SCHOOL_ID, LEADERS_SCORE, LEADERS_ICON FROM CHICAGOSCHOOLS 
    WHERE LEADERS_SCORE = '90' --or LEADERS_SCORE = '101'

```

     * ibm_db_sa://qfh63100:***@824dfd4d-99de-440d-9991-629c01b3832d.bs2io90l08kqb1od8lcg.databases.appdomain.cloud:30119/BLUDB
    Done.
    Done.





<table>
    <thead>
        <tr>
            <th>school_id</th>
            <th>leaders_score</th>
            <th>leaders_icon</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>609974</td>
            <td>90</td>
            <td>Very_Strong</td>
        </tr>
    </tbody>
</table>



## Exercise 4: Using Transactions

### Question 1
Update your stored procedure definition. Add a generic ELSE clause to the IF statement that rolls back the current work if the score did not fit any of the preceding categories.


```sql
%%sql
--#SET TERMINATOR @
CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
    IN in_School_ID  INTEGER, IN in_Leader_Score INTEGER) 
LANGUAGE SQL 
MODIFIES SQL DATA
  BEGIN
 
        DECLARE SQLCODE INTEGER DEFAULT 0;                  -- Host variable SQLCODE declared and assigned 0
        DECLARE retcode INTEGER DEFAULT 0;                  -- Local variable retcode with declared and assigned 0
        DECLARE CONTINUE HANDLER FOR SQLEXCEPTION           -- Handler tell the routine what to do when an error or warning occurs
        SET retcode = SQLCODE;                              -- Value of SQLCODE assigned to local variable retcode
        
    UPDATE "CHICAGOSCHOOLS"
    SET Leaders_Score = in_Leader_Score
    WHERE School_ID = in_School_ID;
    IF in_Leader_Score >=  80 THEN 
        UPDATE "CHICAGOSCHOOLS"
        SET Leaders_Icon = 'Very_Strong'
        WHERE School_ID = in_School_ID;
    ELSEIF in_Leader_Score>= 60 and in_Leader_Score <= 79  THEN
        UPDATE "CHICAGOSCHOOLS"
        SET Leaders_Icon = 'Strong'
        WHERE School_ID = in_School_ID;
    ELSEIF in_Leader_Score >=  40 and in_Leader_Score <= 59  THEN
        UPDATE "CHICAGOSCHOOLS"
        SET Leaders_Icon = 'Average'
        WHERE School_ID = in_School_ID;
    ELSEIF in_Leader_Score >=  20 and in_Leader_Score <= 39  THEN
        UPDATE "CHICAGOSCHOOLS"
        SET Leaders_Icon = 'Weak'
        WHERE School_ID = in_School_ID;
    ELSE
        UPDATE "CHICAGOSCHOOLS"
        SET Leaders_Icon = 'Very Weak'
        WHERE School_ID = in_School_ID;
    END IF;
  
    IF retcode < 0 THEN                                  --  SQLCODE returns negative value for error, zero for success, positive value for warning
      ROLLBACK WORK;
        
     ELSE
        COMMIT WORK;
        
      END IF;
  
  
  
  
  
  
  END 
  @
  

```

     * ibm_db_sa://qfh63100:***@824dfd4d-99de-440d-9991-629c01b3832d.bs2io90l08kqb1od8lcg.databases.appdomain.cloud:30119/BLUDB
    (ibm_db_dbi.ProgrammingError) ibm_db_dbi::ProgrammingError: Statement Execute Failed: [IBM][CLI Driver][DB2/LINUXX8664] SQL0778N  End label "@" is not the same as the begin label.  LINE NUMBER=51.  SQLSTATE=428D5 SQLCODE=-778
    [SQL: --#SET TERMINATOR @
    CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
        IN in_School_ID  INTEGER, IN in_Leader_Score INTEGER) 
    LANGUAGE SQL 
    MODIFIES SQL DATA
      BEGIN
     
            DECLARE SQLCODE INTEGER DEFAULT 0;                  -- Host variable SQLCODE declared and assigned 0
            DECLARE retcode INTEGER DEFAULT 0;                  -- Local variable retcode with declared and assigned 0
            DECLARE CONTINUE HANDLER FOR SQLEXCEPTION           -- Handler tell the routine what to do when an error or warning occurs
            SET retcode = SQLCODE;                              -- Value of SQLCODE assigned to local variable retcode
            
        UPDATE "CHICAGOSCHOOLS"
        SET Leaders_Score = in_Leader_Score
        WHERE School_ID = in_School_ID;
        IF in_Leader_Score >=  80 THEN 
            UPDATE "CHICAGOSCHOOLS"
            SET Leaders_Icon = 'Very_Strong'
            WHERE School_ID = in_School_ID;
        ELSEIF in_Leader_Score>= 60 and in_Leader_Score <= 79  THEN
            UPDATE "CHICAGOSCHOOLS"
            SET Leaders_Icon = 'Strong'
            WHERE School_ID = in_School_ID;
        ELSEIF in_Leader_Score >=  40 and in_Leader_Score <= 59  THEN
            UPDATE "CHICAGOSCHOOLS"
            SET Leaders_Icon = 'Average'
            WHERE School_ID = in_School_ID;
        ELSEIF in_Leader_Score >=  20 and in_Leader_Score <= 39  THEN
            UPDATE "CHICAGOSCHOOLS"
            SET Leaders_Icon = 'Weak'
            WHERE School_ID = in_School_ID;
        ELSE
            UPDATE "CHICAGOSCHOOLS"
            SET Leaders_Icon = 'Very Weak'
            WHERE School_ID = in_School_ID;
        END IF;
      
        IF retcode < 0 THEN                                  --  SQLCODE returns negative value for error, zero for success, positive value for warning
          ROLLBACK WORK;
            
         ELSE
            COMMIT WORK;
            
          END IF;
      
      
      
      
      
      
      END 
      @]
    (Background on this error at: http://sqlalche.me/e/13/f405)


## AUTHOR 
Mohammed abu jaafar


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```
