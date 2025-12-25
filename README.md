# Pro Assignment 5: Comprehensive Testing Suite

## Learning Objectives

By completing this assignment, you will:
- Understand the importance of testing in software development
- Learn to write unit tests for server endpoints
- Practice integration testing for API endpoints
- Set up test coverage reporting
- Implement testing best practices
- Gain experience with testing frameworks (Jest or pytest)

---

## Prerequisites

- Completed at least Assignment 2 (E2E Hello World)
- Working server and client application
- Understanding of your chosen stack (Node.js or Python)
- Cursor AI installed and configured
- Basic understanding of testing concepts

---

## Overview

This is a **pro-level bonus assignment** worth **10 bonus points**. It's optional but highly recommended for students who want to learn professional development practices.

**Goal:** Add comprehensive testing to your application to ensure reliability and catch bugs early.

---

## Instructions

### Step 1: Choose Your Testing Framework

Select a testing framework based on your stack:

**For Node.js:**
- **Jest** (recommended) - Popular, well-documented, built-in coverage
- **Mocha + Chai** - Flexible, modular
- **Vitest** - Fast, modern alternative

**For Python:**
- **pytest** (recommended) - Popular, powerful, easy to use
- **unittest** - Built-in, no installation needed
- **nose2** - Extended unittest

### Step 2: Set Up Testing Environment

1. **Install testing framework:**

   **Node.js (Jest):**
   ```bash
   npm install --save-dev jest
   # Or with TypeScript support
   npm install --save-dev jest @types/jest ts-jest
   ```

   **Python (pytest):**
   ```bash
   pip install pytest pytest-cov
   ```

2. **Configure testing:**

   **Node.js (package.json):**
   ```json
   {
     "scripts": {
       "test": "jest",
       "test:watch": "jest --watch",
       "test:coverage": "jest --coverage"
     }
   }
   ```

   **Python (pytest.ini or setup.cfg):**
   ```ini
   [tool:pytest]
   testpaths = tests
   python_files = test_*.py
   addopts = --cov=. --cov-report=html --cov-report=term
   ```

3. **Create test directory:**
   - Create `tests/` or `__tests__/` directory
   - Organize test files by feature

### Step 3: Write Unit Tests

Write unit tests for your server endpoints:

**Example (Node.js with Jest):**

```javascript
// tests/server.test.js
const request = require('supertest');
const app = require('../server/index');

describe('GET /api/hello', () => {
  test('should return hello message', async () => {
    const response = await request(app)
      .get('/api/hello')
      .expect(200);

    expect(response.body).toHaveProperty('message');
    expect(response.body.message).toBe('Hello Vibe!');
  });

  test('should return JSON format', async () => {
    const response = await request(app)
      .get('/api/hello')
      .expect('Content-Type', /json/);
  });
});
```

**Example (Python with pytest):**

```python
# tests/test_server.py
import pytest
from server.app import app

@pytest.fixture
def client():
    app.config['TESTING'] = True
    with app.test_client() as client:
        yield client

def test_hello_endpoint(client):
    response = client.get('/api/hello')
    assert response.status_code == 200
    data = response.get_json()
    assert 'message' in data
    assert data['message'] == 'Hello Vibe!'
```

### Step 4: Write Integration Tests

Write integration tests that test the full API flow:

**Example (Node.js):**

```javascript
// tests/integration.test.js
const request = require('supertest');
const app = require('../server/index');

describe('API Integration Tests', () => {
  test('full request flow works', async () => {
    const response = await request(app)
      .get('/api/hello')
      .expect(200);

    expect(response.body).toEqual({ message: 'Hello Vibe!' });
  });
});
```

### Step 5: Set Up Test Coverage

Configure test coverage reporting:

**Node.js (Jest):**
```json
{
  "jest": {
    "collectCoverageFrom": [
      "server/**/*.js",
      "!server/**/*.test.js"
    ],
    "coverageThreshold": {
      "global": {
        "branches": 80,
        "functions": 80,
        "lines": 80,
        "statements": 80
      }
    }
  }
}
```

**Python (pytest):**
```bash
pytest --cov=server --cov-report=html --cov-report=term
```

### Step 6: Run Tests and Generate Coverage

