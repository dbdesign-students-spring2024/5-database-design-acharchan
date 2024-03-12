# Data Normalization and Entity-Relationship Diagramming

## Original data set: Normalization
The original data only complies with 1st normal form and describes the composite primary key of ```assignment_id``` and ```student_id```. Here is the original table, pasted from the [instructions.md](instructions.md) file:

| assignment_id | student_id | due_date | `professor` | assignment_topic                | classroom | grade | relevant_reading    | `professor`_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |


## What makes the original data set not 4NF compliant?
The given table is not in 4NF (Fourth Normal Form) because it contains non-key attributes that are functionally dependent on a non-key attribute, in other words, it contains multi-valued dependencies. As an example, the ``professor`` field is functionally dependent on the `course_id` field, as each course can be taught by multiple `professor`s in different sections, and the `assignment_topic` column is dependent on the `assignment_id` and ``professor`` column.  As long as multi-valued dependencies like this exist, the data set fails to meet 4NF standards.

## Generating 4NF-compliant version of the data set
To achieve fourth normal form compliance for the table, I initiated the normalization process starting from the second normal form. After multiple iterations, I have arrived at this final design, which eliminates redundant data and adheres to the fourth normal form (4NF) by dividing the original `Assignments` table into two separate tables and the original `Readings` table into one. Additionally, I have introduced a new table for `professor`s' assignments to accommodate the requirement that instructors can assign the same task to various sections of the same course, but with distinct due dates.

This design complies with all provided assumptions. It supports courses being taught by multiple `professor`s across different sections, and instructors leading multiple sections of the same course. Each section is associated with a specific classroom and `professor`, and different sections of the same course can convene in different classrooms. `Professor`s can distribute assignments with specific due dates and relevant readings. They have the capability to assign the same task to various sections of the same course with different due dates, and students can complete assignments and receive grades. The new design also offers the flexibility to include multiple entries in each table.

### 4NF Tables
  

**Table 1: Courses**

course_id | course_name
------- | -------
1 | data normalization |
2 | single table queries |
3 | python and pandas |
4 | spreadsheet aggregate functions |  
  
Primary key: `course_id ` 

**Table 2: `Professor`s**  
    
`professor`_id | `professor`_name | email
---------|----------|---------
 1 | Melvin | l.melvin@foo.edu
 2 | Longston | e.longston@foo.edu
 3 | Nevarez | i.nevarez@foo.edu  
   
Primary key: `professor_id  `

**Table 3: Sections**  
  
section_id | course_id | `professor`_id | classroom
------- | ------- | ------- | -------
1 | 1 | 1 | WWH 101
2 | 2 | 2 | 60FA 314
3 | 3 | 2 | 60FA 314
4 | 4 | 3 | WWH 201  

Primary key: `section_id ` 
Foreign keys: `course_id`, `professor_id  `


**Table 4: Assignments**  
  
assignment_id | section_id | assignment_topic | due_date | relevant reading |
------- | ------- | ------- | ------- | ------- |
1 | 1 | data normalization | 23.02.21 | deumlich chapter 3
2 | 2 | single table queries | 18.11.21 | dummlers chapter 11
3 | 3 | python and pandas | 05.05.21 | dummlers chapter 14
4 | 4 | spreadsheet aggregate functions | 04.07.21 | zehnder page 87 
  
Primary key: `assignment_id ` 
Foreign key: `section_id  `

**Table 5: Students**
     
student_id | student_name
------- | -------
1 | john
2 | jane
3 | bob
4 | alice
5 | joe   

Primary key: `student_id`  

**Table 6: Grades**

assignment_id | student_id | grade
------- | ------- | -------
1 | 1 | 80
2 | 5 | 25
3 | 2 | 92
4 | 2 | 65

Primary key: `assignment_id`, `student_id ` 
Foreign keys: `assignment_id`, `student_id  `

**Table 7: Readings**

reading_id | assignment_id | reading
------- | ------- | -------
1 | 1 | deumlich chapter 3
2 | 2 | dummlers chapter 11
3 | 3 | dummlers chapter 14
4 | 4 | zehnder page 87  

Primary key: `reading_id ` 
Foreign key: `assignment_id  `

## ER Diagram  
The diagram illustrates three entities: `assignment`, `submission`, and `professor`. The relationships between these entities have the following cardinalities:

1. `assignment` and `submission` have a one-to-many relationship, where each `assignment` can have multiple `submission`s, but each `submission` corresponds to only one `assignment`.
2. ``professor`` and `assignment` share a many-to-many relationship, meaning that each ``professor`` can be associated with multiple `assignment`s, and each `assignment` can be assigned by multiple `professor`s.
3. `professor` and submission have a one-to-many relationship, indicating that one `professor` can grade multiple `submission`s, while each `submission` is graded by only one `professor`.

In this ER diagram, the relationships between entities are accurately represented, ensuring a clear understanding of the database structure and interactions.

![ER diagram](/images/database-design.drawio.svg .drawio.svg)
![ER diagram](images/database-design.drawio.svg)