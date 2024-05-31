# week-3-lab-10
# Práctica de optimización de Código.

## Provided Code Snippets:
**JavaScript Snippet:**
```
// Inefficient loop handling and excessive DOM manipulation
function updateList(items) {
  let list = document.getElementById("itemList");
  list.innerHTML = "";
  for (let i = 0; i < items.length; i++) {
    let listItem = document.createElement("li");
    listItem.innerHTML = items[i];
    list.appendChild(listItem);
  }
}
```
**Java Snippet:**

```
// Redundant database queries
public class ProductLoader {
    public List<Product> loadProducts() {
        List<Product> products = new ArrayList<>();
        for (int id = 1; id <= 100; id++) {
            products.add(database.getProductById(id));
        }
        return products;
    }
}
```
**C# Snippet:**

```
// Unnecessary computations in data processing
public List<int> ProcessData(List<int> data) {
    List<int> result = new List<int>();
    foreach (var d in data) {
        if (d % 2 == 0) {
            result.Add(d * 2);
        } else {
            result.Add(d * 3);
        }
    }
    return result;
}
```

## Code Analysis:
### Task 1: Analyze each code snippet to identify potential performance issues. Look for common problems such as:
* Inefficient loops that can be optimized or rewritten.
* Redundant computations that can be simplified or eliminated.
* Excessive memory usage due to poor data structure choices.
* Unoptimized database queries that slow down data retrieval.

### Task 2: Document your initial findings for each snippet, noting what you believe are the key areas for improvement.

## Implementing Optimizations:

### Task 3: Refactor the code based on your analysis. Apply optimization techniques such as:
* Streamlining loops to reduce computational overhead.
* Implementing effective caching mechanisms where appropriate.
* Reducing complexity by improving conditional logic or function calls.
* Optimizing data structures for better memory management.
* Enhancing database queries to reduce load times.

### Task 4: Ensure that each optimization is documented. For every change you make, write a brief explanation of:
* What the specific issue was.
* How you addressed it with your optimization.
* Why you chose that particular approach.

## Evaluation:
### Task 5: Compile a report summarizing the optimizations you implemented. The report should include:
* A description of the original code problems.
* Detailed explanations of the changes made.
* Discussion on the expected or observed outcomes of these changes in terms of performance improvement.


