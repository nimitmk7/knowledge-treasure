Python fixtures are reusable components in pytest that provide a fixed, reliable baseline for tests to run consistently. 

They serve as the “arrange” phase of testing by setting up preconditions, test data, or environments that tests need to execute properly

In Python — especially in testing frameworks like **pytest** — a **fixture** is a reusable piece of code that sets up some context **before a test runs**, and optionally cleans up afterward.

---

### **🧪 Simple definition:**

> A **fixture** provides **test setup** — like creating objects, opening files, mocking data, etc. — and makes it available to test functions automatically.

---

### **✅ Example: Basic pytest fixture**

```Python
import pytest

@pytest.fixture
def sample_data():
    return {"username": "nimit", "role": "admin"}

def test_user_role(sample_data):
    assert sample_data["role"] == "admin"
```

Here:

- sample_data() is a **fixture**.
- pytest automatically **injects** it into the test.
- You don’t call it manually — pytest does that for you.

---

### **🛠️ Common Use Cases**

|**Use case**|**Example**|
|---|---|
|Setup DB connections|@pytest.fixture def db_conn(): ...|
|Create test users|@pytest.fixture def test_user(): ...|
|Mock APIs or services|@pytest.fixture def mock_api(): ...|
|Temp file or dir|@pytest.fixture def tmp_dir(): ...|


---

### **🔁 Fixture with setup/teardown using** 

### **yield**

```Python
@pytest.fixture
def resource():
    print("SETUP")
    yield "some resource"
    print("TEARDOWN")

def test_something(resource):
    assert resource == "some resource"
```

This prints:

```
SETUP
TEARDOWN
```

Fixture runs **before and after** the test.

---

### **🧠 Why use fixtures?**

- Avoid repetitive setup code.
    
- Share setup across many tests.
    
- Increase test readability and maintainability.
---

### When do fixtures run ?

**Fixtures in pytest are very configurable**, and **they don’t always have to run both before and after a test**. It depends on **how you define them**.

---

#### **🔄 1.** **Default Behavior (No Teardown)**
If you define a fixture like this:

```Python
@pytest.fixture
def simple():
    print("SETUP")
    return "data"
```

It only runs **before** the test. No teardown logic will run afterward.

---

#### **🔁 2.** **With yield (Setup + Teardown)**

If you use yield, pytest will treat everything **before the yield as setup**, and everything **after the yield as teardown**.

```Python
@pytest.fixture
def resource():
    print("SETUP")
    yield "resource"
    print("TEARDOWN")
```

Now:

- Runs **before** the test (SETUP)
- Runs **after** the test (TEARDOWN)

---

#### **🔄 3.** **Configurable Scope**

Fixtures don’t have to run **before/after every test**. You can change how often they’re set up/teared down using scope.

```Python
@pytest.fixture(scope="module")
def db_connection():
    print("Connecting to DB")
    yield "db"
    print("Disconnecting from DB")
```

Available scopes:

|**Scope**|**Setup/Teardown runs…**|
|---|---|
|function _(default)_|Once per test function|
|class|Once per test class|
|module|Once per test module (file)|
|package|Once per package (pytest ≥ 7.0)|
|session|Once per test session (entire test run)|

  

---

#### **🧠 Summary**

| **Behavior**     | **Controlled by** | **Example**                     |
| ---------------- | ----------------- | ------------------------------- |
| Only setup       | No yield          | @pytest.fixture                 |
| Setup + teardown | yield in fixture  | @pytest.fixture + yield         |
| Frequency        | scope parameter   | @pytest.fixture(scope="module") |

Let me know if you want to see a teardown that e.g. closes a file, cleans DB records, or clears environment variables!
