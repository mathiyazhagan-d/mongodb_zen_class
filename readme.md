
# day 38 task Database - MongoDB




## create zen class database
``` 
use zen_class
```
<img src="./image/img (1).png" alt="ans-1">



 ## insert the all collection i needed (users,codekata,attendance,topics,tasks,company_drives,mentors) 

<img src="./image/img (2).png" alt="ans-1">

## 1). Find all the topics and tasks which are thought in the month of October
```
db.topics.aggregate([
    {
     $lookup:{
         from:"tasks",
         localField:"id",
         foreignField:"id",
         as:"task_information"
     }
   },
     { $match:{ $and:[{ $and:[
                     {topic_date:{$gt:new Date ("2020-10-01")}},
                     {topic_date:{$lt:new Date ("2020-10-30")}}
                     ] } ,
                 {$and:[ {"task_information.due_date":{$gt:new Date ("2020-10-01")}},
                         {"task_information.due_date":{$lt:new Date ("2020-10-30")}}
                         ]} ]}
     }    
   ]);
```
<img src="./image/img (3).png" alt="ans-1">

## 2). Find all the company drives which appeared between 15 oct-2020 and 31-oct-2020
```
db.company_drives.find({
    $and: [
      { drive_date: { $lte: new Date("2020-10-31") } },
      { drive_date: { $gte: new Date("2020-10-15") } },
    ],
  }).toArray();
  ```
<img src="./image/img (4).png" alt="ans-1">

## 3). Find all the company drives and students who are appeared for the placement.
```
  db.company_drives.aggregate([
    {
        $lookup: {
              from:"users",
              localField:"id",
              foreignField:"id",
              as :"user_information"
             }
    },
    {
        $project:{
            _id:0,
            "user_information.name":1,
            company_name:1,
            drive_date:1,
            "user_information.email":1,
            "user_information.user_id":1
        }
    }
]).toArray();
```
<img src="./image/img (5).png" alt="ans-1">

## 4). Find the number of problems solved by the user in codekata
```
db.codekata.aggregate([
    {
      $lookup: {
        from: "users",
        localField: "id",
        foreignField: "id",
        as: "user_information",
      },
    },
    {
      $project: {
        _id: 0,
        user_id: 1,
        "user_information.name": 1,
        "user_information.email": 1,
        no_of_problems_solved: 1,
      },
    },
  ]).toArray();
  ```
<img src="./image/img (6).png" alt="ans-1">

## 5). Find all the mentors with who has the mentee's count more than 15
```
db.mentors.find({
    mentee_count:{ $gt:15 }
  }).toArray();
<img src="./image/img (7).png" alt="ans-1">

## 6). Find the number of users who are absent and task is not submitted  between 15 oct-2020 and 31-oct-2020

db.tasks.aggregate([
    {
      $lookup: {
        from: "attendance",
        localField: "id",
        foreignField: "id",
        as: "attendance",
      },
    },
    {
      $match: {
        $and: [{ submitted: false }, { "attendance.present": false }],
      },
    },
  ]).toArray();
```
  <img src="./image/img (8).png" alt="ans-1">