1. **Run tests:**
   ```bash
   # Node.js
   npm test
   npm run test:coverage

   # Python
   pytest
   pytest --cov
   ```

2. **Check coverage:**
   - Aim for **> 80% coverage**
   - Review coverage report
   - Identify untested code

3. **Fix failing tests:**
   - Make sure all tests pass
   - Fix any bugs found by tests

### Step 7: Document Your Testing

1. **Update README.md:**
   - Add "Testing" section
   - Document how to run tests
   - Explain test structure
   - Include coverage information

2. **Document test files:**
   - Add comments explaining test purpose
   - Document test setup and teardown

---

## Requirements

### Required Tests

- [ ] **Unit tests** for all server endpoints
- [ ] **Integration tests** for API flows
- [ ] **Test coverage > 80%** (aim for 80%+)
- [ ] All tests pass
- [ ] Test files organized in `tests/` directory

### Optional Tests

- [ ] Client-side tests (if applicable)
- [ ] Error handling tests
- [ ] Edge case tests
- [ ] CI/CD integration (GitHub Actions)

---

## Acceptance Criteria

Your submission will be evaluated based on:

- [ ] Testing framework installed and configured
- [ ] Unit tests written for server endpoints
- [ ] Integration tests written
- [ ] Test coverage > 80%
- [ ] All tests pass
- [ ] Test coverage report generated
- [ ] Testing documented in README.md
- [ ] Test files organized and well-structured
- [ ] Changes committed and pushed to GitHub

---

## Submission Requirements

1. **Test files:** All tests in `tests/` directory
2. **Coverage report:** Generated and included (or link to coverage)
3. **Documentation:** README.md updated with testing section
4. **Commit:** All changes committed and pushed to GitHub

**Commit message example:**
```bash
git commit -m "Pro Assignment 5: Comprehensive Testing Suite"
```

---

## Grading Rubric

See [Grading Rubrics](../materials/grading-rubrics.md) for detailed criteria.

**Total Points:** 10 bonus points

- **Test suite completeness:** 4 points
  - Unit tests: 2 points
  - Integration tests: 2 points
- **Test coverage:** 3 points
  - > 80% coverage: 3 points
  - 60-80% coverage: 2 points
  - < 60% coverage: 1 point
- **Code quality:** 2 points
  - Tests well-organized
  - Tests are readable and maintainable
- **Documentation:** 1 point
  - Testing documented in README.md
  - Test structure explained

---

## Tips for Success

- **Start simple:** Test one endpoint first, then expand
- **Use Cursor AI:** Generate test code, then understand and modify
- **Test edge cases:** Test error conditions, empty inputs, etc.
- **Keep tests focused:** One test should test one thing
- **Use descriptive test names:** Test names should explain what they test
- **Run tests frequently:** Run tests after each change
- **Aim for 80%+ coverage:** But don't obsess over 100%

---

## Example Test Structure

```
your-project/
├── server/
│   └── index.js (or app.py)
├── client/
│   └── index.html
├── tests/
│   ├── server.test.js (or test_server.py)
│   ├── integration.test.js (or test_integration.py)
│   └── helpers.js (or test_helpers.py)
├── coverage/ (generated)
├── package.json (or requirements.txt)
└── README.md
```

---

## Getting Help

- Ask questions in the help channel
- Review testing framework documentation:
  - [Jest Documentation](https://jestjs.io/docs/getting-started)
  - [pytest Documentation](https://docs.pytest.org/)
- Check [FAQ](../materials/faq.md)
- Review [Student Guide](../materials/student-guide.md)
- Contact your teacher if needed

---

## Resources

**Testing Frameworks:**
- [Jest Documentation](https://jestjs.io/)
- [pytest Documentation](https://docs.pytest.org/)
- [Testing Best Practices](https://github.com/goldbergyoni/javascript-testing-best-practices)

**Test Coverage:**
- [Understanding Code Coverage](https://www.atlassian.com/continuous-delivery/software-testing/code-coverage)
- [Coverage Tools](https://coverage.readthedocs.io/)

---

## Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-12-25 | RepodIn Education Team | Initial version |

---

**Next Review Date:** 2026-03-20

