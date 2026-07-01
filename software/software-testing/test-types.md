# Test Types

## Description

A practical breakdown of the major test types — unit, integration, E2E, manual, performance, and security — with guidelines on when to use each and how to balance them in a test strategy.

## Prerequisites

- [Introduction to Software Testing](intro/what-is-software-testing.md) — understanding of basic testing concepts

## Table of Contents

- [The Test Pyramid](#the-test-pyramid)
- [Unit Tests](#unit-tests)
- [Integration Tests](#integration-tests)
- [End-to-End Tests](#end-to-end-tests)
- [Manual Testing](#manual-testing)
- [Performance Tests](#performance-tests)
- [Security Tests](#security-tests)
- [Other Test Types](#other-test-types)
- [Balancing the Test Suite](#balancing-the-test-suite)
- [Common Pitfalls](#common-pitfalls)

## Content

### The Test Pyramid

The test pyramid is a visual model for thinking about test distribution. It has three layers:

- **Base — Unit tests.** Numerous, fast, cheap. Test individual functions and classes in isolation.
- **Middle — Integration tests.** Fewer, slower. Test interactions between components (API + database, service + service).
- **Top — End-to-end tests.** Fewest, slowest, most expensive. Test complete user flows through the entire system.

```
        /\
       /  \        E2E tests (few)
      /    \
     /------\
    / Integ. \     Integration tests (some)
   /----------\
  /   Unit      \  Unit tests (many)
 /----------------\
```

The pyramid guides your investment: put most of your effort into fast, reliable unit tests, fewer into integration tests, and only a handful into E2E tests.

Many teams invert this pyramid — they rely heavily on manual E2E testing and have few unit tests. This is a recipe for slow, brittle, unpredictable releases.

### Unit Tests

**What they test:** A single unit of code — typically a function, method, or class — in isolation from its dependencies.

**Characteristics:**
- Fast (milliseconds per test).
- Deterministic (same result every time, no external dependencies).
- Run on every developer's machine and in CI.
- Dependencies are replaced with test doubles (mocks, stubs, fakes).

**Example (JavaScript / Vitest):**

```javascript
// Function to test
function calculateTotal(items) {
  return items.reduce((sum, item) => sum + item.price * item.quantity, 0);
}

// Unit test
describe('calculateTotal', () => {
  it('returns 0 for an empty cart', () => {
    expect(calculateTotal([])).toBe(0);
  });

  it('calculates the total for multiple items', () => {
    const items = [
      { price: 10, quantity: 2 },
      { price: 5, quantity: 3 },
    ];
    expect(calculateTotal(items)).toBe(35);
  });

  it('handles a single item', () => {
    const items = [{ price: 7.5, quantity: 1 }];
    expect(calculateTotal(items)).toBe(7.5);
  });
});
```

**Example (Python / pytest):**

```python
def calculate_total(items):
    return sum(item['price'] * item['quantity'] for item in items)

def test_empty_cart():
    assert calculate_total([]) == 0

def test_multiple_items():
    items = [
        {'price': 10, 'quantity': 2},
        {'price': 5, 'quantity': 3},
    ]
    assert calculate_total(items) == 35
```

**What makes a good unit test:**

- Tests one behavior, not one method. A single method might have multiple behaviors (happy path, error case, edge case).
- Is fast and reliable. Slow or flaky unit tests defeat their purpose.
- Does not touch the network, filesystem, or database.
- Has a clear name describing the scenario and expected outcome.

**Example of good test names:**

```javascript
describe('validateEmail', () => {
  it('returns true for a valid email', () => { ... });
  it('returns false when the domain is missing', () => { ... });
  it('returns false when the local part is empty', () => { ... });
  it('returns false when there are consecutive dots', () => { ... });
});
```

**Isolating dependencies:**

Code that talks to external systems (databases, APIs, files) needs test doubles:

```javascript
// Mocking an API call
const userService = {
  getUser: async (id) => {
    const response = await fetch(`/api/users/${id}`);
    return response.json();
  },
};

// Unit test with mocked fetch
it('returns the user data', async () => {
  global.fetch = vi.fn().mockResolvedValue({
    json: () => Promise.resolve({ id: 1, name: 'Alice' }),
  });

  const user = await userService.getUser(1);
  expect(user.name).toBe('Alice');
});
```

### Integration Tests

**What they test:** How multiple units or components work together. Integration tests verify that the pieces connect correctly — for example, that a repository correctly reads and writes to a database, or that an API endpoint returns the right status code.

**Characteristics:**
- Slower than unit tests (milliseconds to seconds).
- Use real dependencies or lightweight alternatives (test database, test container, HTTP server stub).
- Fewer in number than unit tests.
- Catch interface mismatches, configuration issues, and data format problems.

**Example (Node.js + PostgreSQL with a test database):**

```javascript
describe('UserRepository', () => {
  it('creates and retrieves a user', async () => {
    const repo = new UserRepository(db);
    const user = await repo.create({ name: 'Bob', email: 'bob@test.com' });
    const found = await repo.findById(user.id);
    expect(found.name).toBe('Bob');
  });
});
```

**What to test at the integration level:**

- Database queries and migrations.
- API endpoint request/response cycles.
- Message queue publish/subscribe.
- File system read/write operations.
- Interaction between a service and an external cache.

**Test containers** are Docker containers managed by the test framework. They spin up a real instance of the dependency (PostgreSQL, Redis, Elasticsearch) for the test to use:

```java
// Java with TestContainers
@Container
static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:16");

@Test
void shouldSaveAndRetrieveUser() {
    // use postgres.getJdbcUrl() to connect
    User user = userRepository.save(new User("Alice"));
    assertThat(userRepository.findById(user.getId())).isPresent();
}
```

### End-to-End Tests

**What they test:** Complete user flows from the user interface down to the database and back. E2E tests simulate real user behavior — clicking buttons, filling forms, navigating pages.

**Characteristics:**
- Slow (seconds to minutes per test).
- Brittle (fragile selectors, timing issues, environment dependencies).
- Expensive to write and maintain.
- High confidence when they pass — if the E2E tests pass, the feature works.

**Example (Playwright):**

```javascript
test('user can log in and see their dashboard', async ({ page }) => {
  await page.goto('/login');
  await page.fill('[name="email"]', 'user@example.com');
  await page.fill('[name="password"]', 'correct-password');
  await page.click('button[type="submit"]');
  await expect(page.locator('h1')).toHaveText('Dashboard');
  await expect(page.locator('.welcome')).toContainText('Welcome back');
});
```

**Guidelines for E2E tests:**

- Test critical business flows — login, checkout, search, payment.
- Do not test every edge case at the E2E level. Those belong in unit tests.
- Use data attributes (`[data-testid="submit-button"]`) instead of CSS classes for selectors. Classes change with styling, data attributes change only when the test intent changes.
- Run E2E tests in CI, not on every save. They are slow, so run them on PR merges or scheduled intervals.

### Manual Testing

**What it covers:** Anything that automation cannot easily verify — visual layout, usability, accessibility, and exploratory discovery of unexpected behavior.

**Types of manual testing:**

**Exploratory testing** — the tester explores the application without a script, following hunches and investigating suspicious behavior. This is the most effective way to find the bugs that automated tests miss.

**Usability testing** — evaluating how intuitive and pleasant the interface is. A developer might think a feature is obvious; real users will prove otherwise.

**Ad-hoc testing** — random testing without planning. Less thorough than exploratory testing but better than nothing when time is limited.

**Smoke testing** — a quick check that the basic functionality works before deeper testing begins. Often done manually for the first build of a release.

**Bug verification** — confirming that a reported bug is actually fixed and that the fix did not introduce new issues.

Manual testing should be focused where it adds the most value: new features, complex workflows, visual design, and exploratory discovery. Repetitive regression checks should be automated.

### Performance Tests

**What they test:** How the system behaves under load — response times, throughput, resource usage, and stability.

**Load testing** — simulating expected traffic to verify the system meets performance requirements. Use tools like k6, Artillery, or JMeter:

```javascript
// k6 example
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  vus: 100,            // 100 virtual users
  duration: '30s',     // for 30 seconds
};

export default function () {
  const res = http.get('https://api.example.com/users');
  check(res, { 'status is 200': (r) => r.status === 200 });
  sleep(1);
}
```

**Stress testing** — pushing beyond expected limits to find the breaking point. This helps plan capacity and identify bottlenecks.

**Endurance testing** — running at moderate load for an extended period (hours or days) to catch memory leaks, connection pool exhaustion, and resource degradation.

**Spike testing** — sudden bursts of traffic to see how the system handles rapid scaling.

Performance testing should be part of the CI pipeline for critical paths, with budgets that fail the build if response times or throughput degrade beyond thresholds.

### Security Tests

**What they test:** Vulnerabilities in the application that could be exploited by attackers.

**SAST (Static Application Security Testing)** — analyzes source code for security issues without executing it. Tools: SonarQube, ESLint security plugin, CodeQL.

**DAST (Dynamic Application Security Testing)** — probes a running application for vulnerabilities, simulating attacks. Tools: OWASP ZAP, Burp Suite.

**Dependency scanning** — checks third-party libraries for known vulnerabilities (CVEs). Tools: Dependabot, Snyk, OWASP Dependency-Check.

**Penetration testing** — manual security review by an expert who tries to break in. Usually done periodically by an external firm.

**What every developer should know:**

- Never trust user input — validate and sanitize everything.
- Use parameterized queries to prevent SQL injection.
- Hash passwords with bcrypt (never MD5 or SHA1).
- Enable HTTPS everywhere.
- Set security headers (CSP, HSTS, X-Frame-Options).
- Scan dependencies for known vulnerabilities.

### Other Test Types

**Smoke tests** — quick checks that the most critical functionality works. Run after every deployment to confirm the application started correctly.

**Sanity tests** — narrow tests that verify a specific fix or feature works as expected, without running the full test suite.

**Regression tests** — the complete test suite run to ensure that new changes have not broken existing functionality.

**Acceptance tests** — tests that verify the system meets business requirements. These may overlap with E2E tests but are framed in business language.

**Snapshot tests** — capture the output of a component (UI or data structure) and compare it to a stored reference. Useful for detecting unexpected changes in UI rendering.

**Visual regression tests** — take screenshots of UI pages and compare them pixel by pixel to baseline images. Tools: Percy, Chromatic.

### Balancing the Test Suite

A well-balanced test suite follows the pyramid and adapts to the project context:

| Criterion | Unit | Integration | E2E |
|-----------|------|-------------|-----|
| Speed | <10ms | 10ms-2s | 2s-60s |
| Count | 1000s | 100s | 10s-50s |
| Confidence | Low | Medium | High |
| Maintenance | Low | Medium | High |
| Debugging ease | Easy | Medium | Hard |

**General guidelines:**

- Aim for 70% unit, 20% integration, 10% E2E as a starting point. Adjust based on your domain.
- Every bug in production should teach you what type of test to add to catch it next time.
- If a test takes more than 30 seconds to run, it should not be in the default CI pipeline. Put it in a separate nightly or on-demand suite.
- If a test fails intermittently (flaky), quarantine it immediately. A flaky test erodes trust in the entire suite.

### Common Pitfalls

**Writing tests that test implementation, not behavior.** Tests that depend on internal details break when the implementation changes, even if the behavior is correct. Test what the code does, not how.

**Over-mocking.** Replacing too many dependencies with mocks makes the test useless — you end up testing the mock, not the real code. Use real objects where practical and mock only the expensive or unpredictable dependencies.

**Ignoring test maintenance.** Tests need refactoring too. A test suite full of duplicated setup code and cryptic assertions becomes a burden. Apply the same code quality standards to tests as to production code.

**Slow test suites.** If the test suite takes hours to run, developers stop running it. Prioritize speed. Split the suite into fast (every push) and slow (nightly) buckets.

**Chasing coverage numbers.** High coverage is a byproduct of good testing, not a goal. Writing a test that asserts nothing but increases coverage is waste.

## Glossary

| Term | Definition |
|------|------------|
| Unit test | Tests a single unit of code in isolation |
| Integration test | Tests how multiple components work together |
| End-to-end test | Tests a complete user flow through the entire system |
| Mock | A test double that simulates a real dependency |
| Stub | A test double that returns predefined values |
| Fixture | A fixed set of data used as a baseline for tests |
| Coverage | The percentage of code executed by tests |
| Flaky test | A test that passes and fails without code changes |
| Regression | A bug introduced by a change in otherwise working code |
| Smoke test | A quick check that basic functionality works |
| SAST | Static application security testing |
| DAST | Dynamic application security testing |

## Quick References

- [Martin Fowler — Test Pyramid](https://martinfowler.com/bliki/TestPyramid.html) — the original article on test distribution
- [k6 Documentation](https://k6.io/docs/) — load testing tool
- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/) — comprehensive web application security testing
- [Playwright Documentation](https://playwright.dev/docs/writing-tests) — E2E testing framework

## Next Steps

- [Test Case Design](test-case-design.md) — learn to write effective test cases using systematic techniques
