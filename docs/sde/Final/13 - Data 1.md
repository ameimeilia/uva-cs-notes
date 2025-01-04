---
layout: default
title: Data 1
parent: Final Notes
nav_order: 3
---
# Data Persistence
- **Persistence** refers to saving data in a program so that it is retained between runs.
- Without persistence, any data or history from a program is lost when the program closes.
- Without data persistence, all history is lost when the game or terminal is closed.
- To prevent this, data can be stored using:
    - **Files**
    - **Databases**

## Files
- File saving allows reopening data even after closing the program or restarting the computer.
- Common file types for data persistence include:
    - Source code files (.java, .py, .c)
    - Document files (.doc, .pdf)
    - Media files (.mp3, .png)
    - Executable files (.exe, .jar)

## Databases
- Databases store data using files but abstract away the details of how data is managed.
- Offer significant performance optimizations compared to manually handling file storage.
- Two types of databases:
    - **Local**: Uses files on the local machine (e.g., SQLite).
    - **Remote**: Accessed via a server (e.g., PostgreSQL, MySQL).
- SQL (Structured Query Language) is commonly used to manage data in databases.
- Tools:
    - **JDBC**: Direct database interaction.
    - **Hibernate**: ORM tool to abstract SQL, simplifying database interactions.

# JSON (JavaScript Object Notation)
- JSON is a **human-readable** and **writable** data format.
- Commonly used for data transmission between systems or storing structured data.

## Benefits of JSON
- **Structured yet flexible**: Unlike CSV, JSON allows easily adding new data (e.g., “city”) without breaking existing structures.
- **Nesting**: Supports nested structures like lists and maps, which CSV does not easily handle.
- **Human-readable**: Easy to read and understand the data format.
- **Object-friendly**: JSON maps well to programming objects, making it easy to convert objects into JSON strings and vice versa for data transmission.

## Data Types Supported in JSON
- **Integers**: Whole numbers (e.g., `5`)
- **Floats**: Decimal numbers (e.g., `3.5`)
- **Strings**: Text enclosed in quotes (e.g., `"text"`)
- **Booleans**: True or false values (`true`, `false`)
- **Lists**: Ordered data in square brackets (e.g., `[1, 2, 3]`)
- **Maps**: Key-value pairs in curly braces, where keys must be strings (e.g., `{"key": "value"}`)

## Example structure

```
{
  "conferences": [
    {
      "abbreviation":  "ACC", 
      "id": 1,
      "name":  "Atlantic Coast Conference",
      "founded": 1953,
      "size": 14,
      "hasDivisions": true,
      "teams": [
        {
          "id": 1,
          "name": "University of Virginia",
          "abbreviation": "UVA",
          "mascot": "Cavaliers",
          "division": "ACC Coastal",
          "record": {
            "totalWins": 3,
            "totalLosses": 7,
            "confWins": 1,
            "confLosses": 6,
            "winPercent": 0.3
          }
        },
        {
          "id": 2,
          "name": "Virginia Tech",
          "abbreviation": "VT",
          "mascot": "Hokies",
          "division": "ACC Coastal",
          "record": {
            "totalWins": 3,
            "totalLosses": 8,
            "confWins": 1,
            "confLosses": 6,
            "winPercent": 0.272727272
          }
        }
      ]
    },
    {
      "abbreviation":  "Big XII",
      "id": 2,
      "name":  "Big 12 Conference",
      "founded": 1994,
      "size": 14,
      "hasDivisions": false,
      "teams": [
        {
          "id": 3,
          "name": "West Virginia University",
          "abbreviation": "WVU",
          "mascot": "Mountaineers",
          "record": {
            "totalWins": 5,
            "totalLosses": 7,
            "confWins": 3,
            "confLosses": 6,
            "winPercent": 0.416666667
          }
        }
      ]
    }
  ]
}
```

- The outer structure is a **map** with a key `"conferences"` that maps to a **list** of conference objects.
- Each **conference** object is a **map** with keys like `"abbreviation"`, `"id"`, `"name"`, and `"teams"` (which is itself a list of team objects).
- Each **team** object is a **map** containing details like `"id"`, `"name"`, `"abbreviation"`, `"record"`, etc.

## Stepping through object
1. Get the list of conferences: `conferenceList = myJsonObject["conferences"]`
2. Find the ACC conference: `accConference = conferenceList[0]`
3. Get the list of teams in the ACC: `accTeams = accConference["teams"]`
4. Find the team "University of Virginia": `uvaTeam = accTeams[0]`
5. Get the team’s record: `uvaRecord = uvaTeam["record"]`
6. Access specific data like total wins: `totalWins = uvaRecord["totalWins"]`

- **Key takeaway**: JSON parsing involves stepping through the structure from the outermost level to the specific data, often using maps and lists to drill down.

