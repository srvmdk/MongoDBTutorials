## MongoDB: [Aggregation pipelines](https://docs.mongodb.com/manual/reference/operator/aggregation-pipeline/)

---

### Table contents

| No. | Methods                                                                                               |
| --- | ----------------------------------------------------------------------------------------------------- |
| 1.  | [Lookup aggregation](#lookup-aggregation)                                                             |
| 2.  | [Match aggregation](#match-aggregation)                                                               |
| 3.  | [Group aggregation](#group-aggregation)                                                               |
| 4.  | [Project aggregation](#project-aggregation)                                                           |
| 5.  | [Unwind aggregation](#unwind-aggregation)                                                             |
| 6.  | [Projection with arrays - slice operator](#projection-with-arrays-slice-operator)                     |
| 7.  | [Projection with arrays - size operator](#projection-with-arrays-size-operator)                       |
| 8.  | [Projection with arrays - filter operator](#projection-with-arrays-filter-operator)                   |
| 9.  | [Projection with arrays - using multiple operators](#projection-with-arrays-using-multiple-operators) |
| 10. | [Bucket aggregation](#bucket-aggregation)                                                             |
| 11. | [Skip and Limit aggregation](#skip-and-limit-aggregation)                                             |

---

1. ### Lookup aggregation

**_Query to provide all the friends along with their address_**
ref: [friends.json](/json/friends.json), [friends_address.json](/json/friends_address.json)

```js
db.friends.aggregate([
	{
		$lookup: {
			from: "friends_address",
			localField: "_id",
			foreignField: "_id",
			as: "location",
		},
	},
]);
```

_Notes:_
_$lookup_ : merges reference relations from foreign collections
_$lookup_ requires 4 parameters:

- _from_ : foreign collection name
- _localField_ : field in local collection whose value to be matched with foreign collection referenced
- _foreignField_ : field in referenced foreign collection whose value to be matched with local collection
- _as_ : alias, under this the referenced collection details will be shown

_Output:_

```json
{
        "_id" : ObjectId("60aa850e452e992ccc87e359"),
        "name" : "Max",
        "hobbies" : [
                "Sports",
                "Cooking"
        ],
        "age" : 29,
        "examScores" : [
                {
                        "difficulty" : 4,
                        "score" : 57.9
                },
                {
                        "difficulty" : 6,
                        "score" : 62.1
                },
                {
                        "difficulty" : 3,
                        "score" : 88.5
                }
        ],
        "location" : [
                {
                        "_id" : ObjectId("60aa850e452e992ccc87e359"),
                        "address" : {
                                "street" : "3287 high street",
                                "city" : "carlow",
                                "state" : "wexford",
                                "postcode" : 47671
                        }
                }
        ]
}
{
        "_id" : ObjectId("60aa850e452e992ccc87e35a"),
        "name" : "Manu",
        "hobbies" : [
                "Eating",
                "Data Analytics"
        ],
        "age" : 30,
        "examScores" : [
                {
                        "difficulty" : 7,
                        "score" : 52.1
                },
                {
                        "difficulty" : 2,
                        "score" : 74.3
                },
                {
                        "difficulty" : 5,
                        "score" : 53.1
                }
        ],
        "location" : [
                {
                        "_id" : ObjectId("60aa850e452e992ccc87e35a"),
                        "address" : {
                                "street" : "4962 36th ave",
                                "city" : "jasper",
                                "state" : "northwest territories",
                                "postcode" : "U0A 4J6"
                        }
                }
        ]
}
{
        "_id" : ObjectId("60aa850e452e992ccc87e35b"),
        "name" : "Maria",
        "hobbies" : [
                "Cooking",
                "Skiing"
        ],
        "age" : 29,
        "examScores" : [
                {
                        "difficulty" : 3,
                        "score" : 75.1
                },
                {
                        "difficulty" : 8,
                        "score" : 44.2
                },
                {
                        "difficulty" : 6,
                        "score" : 61.5
                }
        ],
        "location" : [
                {
                        "_id" : ObjectId("60aa850e452e992ccc87e35b"),
                        "address" : {
                                "street" : "2575 abanoz sk",
                                "city" : "elazığ",
                                "state" : "kırşehir",
                                "postcode" : 48715
                        }
                }
        ]
}
```

**[👆 Top](#table-contents)**

---

2. ### Match aggregation

**_Query to provide all the records for females_**
ref: [persons.json](/json/persons.json)

```js
db.persons.aggregate([{ $match: { gender: "female" } }]);
// to pretty print, use .pretty() at end
```

_Sample output:_

```json
{
  "_id" : ObjectId("60a995e128b70605e04e8811"),
  "gender" : "female",
  "name" : {
    "title" : "miss",
    "first" : "maeva",
    "last" : "wilson"
  },
  "location" : {
    "street" : "4962 36th ave",
    "city" : "jasper",
    "state" : "northwest territories",
    "postcode" : "U0A 4J6",
    "coordinates" : {
      "latitude" : "-31.6359",
      "longitude" : "111.3806"
    },
    "timezone" : {
      "offset" : "+9:00",
      "description" : "Tokyo, Seoul, Osaka, Sapporo, Yakutsk"
    }
  },
  "email" : "maeva.wilson@example.com",
  "login" : {
    "uuid" : "8f2d499c-6a7e-4606-8c58-211fdb880c31",
    "username" : "smallgoose815",
    "password" : "weston",
    "salt" : "TyL1q8hK",
    "md5" : "bcedea61753320688ea27be01982556d",
    "sha1" : "9e075df851fdaf292170d8e0a92c19ee37fba0f2",
    "sha256" : "6f022405c6a384de3ab5cfc98cccdd97570e5b412046d05dfb79c23ae37612fa"
  },
  "dob" : {
    "date" : "1962-08-11T20:51:07Z",
    "age" : 56
  },
  "registered" : {
    "date" : "2016-10-03T10:02:55Z",
    "age" : 1
  },
  "phone" : "727-218-6012",
  "cell" : "443-382-6538",
  "id" : {
    "name" : "",
    "value" : null
  },
  "picture" : {
    "large" : "https://randomuser.me/api/portraits/women/96.jpg",
    "medium" : "https://randomuser.me/api/portraits/med/women/96.jpg",
    "thumbnail" : "https://randomuser.me/api/portraits/thumb/women/96.jpg"
  },
  "nat" : "CA"
}
// ... many more
```

**[👆 Top](#table-contents)**

---

3. ### Group aggregation

**_Write a query to provide state-wise female count in descending order_**
ref: [persons.json](/json/persons.json)

```js
db.persons.aggregate([
	{ $match: { gender: "female" } },
	{
		$group: {
			_id: {
				state: "$location.state",
			},
			totalFemales: { $sum: 1 },
		},
	},
	{ $sort: { totalFemales: -1 } },
]);
```

_Note:_

- _\_id_ is mandatory for group : determines which field to group
- _$sort_ : sorts the output in _assending \(1)_ or _descending \(-1)_

_Sample output:_

```json
{ "_id" : { "state" : "midtjylland" }, "totalFemales" : 33 }
{ "_id" : { "state" : "nordjylland" }, "totalFemales" : 27 }
{ "_id" : { "state" : "new south wales" }, "totalFemales" : 24 }
{ "_id" : { "state" : "australian capital territory" }, "totalFemales" : 24 }
{ "_id" : { "state" : "syddanmark" }, "totalFemales" : 24 }
// ... many more
```

**[👆 Top](#table-contents)**

---

4. ### Project aggregation

#### Scenario - 1

**_Write a query to project only fullname (new field), gender & email address_**
_fullname will contain a concatinated version of title (uppercase), first(capitalize first letter), last_
ref: [persons.json](/json/persons.json)

```js
db.persons.aggregate([
	{
		$project: {
			_id: 0,
			fullname: {
				$concat: [
					{ $toUpper: "$name.title" },
					" ",
					{ $toUpper: { $substrCP: ["$name.first", 0, 1] } },
					{
						$substrCP: [
							"$name.first",
							1,
							{ $subtract: [{ $strLenCP: "$name.first" }, 1] },
						],
					},
					" ",
					"$name.last",
				],
			},
			gender: 1,
			email: 1,
		},
	},
]);
```

_Operators used:_

- _$concat_ : concatenates strings or a document value
- _$toUpper_ : converts to uppercase
- _$substrCP_ : provides required part of a string
- _$subtract_ : perform subtraction of the values provided in an array format

_Sample output:_

```json
{ "gender" : "male", "email" : "victor.pedersen@example.com", "fullname" : "MR Victor pedersen" }
{ "gender" : "male", "email" : "carl.jacobs@example.com", "fullname" : "MR Carl jacobs" }
{ "gender" : "male", "email" : "zachary.lo@example.com", "fullname" : "MR Zachary lo" }
{ "gender" : "male", "email" : "harvey.chambers@example.com", "fullname" : "MR Harvey chambers" }
{ "gender" : "male", "email" : "gideon.vandrongelen@example.com", "fullname" : "MR Gideon van drongelen" }
// ... many more
```

#### Scenario - 2

**_Write a query to project first name, age, dob, location \(only coordinate)_**
_location will include a new field "type: 'Point'" and coordinates should have valid format of longitude & latitude_
_age and birth date \(dob.date in valid date format) to be flattened_
ref: [persons.json](/json/persons.json)

```js
db.persons.aggregate([
	{
		$project: {
			_id: 0,
			firstname: "$name.first",
			location: {
				coordinates: 1,
			},
			age: "$dob.age",
			dob: "$dob.date",
		},
	},
	{
		$project: {
			firstname: 1,
			age: 1,
			dob: 1,
			location: {
				type: "Point",
				coordinates: [
					{
						$convert: {
							input: "$location.coordinates.longitude",
							to: "double",
							onError: 0.0,
							onNull: 0.0,
						},
					},
					{
						$convert: {
							input: "$location.coordinates.latitude",
							to: "double",
							onError: 0.0,
							onNull: 0.0,
						},
					},
				],
			},
		},
	},
	{
		$project: {
			firstname: 1,
			age: 1,
			location: 1,
			dob: {
				$toDate: "$dob",
			},
		},
	},
]);
```

_Operators used:_

- _$converts_ : converts to desired data types -> must contain command - input, to
- _$toDate_ : converts a valid date to ISO Date format

_Note:_

- performed chained process of _$project_ -> selection in former project method chained down to next project method
- projection 1 - extracted out required data
- projection 2 - formatting the co-ordinates
- projection 3 - formatting age & dob

_Sample output:_

```json

// ===================== after first project method =====================
{ "location" : { "coordinates" : { "latitude" : "-29.8113", "longitude" : "-31.0208" } }, "firstname" : "victor", "age" : 59, "dob" : "1959-02-19T23:56:23Z" }
{ "location" : { "coordinates" : { "latitude" : "-29.6721", "longitude" : "-154.6037" } }, "firstname" : "carl", "age" : 33, "dob" : "1984-09-30T01:20:26Z" }
{ "location" : { "coordinates" : { "latitude" : "76.4507", "longitude" : "-70.2264" } }, "firstname" : "zachary", "age" : 29, "dob" : "1988-10-17T03:45:04Z" }
{ "location" : { "coordinates" : { "latitude" : "-22.5329", "longitude" : "168.9462" } }, "firstname" : "harvey", "age" : 30, "dob" : "1988-05-27T00:14:03Z" }
{ "location" : { "coordinates" : { "latitude" : "-86.1268", "longitude" : "-54.1364" } }, "firstname" : "gideon", "age" : 47, "dob" : "1971-03-28T04:47:21Z" }
// ... many more

// ===================== after second project method =====================
{ "location" : { "type" : "Point", "coordinates" : [ -31.0208, -29.8113 ] }, "firstname" : "victor", "age" : 59, "dob" : "1959-02-19T23:56:23Z" }
{ "location" : { "type" : "Point", "coordinates" : [ -154.6037, -29.6721 ] }, "firstname" : "carl", "age" : 33, "dob" : "1984-09-30T01:20:26Z" }
{ "location" : { "type" : "Point", "coordinates" : [ -70.2264, 76.4507 ] }, "firstname" : "zachary", "age" : 29, "dob" : "1988-10-17T03:45:04Z" }
{ "location" : { "type" : "Point", "coordinates" : [ 168.9462, -22.5329 ] }, "firstname" : "harvey", "age" : 30, "dob" : "1988-05-27T00:14:03Z" }
{ "location" : { "type" : "Point", "coordinates" : [ -54.1364, -86.1268 ] }, "firstname" : "gideon", "age" : 47, "dob" : "1971-03-28T04:47:21Z" }
// ... many more

// ===================== after last project method =====================
{ "location" : { "type" : "Point", "coordinates" : [ -31.0208, -29.8113 ] }, "firstname" : "victor", "age" : 59, "dob" : ISODate("1959-02-19T23:56:23Z") }
{ "location" : { "type" : "Point", "coordinates" : [ -154.6037, -29.6721 ] }, "firstname" : "carl", "age" : 33, "dob" : ISODate("1984-09-30T01:20:26Z") }
{ "location" : { "type" : "Point", "coordinates" : [ -70.2264, 76.4507 ] }, "firstname" : "zachary", "age" : 29, "dob" : ISODate("1988-10-17T03:45:04Z") }
{ "location" : { "type" : "Point", "coordinates" : [ 168.9462, -22.5329 ] }, "firstname" : "harvey", "age" : 30, "dob" : ISODate("1988-05-27T00:14:03Z") }
{ "location" : { "type" : "Point", "coordinates" : [ -54.1364, -86.1268 ] }, "firstname" : "gideon", "age" : 47, "dob" : ISODate("1971-03-28T04:47:21Z") }
// ... many more
```

#### Scenario - 3

**_Write a query to find number of persons born in each year_**
ref: [persons.json](/json/persons.json)

```js
db.persons.aggregate([
	{
		$project: {
			_id: 0,
			birthDate: { $toDate: "$dob.date" },
		},
	},
	{
		$group: {
			_id: { birthYear: { $isoWeekYear: "$birthDate" } },
			noOfPersons: { $sum: 1 },
		},
	},
	{ $sort: { noOfPersons: -1 } },
]);
```

_Operators used:_

- _$isoWeekYear_ : extracts year from a date

_Sample output:_

```json
{ "_id" : { "birthYear" : NumberLong(1955) }, "noOfPersons" : 113 }
{ "_id" : { "birthYear" : NumberLong(1961) }, "noOfPersons" : 111 }
{ "_id" : { "birthYear" : NumberLong(1993) }, "noOfPersons" : 110 }
{ "_id" : { "birthYear" : NumberLong(1960) }, "noOfPersons" : 110 }
{ "_id" : { "birthYear" : NumberLong(1975) }, "noOfPersons" : 107 }
// ... many more
```

5. ### Unwind aggregation

**_Write a query to find hobbies based on age groups_**
ref: [friends.json](/json/friends.json)

```js
db.friends.aggregate([
	{ $unwind: "$hobbies" },
	{
		$group: {
			_id: { age: "$age" },
			allHobbies: { $push: "$hobbies" },
		},
	},
]);

db.friends.aggregate([
	{ $unwind: "$hobbies" },
	{
		$group: {
			_id: { age: "$age" },
			allHobbies: { $addToSet: "$hobbies" },
		},
	},
]);
```

_Note:_
_$unwind_ just provides output with flattening the array mentioned

_Operators used:_

- _$push_ : pushes the values, but duplication (if any) will exist
- _$addToSet_ : removes duplication from the array

_Output:_

```json
// ===================== only using unwing =====================
// the documents got repeated when there are multiple hobbies for each friend
{ "_id" : ObjectId("60aa850e452e992ccc87e359"), "name" : "Max", "hobbies" : "Sports", "age" : 29, "examScores" : [ { "difficulty" : 4, "score" : 57.9 }, { "difficulty" : 6, "score" : 62.1 }, { "difficulty" : 3, "score" : 88.5 } ] }
{ "_id" : ObjectId("60aa850e452e992ccc87e359"), "name" : "Max", "hobbies" : "Cooking", "age" : 29, "examScores" : [ { "difficulty" : 4, "score" : 57.9 }, { "difficulty" : 6, "score" : 62.1 }, { "difficulty" : 3, "score" : 88.5 } ] }
{ "_id" : ObjectId("60aa850e452e992ccc87e35a"), "name" : "Manu", "hobbies" : "Eating", "age" : 30, "examScores" : [ { "difficulty" : 7, "score" : 52.1 }, { "difficulty" : 2, "score" : 74.3 }, { "difficulty" : 5, "score" : 53.1 } ] }
{ "_id" : ObjectId("60aa850e452e992ccc87e35a"), "name" : "Manu", "hobbies" : "Data Analytics", "age" : 30, "examScores" : [ { "difficulty" : 7, "score" : 52.1 }, { "difficulty" : 2, "score" : 74.3 }, { "difficulty" : 5, "score" : 53.1 } ] }
{ "_id" : ObjectId("60aa850e452e992ccc87e35b"), "name" : "Maria", "hobbies" : "Cooking", "age" : 29, "examScores" : [ { "difficulty" : 3, "score" : 75.1 }, { "difficulty" : 8, "score" : 44.2 }, { "difficulty" : 6, "score" : 61.5 } ] }
{ "_id" : ObjectId("60aa850e452e992ccc87e35b"), "name" : "Maria", "hobbies" : "Skiing", "age" : 29, "examScores" : [ { "difficulty" : 3, "score" : 75.1 }, { "difficulty" : 8, "score" : 44.2 }, { "difficulty" : 6, "score" : 61.5 } ] }

// ===================== only using group =====================
// array of each hobbies got pushed into allHobbies, w/o array flattening
{ "_id" : { "age" : 30 }, "allHobbies" : [ [ "Eating", "Data Analytics" ] ] }
{ "_id" : { "age" : 29 }, "allHobbies" : [ [ "Sports", "Cooking" ], [ "Cooking", "Skiing" ] ] }

// ===================== combining unwing and group =====================
// in the 1st query, allHobbies array now falttened but contains duplicate values
{ "_id" : { "age" : 30 }, "allHobbies" : [ "Eating", "Data Analytics" ] }
{ "_id" : { "age" : 29 }, "allHobbies" : [ "Sports", "Cooking", "Cooking", "Skiing" ] }

// ===================== combining unwing and group =====================
// in the 2nd query, the duplication is eliminated by replacing $push with $addToSet
{ "_id" : { "age" : 30 }, "allHobbies" : [ "Eating", "Data Analytics" ] }
{ "_id" : { "age" : 29 }, "allHobbies" : [ "Skiing", "Sports", "Cooking" ] }
```

**[👆 Top](#table-contents)**

---

6. ### Projection with arrays - slice operator

**_Write a query to get first element of examScores array_**
ref: [persons.json](/json/friends.json)

```js
db.friends.aggregate([
	{ $project: { _id: 0, examScore: { $slice: ["$examScores", 1] } } },
]);
```

_Note:_

- by default - _$slice_ requires 2 parameters
- parameter 1 : array
- parameter 2 : no. of elements from start

_Output:_

```json
{ "examScore" : [ { "difficulty" : 4, "score" : 57.9 } ] }
{ "examScore" : [ { "difficulty" : 7, "score" : 52.1 } ] }
{ "examScore" : [ { "difficulty" : 3, "score" : 75.1 } ] }
```

**_Write a query to get to get last 2 elements of examScores array_**
ref: [persons.json](/json/friends.json)

```js
db.friends.aggregate([
	{ $project: { _id: 0, examScore: { $slice: ["$examScores", -2] } } },
]);
```

_Output:_

```json
{ "examScore" : [ { "difficulty" : 6, "score" : 62.1 }, { "difficulty" : 3, "score" : 88.5 } ] }
{ "examScore" : [ { "difficulty" : 2, "score" : 74.3 }, { "difficulty" : 5, "score" : 53.1 } ] }
{ "examScore" : [ { "difficulty" : 8, "score" : 44.2 }, { "difficulty" : 6, "score" : 61.5 } ] }
```

**_Write a query to get to get 2nd element of examScores array_**
ref: [persons.json](/json/friends.json)

```js
db.friends.aggregate([
	{ $project: { _id: 0, examScore: { $slice: ["$examScores", 1, 1] } } },
]);
```

_Note:_
While passing 3 parameters for _$slice_,

- parameter 1 : array
- parameter 2 : index to start
- parameter 3 : no. of elements to consider after start index mentioned in 'parameter 2'

_Output:_

```json
{ "examScore" : [ { "difficulty" : 6, "score" : 62.1 } ] }
{ "examScore" : [ { "difficulty" : 2, "score" : 74.3 } ] }
{ "examScore" : [ { "difficulty" : 8, "score" : 44.2 } ] }
```

**[👆 Top](#table-contents)**

---

7. ### Projection with arrays - size operator

**_Write a query to get the number of exams taken by each friend_**
ref: [persons.json](/json/friends.json)

```js
db.friends.aggregate([
	{ $project: { _id: 0, name: 1, noOfExams: { $size: "$examScores" } } },
]);
```

_Output:_

```json
{ "name" : "Max", "noOfExams" : 3 }
{ "name" : "Manu", "noOfExams" : 3 }
{ "name" : "Maria", "noOfExams" : 3 }
```

**[👆 Top](#table-contents)**

---

8. ### Projection with arrays - filter operator

**_Write a query to get the examScores greater than 60_**
ref: [persons.json](/json/friends.json)

```js
db.friends.aggregate([
	{
		$project: {
			_id: 0,
			name: 1,
			scoreGt60: {
				$filter: {
					input: "$examScores",
					as: "sc",
					cond: { $gt: ["$$sc.score", 60] },
				},
			},
		},
	},
]);
```

_Note:_
_$filter_ operater must be provided with below parameters:

- _input_ : intakes the array to filter
- _as_ : contains variable \(locally) for each iteration of the input array - same as argument from forEach
- _cond_ : contains conditional parameters as an array of 2 elements
  - local variable _as_ to be denoted as _$$_, since _$_ denotes a field name in the collection document
  - conditional value

_Output:_

```json
{ "name" : "Max", "scoreGt60" : [ { "difficulty" : 6, "score" : 62.1 }, { "difficulty" : 3, "score" : 88.5 } ] }
{ "name" : "Manu", "scoreGt60" : [ { "difficulty" : 2, "score" : 74.3 } ] }
{ "name" : "Maria", "scoreGt60" : [ { "difficulty" : 3, "score" : 75.1 }, { "difficulty" : 6, "score" : 61.5 } ] }
```

**[👆 Top](#table-contents)**

---

9. ### Projection with arrays - using multiple operators

**_Write a query to get the maximum examScore for each friend_**
ref: [persons.json](/json/friends.json)

```js
db.friends.aggregate([
	{ $unwind: "$examScores" },
	{ $project: { name: 1, score: "$examScores.score" } },
	{
		$group: {
			_id: "$_id",
			name: { $first: "$name" },
			maxScore: { $max: "$score" },
		},
	},
	{ $sort: { maxScore: -1 } },
]);
```

_Note:_

- step 1 : _unwind_ examScores for each friend -> that makes examScores to get out from the array
- step 2 : _project_ the field required
- step 3a : _group_ based on _\_id_ field value, since there can be duplicate names
- step 3b : within _group_, take the _first_ instance of the _name_ from each group, since all the name in the group is same
- step 3c : select _max_ score from each group
- step 4 : \(optional) sorts the output based on _max_ score

_Operators used:_

- _$max_ : finds maximum value within a group of elements
- _$first_ : considers the first element encountered in the group

_Output:_

```json
{ "_id" : ObjectId("60aa850e452e992ccc87e359"), "name" : "Max", "maxScore" : 88.5 }
{ "_id" : ObjectId("60aa850e452e992ccc87e35b"), "name" : "Maria", "maxScore" : 75.1 }
{ "_id" : ObjectId("60aa850e452e992ccc87e35a"), "name" : "Manu", "maxScore" : 74.3 }
```

**[👆 Top](#table-contents)**

---

10. ### Bucket aggregation

**_Write a query to get persons based on age category boundaries - 18, 30, 50, 60, 80_**
ref: [persons.json](/json/persons.json)

```js
db.persons.aggregate([
	{
		$bucket: {
			groupBy: "$dob.age",
			boundaries: [18, 30, 50, 60, 80],
			output: {
				totalPersons: { $sum: 1 },
				averageAge: { $avg: "$dob.age" },
				// firstname: { $push: '$name.first' },
			},
		},
	},
]);
```

_Sample output:_

```json
{ "_id" : 18, "totalPersons" : 868, "averageAge" : 25.101382488479263 }
{ "_id" : 30, "totalPersons" : 1828, "averageAge" : 39.4917943107221 }
{ "_id" : 50, "totalPersons" : 976, "averageAge" : 54.533811475409834 }
{ "_id" : 60, "totalPersons" : 1328, "averageAge" : 66.55798192771084 }
// ... many more
```

_*to let MongoDB create appropiate buckets*_

```js
db.persons.aggregate([
	{
		$bucketAuto: {
			groupBy: "$dob.age",
			buckets: 5,
			output: {
				totalPersons: { $sum: 1 },
				averageAge: { $avg: "$dob.age" },
			},
		},
	},
]);
```

_Sample output:_

```json
{ "_id" : { "min" : 21, "max" : 32 }, "totalPersons" : 1042, "averageAge" : 25.99616122840691 }
{ "_id" : { "min" : 32, "max" : 43 }, "totalPersons" : 1010, "averageAge" : 36.97722772277228 }
{ "_id" : { "min" : 43, "max" : 54 }, "totalPersons" : 1033, "averageAge" : 47.98838334946757 }
{ "_id" : { "min" : 54, "max" : 65 }, "totalPersons" : 1064, "averageAge" : 58.99342105263158 }
{ "_id" : { "min" : 65, "max" : 74 }, "totalPersons" : 851, "averageAge" : 69.11515863689776 }
// ... many more
```

**[👆 Top](#table-contents)**

---

11. ### Skip and Limit aggregation

#### Scenario - 1

**_Write a query to get top 10 oldest persons in terms of dob_**
ref: [persons.json](/json/persons.json)

```js
db.persons.aggregate([
	{
		$project: {
			_id: 0,
			firstname: "$name.first",
			birthdate: { $toDate: "$dob.date" },
		},
	},
	{ $sort: { birthdate: 1 } },
	{ $limit: 10 },
]);
```

_Note:_
_$limit_ : limits the number of output records

_Output:_

```json
{ "firstname" : "victoria", "birthdate" : ISODate("1944-09-07T15:52:50Z") }
{ "firstname" : "عباس", "birthdate" : ISODate("1944-09-12T07:49:20Z") }
{ "firstname" : "erundina", "birthdate" : ISODate("1944-09-13T14:58:41Z") }
{ "firstname" : "پرهام", "birthdate" : ISODate("1944-09-16T16:03:28Z") }
{ "firstname" : "eli", "birthdate" : ISODate("1944-09-17T15:04:13Z") }
{ "firstname" : "kirk", "birthdate" : ISODate("1944-09-18T11:03:05Z") }
{ "firstname" : "alexis", "birthdate" : ISODate("1944-10-02T22:56:32Z") }
{ "firstname" : "gina", "birthdate" : ISODate("1944-10-04T07:41:31Z") }
{ "firstname" : "sebastian", "birthdate" : ISODate("1944-10-13T15:29:05Z") }
{ "firstname" : "lucy", "birthdate" : ISODate("1944-10-25T16:27:56Z") }
```

#### Scenario - 2

**_Write a query to get next 5 persons next to top 10 oldest persons in terms of dob_**
ref: [persons.json](/json/persons.json)

```js
db.persons.aggregate([
	{
		$project: {
			_id: 0,
			firstname: "$name.first",
			birthdate: { $toDate: "$dob.date" },
		},
	},
	{ $sort: { birthdate: 1 } },
	{ $skip: 10 },
	{ $limit: 5 },
]);
```

_Note:_
_$skip_ : skips the number of records

_Output:_

```json
{ "firstname" : "eva", "birthdate" : ISODate("1944-10-29T02:05:56Z") }
{ "firstname" : "elena", "birthdate" : ISODate("1944-10-31T02:56:40Z") }
{ "firstname" : "gretchen", "birthdate" : ISODate("1944-11-01T20:49:03Z") }
{ "firstname" : "joseph", "birthdate" : ISODate("1944-11-06T11:08:45Z") }
{ "firstname" : "sarah", "birthdate" : ISODate("1944-11-07T07:53:47Z") }
```

**[👆 Top](#table-contents)**
