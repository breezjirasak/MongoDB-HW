# MongoDB Database for Student Marks


## Create data

``` 
use students 
```

## Insert data

```
db.students.insertMany(
[   {"name":"Ramesh","subject":"maths","marks":87},   
    {"name":"Ramesh","subject":"english","marks":59},   
    {"name":"Ramesh","subject":"science","marks":77},   
    {"name":"Rav","subject":"maths","marks":62},   
    {"name":"Rav","subject":"english","marks":83},   
    {"name":"Rav","subject":"science","marks":71},   
    {"name":"Alison","subject":"maths","marks":84},   
    {"name":"Alison","subject":"english","marks":82},  
    {"name":"Alison","subject":"science","marks":86},   
    {"name":"Steve","subject":"maths","marks":81},   
    {"name":"Steve","subject":"english","marks":89},   
    {"name":"Steve","subject":"science","marks":77},   
    {"name":"Jan","subject":"english","marks":0,"reason":"absent"}
])
```

## 1. Find the total marks for each student across all subjects.

```
db.students.aggregate([
   { $group: { _id: "$name", total_marks: { $sum: "$marks" } } }
])
```

Result of the following:
```
[
  { _id: 'Rav', total_marks: 216 },
  { _id: 'Steve', total_marks: 247 },
  { _id: 'Jan', total_marks: 0 },
  { _id: 'Alison', total_marks: 252 },
  { _id: 'Ramesh', total_marks: 223 }
]
```

## 2. Find the maximum marks scored in each subject.

```
db.students.aggregate([
   { $group: { _id: "$subject", max_marks: { $max: "$marks" } } }
])
```

Result of the following:
```
[
  { _id: 'maths', max_marks: 87 },
  { _id: 'science', max_marks: 86 },
  { _id: 'english', max_marks: 89 }
]
```

## 3. Find the minimum marks scored by each student.

```
db.students.aggregate([
   { $group: { _id: "$name", min_marks: { $min: "$marks" } } }
])
```

Result of the following:
```
[
  { _id: 'Ramesh', min_marks: 59 },
  { _id: 'Steve', min_marks: 77 },
  { _id: 'Jan', min_marks: 0 },
  { _id: 'Alison', min_marks: 82 },
  { _id: 'Rav', min_marks: 62 }
]
```

## 4. Find the top two subjects based on average marks.
```
db.students.aggregate([
  { $group: { _id: "$subject", avg_marks: { $avg: "$marks" } } },{ $sort: { avg_marks: -1 } },{ $limit: 2 }
])
```

Result of the following:
```
[
  { _id: 'maths', avg_marks: 78.5 },
  { _id: 'science', avg_marks: 77.75 }
]
```
