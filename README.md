# Ex.No.6 AI-Assisted Programming and Debugging

### Date: 03.09.2026

### Register No.: 212224040293

### Aim: Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights with Multiple AI Tools.

### AI Tools Required:
- ChatGPT (OpenAI)
- Google Gemini
- GitHub Copilot

---

## Explanation

**Interesting Area Chosen: Machine Health Monitoring System (Industrial IoT)**

The persona pattern was used to prompt an AI as an experienced programmer to design a program that reads sensor values (temperature, vibration) from a machine and flags a maintenance alert if values cross a safe threshold. The same problem was implemented across **Python, C, and Java** using AI tools, then debugged, optimized, complexity-analyzed, unit tested, and compared against manual coding.

### Persona Pattern Prompt Used

> "Act as a senior embedded systems programmer with 10 years of experience in industrial IoT. Write clean, well-commented code for a Machine Health Monitoring function that takes a list of temperature and vibration readings and returns an alert status ('SAFE', 'WARNING', or 'CRITICAL') based on threshold rules. Explain your design choices."

---

## 1. Code Generation — Python

### AI-Generated Code (Initial Version — with a bug)

```python
def check_machine_health(temp_readings, vib_readings):
    alerts = []
    for i in range(len(temp_readings)):
        temp = temp_readings[i]
        vib = vib_readings[i]
        if temp > 90 or vib > 8:
            status = "CRITICAL"
        elif temp > 75 or vib > 5:
            status = "WARNING"
        else:
            status = "SAFE"
        alerts.append(status)
    return alerts

# Bug: crashes if temp_readings and vib_readings have different lengths
readings = check_machine_health([70, 80, 95], [3, 6])
print(readings)
```

### Bug Identified

Running this raises an `IndexError` because `vib_readings` has fewer elements than `temp_readings` — the AI-generated code didn't validate that both lists are the same length.

### Optimized / Fixed Version

```python
def check_machine_health(temp_readings: list[float], vib_readings: list[float]) -> list[str]:
    if len(temp_readings) != len(vib_readings):
        raise ValueError("temp_readings and vib_readings must be the same length")

    alerts = []
    for temp, vib in zip(temp_readings, vib_readings):
        if temp > 90 or vib > 8:
            status = "CRITICAL"
        elif temp > 75 or vib > 5:
            status = "WARNING"
        else:
            status = "SAFE"
        alerts.append(status)
    return alerts
```

**Optimizations made:**
- Replaced index-based loop with `zip()` — cleaner and avoids out-of-range errors.
- Added explicit length validation with a meaningful error message.
- Added type hints for readability and static-analysis support.

### Time & Space Complexity

- **Time Complexity:** O(n) — single pass through both lists of length n.
- **Space Complexity:** O(n) — output list stores one status per reading.

### AI-Generated Unit Tests (Python `unittest`)

```python
import unittest

class TestMachineHealth(unittest.TestCase):
    def test_all_safe(self):
        self.assertEqual(check_machine_health([60, 65], [2, 3]), ["SAFE", "SAFE"])

    def test_warning_threshold(self):
        self.assertEqual(check_machine_health([80], [4]), ["WARNING"])

    def test_critical_threshold(self):
        self.assertEqual(check_machine_health([95], [9]), ["CRITICAL"])

    def test_mismatched_lengths_raises_error(self):
        with self.assertRaises(ValueError):
            check_machine_health([70, 80], [3])

if __name__ == "__main__":
    unittest.main()
```

---

## 2. Code Generation — C

```c
#include <stdio.h>
#include <string.h>

void check_machine_health(float temp[], float vib[], int n, char results[][10]) {
    for (int i = 0; i < n; i++) {
        if (temp[i] > 90 || vib[i] > 8)
            strcpy(results[i], "CRITICAL");
        else if (temp[i] > 75 || vib[i] > 5)
            strcpy(results[i], "WARNING");
        else
            strcpy(results[i], "SAFE");
    }
}

int main() {
    float temp[] = {70, 80, 95};
    float vib[] = {3, 6, 9};
    int n = 3;
    char results[3][10];

    check_machine_health(temp, vib, n, results);

    for (int i = 0; i < n; i++)
        printf("Reading %d: %s\n", i + 1, results[i]);

    return 0;
}
```

