- [[Api]]

## Overview
- Code quality analysis platform
- Static code analysis (SAST)
- Continuous code inspection
- Quality gates and metrics
- Multi-language support

## Key Features

### Code Quality Metrics
- **Code Smells** - maintainability issues
- **Bugs** - potential runtime errors
- **Vulnerabilities** - security issues
- **Coverage** - test coverage percentage
- **Duplications** - code duplication detection
- **Technical Debt** - effort to fix issues

### Quality Gates
- Define quality criteria
- Pass/fail based on metrics
- Block merges if quality gate fails
- Customizable thresholds

### Rules and Profiles
- Language-specific rules
- Custom rule sets
- Rule severity levels
- Rule activation/deactivation

## Supported Languages
- Java, C#, JavaScript, TypeScript
- Python, PHP, Ruby, Go
- Kotlin, Scala, Swift
- HTML, CSS, XML, SQL
- And many more...

## Integration

### CI/CD Integration
```yaml
# GitHub Actions example
- name: SonarQube Scan
  uses: sonarsource/sonarqube-scan-action@master
  env:
    SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
    SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
```

### Maven
```xml
<plugin>
    <groupId>org.sonarsource.scanner.maven</groupId>
    <artifactId>sonar-maven-plugin</artifactId>
    <version>3.9.1.2184</version>
</plugin>
```

```bash
mvn sonar:sonar -Dsonar.login=token
```

### Gradle
```gradle
plugins {
    id "org.sonarqube" version "3.4.0"
}

sonarqube {
    properties {
        property "sonar.login", "token"
    }
}
```

## Common Issues Detected

### Code Smells
- Long methods
- Too many parameters
- Duplicated code
- Complex conditions
- Dead code

### Bugs
- Null pointer exceptions
- Resource leaks
- Logic errors
- Infinite loops
- Unreachable code

### Vulnerabilities
- SQL injection
- XSS vulnerabilities
- Hardcoded passwords
- Weak cryptography
- Insecure deserialization

## Example Report
```
Files: 150
Lines: 15,000
Issues: 45
  - Bugs: 5
  - Vulnerabilities: 2
  - Code Smells: 38
Coverage: 75%
Duplications: 3.2%
Technical Debt: 2 days
```

## Best Practices
- Run analysis on every commit
- Set up quality gates
- Fix critical issues first
- Track technical debt
- Use SonarLint in IDE
- Review and tune rules
- Monitor trends over time

## SonarLint (IDE Plugin)
- Real-time analysis in IDE
- Before-commit feedback
- Same rules as SonarQube
- Available for IntelliJ, Eclipse, VS Code

## Alternatives
- **Checkstyle** - Java code style
- **ESLint** - JavaScript linting
- **Pylint** - Python linting
- **PMD** - multi-language static analysis
- **SpotBugs** - Java bug finder

