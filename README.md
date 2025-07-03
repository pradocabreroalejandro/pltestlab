# PLTestLab 🚀

> **Enterprise Testing Framework for Oracle PL/SQL**  
> JSON-native • Observable • CI/CD Ready

[![Oracle](https://img.shields.io/badge/Oracle-12.2%2B-red?style=flat-square&logo=oracle)](https://www.oracle.com)
[![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-green?style=flat-square)](releases)

PLTestLab is a modern testing framework for Oracle PL/SQL that brings **enterprise-grade testing** to the Oracle ecosystem. Built with JSON-native types, distributed tracing, and semantic result comparison.

---

## ✨ Why PLTestLab?

**Traditional Oracle testing is broken.** You're stuck with utPLSQL's limitations, manual validation scripts, or expensive enterprise tools that don't integrate with modern CI/CD.

PLTestLab **changes the game**:

- 🧪 **Structured test definition** with JSON data and semantic comparison
- 📊 **Native observability** with OpenTelemetry integration  
- ⚡ **Volatile data approach** - no test pollution, ever
- 🔄 **CI/CD native** with proper exit codes and JSON output
- 🎯 **Enterprise ready** - parallel execution, artifact collection, timeout handling

---

## 🚀 Quick Start

### Installation

```bash
# Download and install
curl -fsSL https://raw.githubusercontent.com/yourorg/pltestlab/main/install.sh | bash

# Or manually
git clone https://github.com/yourorg/pltestlab.git
cd pltestlab
./install.sh
```

### Setup Database

```sql
-- Connect as SYSDBA and create PLTestLab schema
@install/create_schema.sql

-- Install framework (as pltestlab user)
@install/install_framework.sql
```

### Configure Connection

```bash
plt-run config set db.host "your-oracle-db.com"
plt-run config set db.service "XE"
plt-run config set db.user "pltestlab"
plt-run config set db.password "your-password"
```

### Your First Test

```sql
-- Create a simple test
DECLARE
    l_test_id NUMBER;
BEGIN
    -- Create test
    l_test_id := plt_framework.create_test(
        p_test_name => 'customer_validation_test',
        p_description => 'Validate customer creation logic'
    );
    
    -- Add test data
    plt_framework.add_test_data(
        p_test_id => l_test_id,
        p_table_name => 'CUSTOMERS',
        p_data_json => JSON('[
            {"customer_id": 1001, "name": "John Doe", "status": "ACTIVE"},
            {"customer_id": 1002, "name": "Jane Smith", "status": "PENDING"}
        ]')
    );
    
    -- Add execution step
    plt_framework.add_test_step(
        p_test_id => l_test_id,
        p_step_order => 1,
        p_step_type => 'EXECUTE',
        p_sql_statement => 'SELECT COUNT(*) as count FROM customers WHERE status = ''ACTIVE''',
        p_expected_result => JSON('{"count": 1}'),
        p_expected_result_type => 'JSON'
    );
END;
/
```

### Run Your Test

```bash
# Execute with full tracing
plt-run run --test-name "customer_validation_test" --trace --json

# Run in CI/CD pipeline
plt-run run --tag "UNIT" --env "UAT" --parallel 4 --artifacts
```

---

## 🏗️ Architecture

PLTestLab consists of three layers:

### 1. **Database Framework** (PL/SQL)
- JSON-native data types (Oracle 12.2+ required)
- Structured test definition with steps and expectations
- Semantic result comparison with tolerance handling
- Automatic artifact collection and tracing

### 2. **CLI Interface** (Bash)
- Enterprise-grade command-line interface
- Configuration management and environment handling
- CI/CD integration with proper exit codes
- Parallel execution and artifact export

### 3. **Observability** (OpenTelemetry)
- Distributed tracing with PLTelemetry integration
- Automatic span creation for test runs and steps
- Metrics collection for duration and success rates
- Grafana/Tempo integration out-of-the-box

---

## 📋 Features

### 🧪 **Test Management**
- **Hierarchical test organization** with tags and environments
- **Step-based execution** (SETUP_DATA → EXECUTE → VERIFY → CLEANUP)
- **Volatile data approach** - each test creates and destroys its own data
- **Version control** for test definitions and expectations

### 🔍 **Advanced Comparison**
- **Semantic JSON comparison** with field exclusion (timestamps, audit fields)
- **Numeric tolerance** for floating-point comparisons
- **Rowcount validation** for data transformation tests
- **Custom SQL assertions** for complex business logic

### 📊 **Enterprise Observability**
- **Automatic span creation** for distributed tracing
- **Artifact collection** - diffs, logs, query plans, intermediate data
- **Performance metrics** with millisecond precision
- **Error correlation** with full stack traces

### 🔄 **CI/CD Integration**
- **Parallel execution** for faster feedback
- **Environment-specific configuration** (DEV/UAT/PROD)
- **JSON output** for pipeline integration
- **Proper exit codes** for build success/failure

---

## 🎯 Use Cases

### **Unit Testing**
```sql
-- Test individual PL/SQL procedures with controlled data
l_test_id := plt_framework.create_test(
    p_test_name => 'calculate_discount_test',
    p_tags => 'UNIT,PRICING'
);
```

### **Integration Testing**
```sql
-- Test complete business workflows
l_test_id := plt_framework.create_test(
    p_test_name => 'order_fulfillment_integration',
    p_tags => 'INTEGRATION,ORDER_MGMT'
);
```

### **Data Quality Testing**
```sql
-- Validate data transformations and migrations
plt_framework.add_expectation(
    p_test_id => l_test_id,
    p_expectation_type => 'ROWCOUNT',
    p_expected_value => JSON('{"min": 1000, "max": 1500}')
);
```

### **Performance Testing**
```sql
-- Monitor query performance with tolerance
plt_framework.add_expectation(
    p_test_id => l_test_id,
    p_expectation_type => 'EXECUTION_TIME',
    p_expected_value => JSON('{"max_ms": 5000}'),
    p_tolerance_pct => 10
);
```

---

## 🛠️ CLI Reference

### **Basic Commands**
```bash
plt-run run --test-id 123                    # Execute specific test
plt-run run --tag UNIT --parallel 4          # Run tests in parallel
plt-run run --test-name "my_test" --dry-run  # Validate without executing
plt-run list                                 # Show available tests
plt-run config show                          # Display configuration
```

### **Advanced Options**
```bash
plt-run run \
  --tag INTEGRATION \
  --env UAT \
  --trace \
  --compare \
  --artifacts \
  --timeout 300 \
  --json
```

### **CI/CD Integration**
```yaml
# GitLab CI example
test_database:
  script:
    - plt-run run --tag UNIT --env CI --json --artifacts
  artifacts:
    reports:
      junit: artifacts/test-results.xml
    paths:
      - artifacts/
```

---

## 📊 Sample Output

### **CLI Output**
```bash
$ plt-run run --test-name "order_processing" --trace

╔═══════════════════════════════════════════════════════════════╗
║                      PLTestLab CLI v1.0.0                    ║
║           Enterprise Testing Framework for Oracle             ║
║                                                               ║
║  🚀 JSON-native  📊 Observable  🔄 CI/CD Ready               ║
╚═══════════════════════════════════════════════════════════════╝

[INFO] Starting PLTestLab execution...
[INFO] Environment: UAT
[INFO] Tracing: ENABLED
[INFO] Executing test(s)...
[SUCCESS] ✓ Test PASSED
[SUCCESS] Test execution completed in 2.3s
[INFO] Artifacts saved to /tmp/pltestlab_12345/artifacts.json
```

### **JSON Output**
```json
{
  "run_id": 1001,
  "test_id": 123,
  "status": "PASSED",
  "duration_ms": 2340,
  "environment": "UAT",
  "steps": [
    {
      "step_id": 1,
      "step_type": "SETUP_DATA",
      "status": "PASSED",
      "duration_ms": 120
    },
    {
      "step_id": 2,
      "step_type": "EXECUTE",
      "status": "PASSED",
      "duration_ms": 890,
      "actual_result": {"order_count": 5, "total_amount": 1250.00}
    }
  ],
  "artifacts": [
    {
      "type": "COMPARISON",
      "name": "result_comparison",
      "data": {"match": true, "comparison_type": "JSON"}
    }
  ]
}
```

---

## 🔧 Configuration

PLTestLab uses a configuration file at `~/.pltestlab/config`:

```bash
# Database connection
PLT_DB_HOST="oracle-prod.company.com"
PLT_DB_PORT="1521"
PLT_DB_SERVICE="PROD"
PLT_DB_USER="pltestlab"
PLT_DB_PASSWORD="secure_password"

# Observability
PLT_TELEMETRY_ENDPOINT="http://jaeger:14268"
PLT_TELEMETRY_ENABLED="true"

# Defaults
PLT_DEFAULT_ENV="DEV"
```

---

## 📈 Observability & Monitoring

### **Grafana Dashboard**
PLTestLab automatically sends metrics to your observability stack:

- **Test execution rates** and success percentages
- **Performance trends** with P95/P99 latencies  
- **Error correlation** with full trace context
- **Environment comparison** (DEV vs UAT vs PROD)

### **Distributed Tracing**
Each test execution creates a trace with:
- Parent span for the entire test
- Child spans for each step (SETUP → EXECUTE → VERIFY → CLEANUP)
- Automatic error propagation and correlation
- Custom attributes for test metadata

---

## 🏆 Why PLTestLab vs Alternatives?

| Feature | PLTestLab | utPLSQL | Commercial Tools |
|---------|-----------|---------|------------------|
| **JSON Native** | ✅ Oracle 12.2+ | ❌ String-based | ⚠️ Limited |
| **Distributed Tracing** | ✅ OpenTelemetry | ❌ None | ⚠️ Proprietary |
| **CI/CD Integration** | ✅ CLI + JSON | ⚠️ Limited | ⚠️ Complex |
| **Semantic Comparison** | ✅ Built-in | ❌ Manual | ⚠️ Basic |
| **Volatile Data** | ✅ Automatic | ❌ Manual cleanup | ⚠️ Varies |
| **Enterprise Features** | ✅ Parallel, artifacts | ❌ Basic | ✅ Licensed |
| **Learning Curve** | 🟢 Moderate | 🟡 Steep | 🔴 High |
| **Cost** | 🟢 Free | 🟢 Free | 🔴 $$$$ |

---

## 🛣️ Roadmap

### **v1.1 - Advanced Comparisons**
- [ ] JSONPath filtering for complex object comparison
- [ ] TABLE/ROWSET comparison for multi-row results
- [ ] Custom comparison functions

### **v1.2 - Enhanced Observability**
- [ ] Prometheus metrics export
- [ ] Slack/Teams notifications
- [ ] HTML test reports

### **v1.3 - Enterprise Features**
- [ ] Test templates and inheritance
- [ ] Approval workflows for production tests
- [ ] Advanced scheduling and triggers

---

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### **Development Setup**
```bash
git clone https://github.com/yourorg/pltestlab.git
cd pltestlab
./scripts/dev-setup.sh
```

### **Running Tests**
```bash
# Test the framework itself
plt-run run --tag PLT_FRAMEWORK --env DEV

# Run integration tests
./scripts/integration-tests.sh
```

---

## 📜 License

PLTestLab is released under the [MIT License](LICENSE).

---

## 📞 Support

- 📖 **Documentation**: [docs.pltestlab.io](https://docs.pltestlab.io)
- 💬 **Community**: [Slack Channel](https://pltestlab.slack.com)
- 🐛 **Issues**: [GitHub Issues](https://github.com/yourorg/pltestlab/issues)
- 📧 **Enterprise Support**: support@pltestlab.io

---

<p align="center">
  <strong>Built with ❤️ for the Oracle community</strong><br>
  <em>Because enterprise testing shouldn't suck</em>
</p>
