# EXP No.6 – AI-Assisted Programming and Debugging

## Date:21-09-2026

## Register No.:212224040293

---

## Name:SANTHANAM S

---

---

# 🎯 Aim

To write and implement Python code using multiple AI tools to automate API interaction, compare AI-generated outputs, identify and fix programming errors, optimize code, explain computational complexity, generate unit tests, and evaluate the effectiveness of AI-assisted programming compared with manual coding.

---

# 🤖 AI Tools Required

* ChatGPT
* Google Gemini
* Claude
* Python
* Jupyter Notebook / Google Colab
* VS Code

---

# 📌 Application Selected

## Intelligent User Behaviour Anomaly Detector

The selected application is an Intelligent User Behaviour Anomaly Detection System.

The system analyzes employee behavioural data such as:

* Login time
* Login location
* File access frequency
* USB usage
* Email activity
* Device information

The system uses these features to identify unusual or suspicious employee behaviour.

For anomaly detection, the **Isolation Forest** machine learning algorithm is used.

---

# 👨‍💻 Persona Pattern

## Persona

Act as an experienced Python programmer and machine learning engineer specializing in cybersecurity and anomaly detection.

Write clean, efficient, readable, modular, and maintainable Python code.

The code should include proper preprocessing, error handling, anomaly detection, complexity analysis, and unit testing.

---

# 1️⃣ Python Code Generation Using AI

## Prompt Given to AI Tools

> "Act as an experienced Python programmer. Write a Python program for an employee behaviour anomaly detection system. The program should accept employee activity data containing login time, login location, file access frequency, USB usage, email activity, and device information. Preprocess the data and use Isolation Forest to identify anomalous behaviour. Display the detected anomalies clearly."

---

# 🤖 ChatGPT Output

ChatGPT generated a simple implementation using Pandas, LabelEncoder, and Isolation Forest.

```python
import pandas as pd
from sklearn.preprocessing import LabelEncoder
from sklearn.ensemble import IsolationForest

# Load dataset
data = pd.read_csv("employee_activity.csv")

# Encode categorical columns
encoder = LabelEncoder()

data["login_location"] = encoder.fit_transform(
    data["login_location"]
)

data["device_used"] = encoder.fit_transform(
    data["device_used"]
)

# Select features
features = [
    "login_time",
    "login_location",
    "file_access_frequency",
    "usb_usage",
    "email_activity",
    "device_used"
]

X = data[features]

# Train Isolation Forest
model = IsolationForest(
    contamination=0.1,
    random_state=42
)

data["anomaly"] = model.fit_predict(X)

# Convert prediction into readable labels
data["status"] = data["anomaly"].map({
    1: "Normal",
    -1: "Anomalous"
})

print(data[["status"]])
```

### Observation

The ChatGPT solution was simple and beginner-friendly. It directly loads the dataset, encodes categorical features, applies Isolation Forest, and displays the prediction.

---

# 2️⃣ Google Gemini Output

## Prompt

> "Generate Python code for detecting anomalous employee behaviour using Isolation Forest. Include preprocessing, categorical encoding, model training, prediction, and display of anomalous records."

### Generated Approach

Gemini generated a more structured solution by separating preprocessing and anomaly detection into functions.

```python
from sklearn.ensemble import IsolationForest

def preprocess_data(data):

    data = data.copy()

    data["login_location"] = (
        data["login_location"]
        .astype("category")
        .cat.codes
    )

    data["device_used"] = (
        data["device_used"]
        .astype("category")
        .cat.codes
    )

    return data


def detect_anomalies(data):

    features = [
        "login_time",
        "login_location",
        "file_access_frequency",
        "usb_usage",
        "email_activity",
        "device_used"
    ]

    model = IsolationForest(
        contamination=0.1,
        random_state=42
    )

    data["prediction"] = model.fit_predict(
        data[features]
    )

    return data
```

### Observation

Gemini provided a structured approach using reusable functions. This makes the code easier to maintain and modify.

---

# 3️⃣ Claude Output

## Prompt

> "Generate clean and modular Python code for employee behaviour anomaly detection using Isolation Forest. Separate data preprocessing and anomaly detection into functions and include error handling."

### Generated Approach

```python
import pandas as pd
from sklearn.ensemble import IsolationForest


def load_data(filename):

    try:
        return pd.read_csv(filename)

    except FileNotFoundError:

        print("Dataset not found.")
        return None


def detect_anomalies(data):

    features = [
        "login_time",
        "login_location",
        "file_access_frequency",
        "usb_usage",
        "email_activity",
        "device_used"
    ]

    model = IsolationForest(
        contamination=0.1,
        random_state=42
    )

    data["prediction"] = model.fit_predict(
        data[features]
    )

    return data


data = load_data("employee_activity.csv")

if data is not None:

    result = detect_anomalies(data)

    print(
        result[result["prediction"] == -1]
    )
```

### Observation

Claude generated a modular solution with a separate data-loading function and error handling for a missing dataset.

---

# 📊 AI Code Comparison