# Parsing JSON in Java

## Org.JSON
- Library: [`org.json`](https://mvnrepository.com/artifact/org.json/json) is used for working with JSON in Java.
- Key data types:
    - `JSONObject`: Represents a JSON object (curly braces).
    - `JSONArray`: Represents a JSON array (square brackets).
## [JSONObject](https://stleary.github.io/JSON-java/org/json/JSONObject.html)
- Represents a JSON object with key-value pairs.
- Keys must be strings.
- Values can be of various types: String, number, boolean, `JSONObject`, or `JSONArray`.

**Common methods**:
- `get(String key)`: General method to get a value (returns `Object`).
- `getInt()`, `getString()`, `getBoolean()`: Retrieve values of specific types.
- `getJSONObject()`, `getJSONArray()`: Extract JSON objects/arrays from the parent JSON.
- `hasKey()`: Check if a key exists.
- `optInt()`: Retrieve value or handle missing key with an `Optional` return.
## [JSONArray](https://stleary.github.io/JSON-java/org/json/JSONArray.html)
- Represents a JSON array of ordered items.
- Access values by index (`int index`).

**Common methods**:
- `getInt()`, `getString()`, etc.: Retrieve values from the array.
- `length()`: Get the size of the array.

## Example: Parse QUIZ JSON
- parse the following JSON string into a series of objects that will let us build a quiz from this format

```
{
  "quiz": {
    "sport": [
      {
        "question": "Which one is a correct team name in NBA?",
        "options": [
          "New York Bulls",
          "Los Angeles Kings",
          "Golden State Warriros",
          "Houston Rockets"
        ],
        "answer": "Houston Rockets"
      }
    ],
    "maths": [
      {
        "question": "5 + 7 = ?",
        "options": [
          "10",
          "11",
          "12",
          "13"
        ],
        "answer": "12"
      },
      {
        "question": "12 - 8 = ?",
        "options": [
          "1",
          "2",
          "3",
          "4"
        ],
        "answer": "4"
      }
    ]
  }
}
```

## Our classes

### `Quiz.java`

- Represents the overall quiz.
- Contains a map of categories (`QuizCategory`).
- Methods:
    - `addCategory()`: Add a new category.
    - `getNumQuestions()`: Get total number of questions.
    - `getRandomQuestion()`: Fetch a random question from a category.

### `QuizCategory.java`

- Represents a quiz category.
- Contains a list of questions (`QuizQuestion`).
- Methods:
    - `addQuestion()`: Add a question.
    - `getNumberOfQuestions()`: Get total questions.
    - `getRandomQuestion()`: Get a random question from the list.

### `QuizQuestion.java`

- Represents an individual question.
- Fields:
    - `question`: The question text.
    - `options`: List of possible answers.
    - `answer`: The correct answer.
- Methods:
    - `toString()`: Print the question, options, and answer.

## Parsing a JSON String

### Parsing QuizQuestion json
- Extract question, options (as `JSONArray`), and answer from JSON.`
- Example method:

```java
    private QuizQuestion jsonToQuizQuestion(JSONObject jsonObject) {
	    String question = jsonObject.getString("question");
	    String answer = jsonObject.getString("answer");

        JSONArray choiceArray = jsonObject.getJSONArray("options");
        List<String> options = new ArrayList<>();
        for (int i = 0; i < choiceArray.length(); i++) {
            options.add(choiceArray.getString(i));
        }

        return new QuizQuestion(answer, options, question);
    }
```

### Parsing QuizCategory
- Each category is a `JSONArray` of `QuizQuestion` objects.
- Parse each category by iterating over the array and converting it into `QuizQuestion` objects.
- Sample code for parsing categories and questions:

```java
private Quiz getQuizFromJSON(JSONObject root) {
        var quiz = new Quiz();

        var quizRoot = root.getJSONObject("quiz");
        var categorySet = quizRoot.keySet();

        for (String quizCategoryName : categorySet) {
            JSONArray questionArray = quizRoot.getJSONArray(quizCategoryName);
            QuizCategory quizCategory = getQuizCategory(quizCategoryName, questionArray);
            quiz.addCategory(quizCategory);
        }
        return quiz;
    }

    private QuizCategory getQuizCategory(String category, JSONArray questionArray) {
        QuizCategory quizCategory = new QuizCategory(category);
        for (Object questionObject : questionArray) {
            JSONObject questionJSON = (JSONObject) questionObject;
            QuizQuestion question = jsonToQuizQuestion(questionJSON);
            quizCategory.addQuestion(question);
        }
        return quizCategory;
    }
```

### Final Quiz Parser code:

```java
public class QuizParser {
    public static void main(String[] args) throws IOException{
        QuizParser parser = new QuizParser();
        JSONObject rootJSON = parser.getRootJsonObject("quiz.json");
        var quiz = parser.getQuizFromJSON(rootJSON);

        System.out.println("Quiz size: " + quiz.getNumQuestions());

        QuizQuestion quizQuestion = quiz.getRandomQuestion("math");
        System.out.println(quizQuestion);
    }

    private JSONObject getRootJsonObject(String quizFileName) throws IOException {
        var fileText = getFileContents(quizFileName);
        JSONObject root = new JSONObject(fileText);
        return root;
    }

    private String getFileContents(String filename) throws IOException {
        StringBuilder stringBuilder = new StringBuilder();
        BufferedReader bufferedReader = new BufferedReader(new FileReader(filename));
        bufferedReader.lines().forEach(stringBuilder::append);
        return stringBuilder.toString();
    }

    private Quiz getQuizFromJSON(JSONObject root) {
        var quiz = new Quiz();

        var quizRoot = root.getJSONObject("quiz");
        var categorySet = quizRoot.keySet();

        for (String quizCategoryName : categorySet) {
            JSONArray categoryJSON = quizRoot.getJSONArray(quizCategoryName);
            QuizCategory quizCategory = getQuizCategory(quizCategoryName, categoryJSON);
            quiz.addCategory(quizCategory);
        }
        return quiz;
    }

    private QuizCategory getQuizCategory(String category, JSONArray categoryJSON) {
        QuizCategory quizCategory = new QuizCategory(category);
        for (Object questionObject : categoryJSON) {
            JSONObject questionJSON = (JSONObject) questionObject;
            QuizQuestion question = jsonToQuizQuestion(questionJSON);
            quizCategory.addQuestion(question);
        }
        return quizCategory;
    }

    private QuizQuestion jsonToQuizQuestion(JSONObject jsonObject) {
       String question = jsonObject.getString("question");
       String answer = jsonObject.getString("answer");

        JSONArray choiceArray = jsonObject.getJSONArray("options");
        List<String> options = new ArrayList<>();
        for (int i = 0; i < choiceArray.length(); i++) {
            options.add(choiceArray.getString(i));
        }

        return new QuizQuestion(question, options, answer);
    }
}
```

# Building JSON Objects
- **Goal**: Convert a `Quiz` object into a `JSONObject`.

## QuizQuestion to JSONObject
- `QuizQuestion` has the following fields:

```java
public class QuizQuestion {
    private String question;
    private List<String> options;
    private String answer;

    // methods and constructors hidden
}
```

- code to write to a `QuizQuestion` to a `JSONObject`

```java
public JSONObject toJSON() {
	JSONObject questionJSON = new JSONObject();

	// store question and answer
	questionJSON.put("question", question);
	questionJSON.put("answer", answer);

	// `org.json` knows that `List<String>` should be stored as a
	// `JSONArray` of Strings
	questionJSON.put("options", options);
	
	return questionJSON;
}
```

## QuizCategory to JSONArray
- `QuizCategory` has the following fields:

```java
public class QuizCategory {
    private String categoryName;
    private List<QuizQuestion> questions;
    
    // methods and constructor hidden
}
```

- `toJSON()` converts the list of `QuizQuestion` objects to a `JSONArray` without including the category name (handled separately in `Quiz`)

```java
public JSONArray toJSON() {
    List<JSONObject> questionObjects = questions.stream()
            .map(q -> q.toJSON())
            .toList();
    return new JSONArray(questionObjects);
}
```

## Quiz to JSON Object
- `Quiz.java` field:

```java
public class Quiz {
    private Map<String,QuizCategory> quiz;
```

From there, we can get our entire Quiz as JSON using, in Quiz.java

```java
public JSONObject toJSON() {
	JSONObject quizJSON = new JSONObject();
	for (String categoryName: quiz.keySet()) {
		QuizCategory quizCategory = quiz.get(categoryName);
		quizJSON.put(categoryName, quizCategory.toJSON());
	}

	// maps entire object to the key “quiz” in some outer JSONObject
	JSONObject root = new JSONObject();
    root.put("quiz", quizJSON);

    return root;
}
```

- after running `toJSON()` on a quiz built from our starting json in the last module:

```java
// printed using .toString(int indentFactor) -> questionObject.toString(2)
{"quiz": {
  "sports": [{
    "question": "Which one is a correct team name in NBA?",
    "answer": "Houston Rockets",
    "options": [
      "New York Bulls",
      "Los Angeles Kings",
      "Golden State Warriros",
      "Houston Rockets"
    ]
  }],
  "math": [
    {
      "question": "5 + 7 = ?",
      "answer": "12",
      "options": [
        "10",
        "11",
        "12",
        "13"
      ]
    },
    {
      "question": "12 - 8 = ?",
      "answer": "4",
      "options": [
        "1",
        "2",
        "3",
        "4"
      ]
    }
  ]
}}
```
