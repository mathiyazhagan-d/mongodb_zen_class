
# day 38 task Database - MongoDB




## create zen class database
``` 
use zen_class
```
<img src="./image/img (1).png" alt="ans-1">



 ## insert the all collection needed (users,codekata,attendance,topics,tasks,company_drives,mentors) 
```
// users

 db.users.insertMany([
    {
      id: 1,
      name: "Mathi",
      email: "Mathisa@gmail.com",
    },  
    {
      id: 2,
      name: "sam",
      email: "sam1234335r@gmail.com",
    },
    {
      id: 3,
      name: "Pavan",
      email: "Pavankumar@gmail.com",
    },
    {
      id: 4,
      name: "Shiek",
      email: "Shiek23452hj@gmail.com",
    },
    {
      id: 5,
      name: "Ajith",
      email: "Ajithw4gh537a@gmail.com",
    },
  ]);

// codekata

  db.codekata.insertMany([
  {
    id: 1,
    no_of_problems_solved: 105,
  },
  {
    id: 2,
    no_of_problems_solved: 120,
  },
  {
    id: 3,
    no_of_problems_solved: 95,
  },
  {
    id: 4,
    no_of_problems_solved: 70,
  },
  {
    id: 5,
    no_of_problems_solved: 90,
  },
]);

// attendance

db.attendance.insertMany([
    {
      id: 1,
      present: true,
    },
    {
      id: 2,
      present: true,
    },
    {
      id: 3,
      present: false,
    },
    {
      id: 4,
      present: false,
    },
    {
      id: 5,
      present: true,
    },
  ]);

// topics

db.topics.insertMany([
    {
      id: 1,
      topic: "HTML",
      topic_date: new Date("2020-9-01"),
    },
    {
      id: 2,
      topic: "CSS",
      topic_date: new Date("2020-9-10"),
    },
    {
      id: 3,
      topic: "Javascript",
      topic_date: new Date("2020-9-15"),
    },
    {
      id: 4,
      topic: "ReactJS",
      topic_date: new Date("2020-10-20"),
    },
    {
      id: 5,
      topic: "NodeJS",
      topic_date: new Date("2020-10-25"),
    },
  ]);

// tasks

db.tasks.insertMany([
    {
      id: 1,
      task: "HTML task",
      due_date: new Date("2020-9-05"),
      submitted: true,
    },
    {
      id: 2,
      task: "CSS task",
      due_date: new Date("2020-9-15"),
      submitted: true,
    },
    {
      id: 3,
      task: "Javascript task",
      due_date: new Date("2020-9-20"),
      submitted: true,
    },
    {
      id: 4,
      task: "ReactJS task",
      due_date: new Date("2020-10-25"),
      submitted: false,
    },
    {
      id: 5,
      task: "NodeJS task",
      due_date: new Date("2020-10-29"),
      submitted: false,
    },
  ]);

// company_drives

db.company_drives.insertMany([
    {
      id: 1,
      drive_date: new Date("2020-9-05"),
      company_name: "realme",
    },
    {
      id: 1,
      drive_date: new Date("2020-10-10"),
      company_name: "Amazon",
    },
    {
      id: 2,
      drive_date: new Date("2020-10-20"),
      company_name: "infotech",
    },
    {
      id: 3,
      drive_date: new Date("2020-10-15"),
      company_name: "Zoho",
    },
    {
      id: 5,
      drive_date: new Date("2020-9-30"),
      company_name: "dell",
    },
  ]);

// mentors

db.mentors.insertMany([
    {
      id: 1,
      mentor_name: "Sanjai",
      mentor_email: "Sanjai3456yw@gmail.com",
      mentee_count: 40,
    },
    {
      id: 2,
      mentor_name: "Ruban",
      mentor_email: "Rubanka84n@gmail.com",
      mentee_count: 35,
    },
    {
      id: 3,
      mentor_name: "Arun kumar",
      mentor_email: "Arunkumare352@gmail.com",
      mentee_count: 20,
    },
    {
      id: 4,
      mentor_name: "krishna",
      mentor_email: "krishnagsdr4432@gmail.com",
      mentee_count: 32,
    },
    {
      id: 5,
      mentor_name: "Saran",
      mentor_email: "Saran4t23knj@gmail.com",
      mentee_count: 6,
    },
  ]);
```
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
  ```
<img src="./image/img (7).png" alt="ans-1">

## 6). Find the number of users who are absent and task is not submitted  between 15 oct-2020 and 31-oct-2020
```
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