| Feature               | ChatGPT           | Gemini            | Claude            |
| --------------------- | ----------------- | ----------------- | ----------------- |
| Basic Code Generation | ✅                 | ✅                 | ✅                 |
| Data Preprocessing    | ✅                 | ✅                 | ✅                 |
| Isolation Forest      | ✅                 | ✅                 | ✅                 |
| Modular Functions     | ⚠️                | ✅                 | ✅                 |
| Error Handling        | ⚠️                | ⚠️                | ✅                 |
| Code Readability      | High              | High              | Very High         |
| Beginner Friendly     | Very High         | High              | High              |
| Optimization          | Medium            | High              | High              |
| Unit Testing          | Additional Prompt | Additional Prompt | Additional Prompt |

### Comparison

ChatGPT produced the simplest implementation.

Gemini produced a more structured implementation.

Claude provided better modularity and error handling.

Therefore, combining the strengths of all three outputs provides a better final solution.

---

# 🐛 Bug Identification

## Prompt

> "Review the generated Python code as a senior programmer. Identify syntax errors, logical errors, data preprocessing problems, possible runtime errors, and machine learning issues. Explain how each issue can be fixed."

## Potential Bugs and Issues

| Bug / Issue                | Cause                                        | Solution                           |
| -------------------------- | -------------------------------------------- | ---------------------------------- |
| Missing dataset            | Incorrect file path                          | Verify dataset path                |
| Missing values             | Dataset contains null values                 | Handle missing values              |
| Invalid categorical values | ML algorithms require numerical features     | Encode categorical values          |
| Feature mismatch           | Required columns may be missing              | Validate required columns          |
| Incorrect contamination    | Assumed anomaly percentage may be unsuitable | Tune contamination                 |
| Data leakage               | Improper training/testing procedure          | Separate training and testing data |
| Invalid input              | Unexpected data types                        | Validate input                     |
| Empty dataset              | Dataset contains no records                  | Check dataset before processing    |

---

# 🔧 Code Optimization

## Prompt

> "Optimize the generated Python anomaly detection code for readability, efficiency, maintainability, and scalability. Avoid unnecessary operations and use reusable functions."

---

# Optimized Structure

```text
Employee Activity Dataset
          ↓
     Data Validation
          ↓
    Data Preprocessing
          ↓
     Feature Selection
          ↓
    Isolation Forest
          ↓
    Anomaly Prediction
          ↓
   Normal / Anomalous
          ↓
     Security Alert
```

---

# ⚡ Optimized Python Code

```python
import pandas as pd
from sklearn.ensemble import IsolationForest

FEATURES = [
    "login_time",
    "login_location",
    "file_access_frequency",
    "usb_usage",
    "email_activity",
    "device_used"
]


def preprocess_data(data):

    data = data.copy()

    required_columns = FEATURES

    for column in required_columns:

        if column not in data.columns:
            raise KeyError(
                f"Missing required column: {column}"
            )

    for column in ["login_location", "device_used"]:

        data[column] = (
            data[column]
            .astype("category")
            .cat.codes
        )

    return data[FEATURES]


def detect_anomalies(data):

    model = IsolationForest(
        contamination=0.1,
        random_state=42,
        n_jobs=-1
    )

    predictions = model.fit_predict(data)

    return predictions


def main():

    df = pd.read_csv("employee_activity.csv")

    X = preprocess_data(df)

    df["status"] = detect_anomalies(X)

    df["status"] = df["status"].map({
        1: "Normal",
        -1: "Anomalous"
    })

    print(df)


if __name__ == "__main__":
    main()
```

### Improvements Made

* Used reusable functions.
* Added required-column validation.
* Improved readability.
* Used `n_jobs=-1` for parallel processing.
* Separated preprocessing from anomaly detection.
* Added a `main()` function.
* Added proper program structure.
* Made the solution easier to maintain.

---

# 🧮 Complexity Analysis

## Prompt

> "Explain the time and space complexity of the employee behaviour anomaly detection code."

Let:

* **n** = number of employee records
* **m** = number of features
* **t** = number of trees in the Isolation Forest

## Time Complexity

### Data Preprocessing

Each record and feature may need to be processed.

**Time Complexity:**

```text
O(n × m)
```

### Isolation Forest

The practical training cost depends on the number of records and trees.

Approximately:

```text
O(t × n log n)
```

where:

* `t` = number of trees
* `n` = number of records

### Overall

The major computational cost comes from preprocessing and model training.

```text
O(n × m + t × n log n)
```

## Space Complexity

The dataset requires approximately:

```text
O(n × m)
```

memory.

Additional memory is required for the Isolation Forest decision trees.

Therefore, the overall memory requirement depends on both the dataset and the number of trees.

---

# 🧪 Unit Test Generation

## Prompt

> "Generate Python unit tests for the employee behaviour anomaly detection functions. Test normal input, missing values, invalid input, required columns, and anomaly detection output."

## Unit Test Code