**Observation:** The AI-generated C version required manual attention to fixed-size buffers (`char results[][10]`) since C lacks dynamic strings — a limitation Python's version doesn't have. This is a good example of where AI-generated code needs a human review for language-specific safety (buffer overflow risk if status strings ever exceed 9 characters).

---

## 3. Code Generation — Java

```java
public class MachineHealthMonitor {

    public static String[] checkMachineHealth(double[] tempReadings, double[] vibReadings) {
        if (tempReadings.length != vibReadings.length) {
            throw new IllegalArgumentException("Arrays must be of equal length");
        }

        String[] results = new String[tempReadings.length];
        for (int i = 0; i < tempReadings.length; i++) {
            if (tempReadings[i] > 90 || vibReadings[i] > 8) {
                results[i] = "CRITICAL";
            } else if (tempReadings[i] > 75 || vibReadings[i] > 5) {
                results[i] = "WARNING";
            } else {
                results[i] = "SAFE";
            }
        }
        return results;
    }

    public static void main(String[] args) {
        double[] temp = {70, 80, 95};
        double[] vib = {3, 6, 9};

        String[] results = checkMachineHealth(temp, vib);
        for (String r : results) {
            System.out.println(r);
        }
    }
}
```

**Observation:** The Java version generated by AI already included the length-validation check (unlike the first Python attempt), showing that output quality can vary between prompts/tools even for the same persona pattern instruction.

---

## 4. Manual Coding vs AI-Assisted Coding — Comparison

| Aspect                  | Manual Coding                          | AI-Assisted Coding                          |
| ------------------------ | --------------------------------------- | --------------------------------------------- |
| Development Speed       | Slower — written line by line           | Much faster — full function generated instantly |
| Bug Introduction        | Fewer logic bugs, more typos            | Logic gaps possible (e.g. missing validation) |
| Documentation           | Often skipped under time pressure       | AI adds comments/docstrings by default        |
| Language Switching      | Requires re-learning syntax each time   | AI adapts the same logic across languages quickly |
| Edge Case Handling      | Depends on developer experience         | Inconsistent — needs manual review             |
| Test Generation         | Manual, time-consuming                  | AI drafts test cases quickly, needs verification |
| Best Use                | Critical, safety-sensitive core logic   | Boilerplate, prototypes, multi-language ports |

---

## 5. Code Quality Analysis

| Metric              | Python (Fixed) | C            | Java          |
| -------------------- | -------------- | ------------ | ------------- |
| Readability          | High           | Medium       | High          |
| Error Handling       | Good (after fix) | Weak (buffer risk) | Good (built-in) |
| Complexity (Time)    | O(n)           | O(n)         | O(n)          |
| Complexity (Space)   | O(n)           | O(n)         | O(n)          |
| Maintainability      | High           | Medium       | High          |
| AI Review Needed?    | Yes (had a bug) | Yes (buffer safety) | Minor (naming/style only) |

**Summary:** AI tools accelerated code generation across all three languages but did not produce equally reliable output — the Python version needed a bug fix, and the C version needed a safety review, while the Java version was closest to production-ready. This confirms that AI-assisted coding is most effective when paired with human code review, especially for input validation and language-specific safety concerns.

---

## Conclusion

This experiment demonstrated how AI tools can be used with the persona pattern to generate, debug, and optimize code for a Machine Health Monitoring System across Python, C, and Java. AI-assisted coding significantly reduced development time and generated documentation, unit tests, and multi-language ports quickly. However, manual review remained essential to catch logic bugs (missing input validation), language-specific risks (buffer handling in C), and to verify complexity and edge-case coverage. The comparison confirmed that AI-assisted coding works best as a productivity accelerator alongside — not a replacement for — careful manual code review.

## Result

The corresponding Prompt is executed successfully, and the Python, C, and Java implementations were generated, debugged, optimized, and evaluated using multiple AI tools.