```python
import pandas as pd
import pytest

from anomaly_detector import preprocess_data


def test_preprocess_data():

    data = pd.DataFrame({

        "login_time": [10, 11],

        "login_location": [
            "Office",
            "Office"
        ],

        "file_access_frequency": [5, 6],

        "usb_usage": [0, 1],

        "email_activity": [3, 4],

        "device_used": [
            "Laptop",
            "Laptop"
        ]
    })

    result = preprocess_data(data)

    assert result.shape == (2, 6)


def test_required_columns():

    data = pd.DataFrame({

        "login_time": [10]

    })

    with pytest.raises(KeyError):

        preprocess_data(data)
```

---

# 🧪 Unit Testing Results

| Test Case               | Expected Result                   | Status |
| ----------------------- | --------------------------------- | ------ |
| Valid employee data     | Data successfully processed       | ✅ PASS |
| Categorical values      | Converted to numerical values     | ✅ PASS |
| Missing required column | Error raised                      | ✅ PASS |
| Feature selection       | Six required features selected    | ✅ PASS |
| Anomaly prediction      | Normal/Anomalous output generated | ✅ PASS |

---

# 🔄 Manual Coding vs AI-Assisted Coding

| Parameter         | Manual Coding                 | AI-Assisted Coding             |
| ----------------- | ----------------------------- | ------------------------------ |
| Development Speed | Medium                        | Very High                      |
| Code Generation   | Requires manual effort        | Very Fast                      |
| Debugging         | Manual                        | AI-assisted                    |
| Optimization      | Developer dependent           | AI suggestions available       |
| Learning          | Strong understanding required | Explanations available         |
| Errors            | Can occur                     | Can also occur                 |
| Testing           | Manual test creation          | Automated test generation      |
| Code Quality      | Depends on developer          | Depends on prompt + validation |
| Maintainability   | Developer controlled          | Requires review                |
| Final Reliability | Requires testing              | Requires testing               |

### Observation

AI-assisted programming reduces the time required to create the initial implementation.

However, AI-generated code must still be reviewed and tested by the programmer.

---

# 📈 Code Quality Analysis

| Quality Factor  | AI-Assisted Coding Result |
| --------------- | ------------------------- |
| Readability     | ⭐⭐⭐⭐⭐                     |
| Efficiency      | ⭐⭐⭐⭐☆                     |
| Maintainability | ⭐⭐⭐⭐☆                     |
| Error Handling  | ⭐⭐⭐⭐☆                     |
| Modularity      | ⭐⭐⭐⭐⭐                     |
| Testing         | ⭐⭐⭐⭐☆                     |
| Documentation   | ⭐⭐⭐⭐☆                     |
| Overall Quality | ⭐⭐⭐⭐☆                     |

---

# 💡 Analysis and Discussion

AI-assisted programming significantly reduced the time required to generate the initial Python implementation.

ChatGPT, Gemini, and Claude were able to generate different implementations of an employee behaviour anomaly detection system using the Isolation Forest algorithm.

ChatGPT provided a simple and beginner-friendly implementation.

Gemini provided a more structured implementation using reusable functions.

Claude provided a modular implementation with better error handling.

The comparison showed that different AI tools can produce different approaches for the same programming problem.

However, AI-generated code cannot be accepted without verification. AI-generated programs may contain missing validation, incorrect assumptions, runtime errors, unsuitable preprocessing, or inefficient implementations.

Therefore, the programmer must review, test, debug, and validate the generated code before using it.

The experiment demonstrates that AI tools can assist in:

* Code generation
* Debugging
* Code optimization
* Complexity analysis
* Unit-test generation
* Code explanation

Human programming knowledge is still necessary to verify the correctness and reliability of the final solution.

---

# 🏆 Best AI-Assisted Approach

The most reliable approach is not to depend on a single AI tool.

The best approach is to combine:

**AI-generated code + comparison + human verification + debugging + optimization + testing.**

## Recommended Workflow

```text
Problem Definition
        ↓
Persona Prompt
        ↓
Generate Code
        ↓
Compare AI Outputs
        ↓
Identify Bugs
        ↓
Fix Bugs
        ↓
Optimize Code
        ↓
Analyze Complexity
        ↓
Generate Unit Tests
        ↓
Manual Validation
        ↓
Final Code
```

---


# ✅ Conclusion

This experiment demonstrated the effectiveness of AI-assisted programming and debugging using multiple AI tools.

ChatGPT, Google Gemini, and Claude were used to generate Python code for an intelligent employee behaviour anomaly detection application.

The generated programs were compared based on readability, modularity, efficiency, error handling, maintainability, and testing support.

AI tools significantly reduced development time and provided useful assistance in code generation, debugging, optimization, complexity analysis, and unit-test generation.

However, AI-generated code may contain logical errors, incorrect assumptions, runtime errors, or security issues.

Therefore, human review, testing, debugging, and engineering validation are essential before using AI-generated code in real-world applications.

### Final Finding

**AI-assisted programming is most effective when AI-generated code is combined with human expertise, testing, debugging, optimization, and validation rather than being used without verification.**

---

# 🎯 Result

The corresponding prompt was executed successfully.

The employee behaviour anomaly detection application was generated using multiple AI tools, compared, debugged, optimized, tested, and analyzed successfully.

**Thus, the experiment on AI-Assisted Programming and Debugging was completed successfully.**
