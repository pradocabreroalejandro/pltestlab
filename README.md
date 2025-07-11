# PLTestLab + CUE 🚀

> **"Tan simple como quieras. Tan complejo como necesites. Hecho para humanos."**

Enterprise Testing Framework for Oracle PL/SQL with Interactive Observability Dashboard

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Oracle](https://img.shields.io/badge/Oracle-12.2%2B-red.svg)](https://www.oracle.com/database/)
[![CUE](https://img.shields.io/badge/CUE-0.6%2B-blue.svg)](https://cuelang.org/)
[![OpenTelemetry](https://img.shields.io/badge/OpenTelemetry-Ready-green.svg)](https://opentelemetry.io/)

PLTestLab revolutionizes Oracle PL/SQL testing by combining CUE's expressiveness with real-time interactive dashboards. From 3-line API validations to enterprise financial workflows with complete observability.

## ⚡ Quick Start (30 seconds)

```bash
# 1. Install PLTestLab
curl -fsSL https://pltestlab.io/install.sh | bash

# 2. Your first test
echo 'quick_test: {call: "my_api.validate", expect: "OK"}' > test.cue

# 3. Run with interactive dashboard
plt-run execute --test-name quick_test --dashboard
# → Browser opens automatically with live dashboard
```

**✅ Worked?** Great! You now have enterprise Oracle observability.  
**❌ Failed?** Click the red node in dashboard → complete context: traces, logs, metrics, suggestions.

## 🎯 Why PLTestLab?

Traditional Oracle testing is broken. You're stuck with utPLSQL's limitations, cryptic error messages, or expensive enterprise tools that don't integrate with modern workflows.

**PLTestLab changes the game:**

| Feature | PLTestLab | utPLSQL | Commercial Tools |
|---------|-----------|---------|------------------|
| **Interactive Dashboard** | ✅ Real-time web UI | ❌ Text logs | ⚠️ Legacy interfaces |
| **Progressive Complexity** | ✅ 3 lines → Enterprise | ❌ Complex setup required | ⚠️ Rigid templates |
| **Visual Debugging** | ✅ Click → drill-down | ❌ Stack traces | ⚠️ Limited |
| **PLTelemetry Integration** | ✅ Distributed tracing | ❌ Manual debugging | ⚠️ Proprietary |
| **Cost** | 🟢 Open Source | 🟢 Free | 🔴 $$$$ |
| **Learning Curve** | 🟢 Natural syntax | 🟡 Steep | 🔴 High |

## 🏗️ Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Test.cue      │───▶│  CUE Compiler    │───▶│  PLTestLab JSON │
│  (Definition)   │    │   + Validator    │    │   (Execution)   │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                │                        │
                                ▼                        ▼
                       ┌──────────────────┐    ┌─────────────────┐
                       │   Error Report   │    │ Interactive     │
                       │  (Pre-execution) │    │ Dashboard       │
                       └──────────────────┘    └─────────────────┘
```

### Core Principles

1. **Tan simple como quieras** - Basic test in 3 lines, zero configuration
2. **Tan complejo como necesites** - Enterprise multi-step flows with exhaustive validation  
3. **Hecho para humanos** - Interactive dashboards, clear errors, intelligent defaults

### Components

- **CUE Schema Library** - Progressive types: from shortcuts to enterprise-grade
- **CUE Compiler/Validator** - Transpiles CUE → JSON + validation + preview dashboard
- **PLTestLab Core** - Oracle PL/SQL execution engine with context resolution
- **Interactive Dashboard System** - Web dashboard with complete PLTelemetry integration
- **Real-time Observability** - OpenTelemetry traces + Prometheus metrics + automatic correlation

## 📊 Interactive Dashboard System

PLTestLab doesn't generate static diagrams - it creates a **complete interactive dashboard** with enterprise-grade real-time observability.

### Auto-launch Dashboard
```bash
# Execution with automatic web dashboard
plt-run execute --test-name financial_test --dashboard --port 8080
# → Browser opens automatically: http://localhost:8080/dashboard/run/456
# → Real-time updates via WebSocket
```

### Interactive Experience

The dashboard provides a responsive layout with multiple interactive panels:

- **📊 Main Canvas** - Interactive execution flow diagram
- **🔍 Telemetry Panel** - Drill-down with PLTelemetry correlation
- **📈 Timeline Panel** - Execution timeline with zoom and markers

#### Click Navigation
- **Click any node** → Automatic drill-down to complete step telemetry
- **Highlight data flow** → Visual path from/to selected step  
- **Auto-filter logs** → Contextual logs by step and timerange
- **Correlate traces** → Automatic distributed trace correlation

#### Real-time Visual States
```javascript
const nodeStates = {
  PENDING: {color: '#6c757d', icon: '⏳'},
  RUNNING: {color: '#007bff', icon: '🔄', animated: true, showProgress: true},
  PASSED: {color: '#28a745', icon: '✅'},
  FAILED: {color: '#dc3545', icon: '❌', showError: true},
  TIMEOUT: {color: '#ffc107', icon: '⏱️'},
  SKIPPED: {color: '#e2e3e5', icon: '⭕'}
};
```

### PLTelemetry Integration

Complete automatic correlation from multiple sources:

```javascript
// Automatically correlated data per step
const telemetryCorrelation = {
  traces: getSpansForStep(stepId),           // OpenTelemetry distributed traces
  metrics: getMetricsForTimeRange(start, end), // Prometheus metrics in step timerange  
  logs: getLogsWithTraceId(traceId),         // Logs filtered by trace_id and step
  dbQueries: getQueriesForStep(stepId),      // DB queries executed during step
  apiCalls: getApiCallsForStep(stepId),      // External API calls from step
  pltEvents: getPLTEventsForStep(stepId)     // PLTestLab internal events
};
```

### Smart Insights

Automatic intelligent problem detection:

- **⚠️ Performance bottlenecks** - "This step is 40% slower than usual. Check DB connection pool."
- **🔍 Error correlation** - "Similar error seen in run #454. Compare configurations?"
- **💾 Resource issues** - "Memory usage spike detected. Possible memory leak in PL/SQL."

## 🧪 Progressive Test Definition

### Level 0: Ultra Simple
```cue
// Maximum shortcut - direct validation in one line
quick_test: {call: "customer_api.validate", expect: "SUCCESS"}

// Slightly more - with minimal data
basic_test: {
    setup: {customers: [{id: 1001, name: "John"}]}
    call: "api.create_profile"
    expect: {status: "CREATED"}
}
```

### Level 1: Simple Tests
```cue
// Common case - API with basic setup
simple_customer_test: {
    name: "validate_customer_creation"
    
    // Automatic setup with intelligent defaults
    setup: {
        customers: [{id: 1001, name: "John Doe", status: "ACTIVE"}]
    }
    
    // Direct execution - one procedure
    execute: {
        call: "customer_api.create_customer_profile"
        params: {
            p_customer_id: 1001,
            p_profile_type: "STANDARD"
        }
    }
    
    // Simple but effective validation
    expect: {
        result: {status: "CREATED", profile_id: ">0"}
        db_state: {
            customer_profiles: {count: 1, where: "customer_id = 1001"}
        }
    }
}
```

### Level 2: Intermediate Tests
```cue
order_processing_test: {
    name: "order_fulfillment_workflow"
    tags: ["INTEGRATION", "ORDER_MGMT"]
    
    setup: {
        customers: [{id: 1001, name: "Corp A", credit_limit: 50000}]
        products: [
            {id: 2001, name: "Widget Pro", price: 100.00, stock: 1000},
            {id: 2002, name: "Widget Premium", price: 200.00, stock: 500}
        ]
        warehouses: [{id: 3001, name: "Main Warehouse", region: "EMEA"}]
    }
    
    pipeline: [
        {
            id: "create_order"
            call: "order_api.create_order"
            params: {
                customer_id: 1001,
                items: [
                    {product_id: 2001, quantity: 10},
                    {product_id: 2002, quantity: 5}
                ]
            }
            capture: ["order_id", "total_amount"]
        },
        {
            id: "allocate_inventory"
            depends_on: ["create_order"]
            call: "inventory_api.allocate_stock"
            params: {
                order_id: "@create_order.order_id",
                warehouse_id: 3001
            }
            capture: ["allocation_results"]
        },
        {
            id: "process_payment"
            depends_on: ["allocate_inventory"]
            call: "payment_api.process_payment"
            params: {
                order_id: "@create_order.order_id",
                amount: "@create_order.total_amount"
            }
        }
    ]
    
    validation: {
        results: [
            {
                source: "@process_payment"
                expected: {status: "COMPLETED", transaction_id: ">0"}
            }
        ]
        database: [
            {
                name: "order_status_updated"
                sql: "SELECT status FROM orders WHERE order_id = @create_order.order_id"
                expected: {status: "CONFIRMED"}
            },
            {
                name: "inventory_reduced"
                sql: "SELECT stock FROM products WHERE id IN (2001, 2002)"
                expected: [
                    {id: 2001, stock: 990},
                    {id: 2002, stock: 495}
                ]
            }
        ]
    }
}
```

### Level 3: Enterprise Tests
```cue
complex_financial_integration: {
    name: "multi_currency_transaction_processing"
    description: "End-to-end financial transaction with currency conversion, compliance checks, and audit trail"
    tags: ["ENTERPRISE", "FINANCIAL", "REGULATORY"]
    environment: "UAT"
    
    setup: {
        // Complex setup with relationships
        tables: {
            customers: {
                truncate: true
                data: [
                    {id: 1001, name: "Global Corp", country: "US", kyc_status: "VERIFIED"},
                    {id: 1002, name: "Euro Ltd", country: "DE", kyc_status: "VERIFIED"}
                ]
            }
            accounts: {
                data: [
                    {id: 2001, customer_id: 1001, currency: "USD", balance: 100000.00, type: "CORPORATE"},
                    {id: 2002, customer_id: 1002, currency: "EUR", balance: 75000.00, type: "CORPORATE"}
                ]
                foreign_keys: ["customer_id -> customers.id"]
            }
            exchange_rates: {
                data: [
                    {from_currency: "EUR", to_currency: "USD", rate: 1.0847, effective_date: "2025-07-11"},
                    {from_currency: "USD", to_currency: "EUR", rate: 0.9219, effective_date: "2025-07-11"}
                ]
            }
            compliance_rules: {
                data: [
                    {rule_id: "AML_001", rule_type: "AMOUNT_THRESHOLD", threshold: 10000, currency: "USD"},
                    {rule_id: "KYC_001", rule_type: "CUSTOMER_VERIFICATION", required_status: "VERIFIED"}
                ]
            }
        }
        sequences: {
            transaction_seq: {reset_to: 100000}
        }
    }
    
    execution_pipeline: [
        {
            step_id: "prepare_transaction_batch"
            type: "DATA_TRANSFORM"
            description: "Prepare complex transaction batch with business logic"
            transformation: {
                source_data: {
                    accounts: "@setup.accounts",
                    rates: "@setup.exchange_rates"
                }
                logic: """
                    -- Complex PL/SQL preparation logic
                    DECLARE
                        l_batch transaction_batch_t := transaction_batch_t();
                        l_converted_amount NUMBER;
                    BEGIN
                        FOR acc IN (SELECT * FROM accounts WHERE balance > 5000) LOOP
                            -- Currency conversion logic
                            SELECT amount_usd INTO l_converted_amount 
                            FROM TABLE(currency_api.convert_amount(acc.balance, acc.currency, 'USD'));
                            
                            l_batch.extend;
                            l_batch(l_batch.count) := transaction_rec_t(
                                transaction_id => transaction_seq.nextval,
                                from_account => acc.id,
                                amount_original => acc.balance * 0.1,
                                amount_usd => l_converted_amount * 0.1,
                                transaction_type => 'INTERNATIONAL_TRANSFER'
                            );
                        END LOOP;
                        :result_batch := l_batch;
                    END;
                """
                output_binding: "transaction_batch"
            }
            error_handling: {
                on_failure: "ABORT"
                timeout_seconds: 120
            }
        },
        {
            step_id: "compliance_validation"
            type: "API_CALL"
            depends_on: ["prepare_transaction_batch"]
            description: "Validate transactions against compliance rules"
            api_call: {
                package: "compliance_api"
                procedure: "validate_transaction_batch"
                parameters: {
                    p_transactions: {
                        type: "TABLE"
                        element_type: "transaction_rec_t"
                        data: "@prepare_transaction_batch.transaction_batch"
                    }
                    p_rules: {
                        type: "JSON"
                        content: {
                            aml_checks: true,
                            kyc_verification: true,
                            sanctions_screening: true,
                            country_restrictions: ["OFAC", "EU_SANCTIONS"]
                        }
                    }
                }
                output_capture: {
                    validation_summary: "RETURN"
                    flagged_transactions: "OUT"
                    compliance_score: "JSON_EXTRACT($.overall_score)"
                }
            }
            error_handling: {
                on_failure: "CONTINUE"
                max_retries: 2
                timeout_seconds: 300
            }
        },
        {
            step_id: "risk_assessment"
            type: "API_CALL"
            depends_on: ["compliance_validation"]
            description: "Perform risk assessment on validated transactions"
            api_call: {
                package: "risk_engine"
                procedure: "assess_transaction_risk"
                parameters: {
                    p_transactions: "@prepare_transaction_batch.transaction_batch"
                    p_compliance_results: "@compliance_validation.validation_summary"
                    p_risk_parameters: {
                        type: "JSON"
                        content: {
                            risk_appetite: "MEDIUM",
                            max_exposure_usd: 500000,
                            geographic_limits: {
                                high_risk_countries: ["..."],
                                max_amount_high_risk: 25000
                            }
                        }
                    }
                }
                output_capture: {
                    risk_scores: "OUT"
                    approved_transactions: "OUT"
                    rejected_transactions: "OUT"
                }
            }
        },
        {
            step_id: "execute_approved_transactions"
            type: "API_CALL"
            depends_on: ["risk_assessment"]
            description: "Execute the approved transactions"
            api_call: {
                package: "transaction_engine"
                procedure: "execute_transaction_batch"
                parameters: {
                    p_transactions: "@risk_assessment.approved_transactions"
                    p_execution_options: {
                        type: "JSON"
                        content: {
                            execution_mode: "ATOMIC",
                            settlement_date: "T+2",
                            notification_channels: ["EMAIL", "WEBHOOK"],
                            audit_level: "DETAILED"
                        }
                    }
                }
                output_capture: {
                    execution_summary: "RETURN"
                    settlement_instructions: "OUT"
                    audit_trail: "JSON_EXTRACT($.audit_details)"
                }
            }
            error_handling: {
                on_failure: "ABORT"
                timeout_seconds: 600
            }
        }
    ]
    
    validation: {
        output_validation: [
            {
                name: "execution_summary_validation"
                source: "@execute_approved_transactions.execution_summary"
                expected: {
                    total_executed: {type: "number", min: 1},
                    success_rate: {type: "number", min: 0.95},
                    total_amount_usd: {type: "number", max: 500000},
                    processing_time_ms: {type: "number", max: 60000}
                }
                tolerance: {
                    numeric_precision: 2
                    ignore_fields: ["processing_timestamp", "batch_id"]
                }
            },
            {
                name: "compliance_score_validation"
                source: "@compliance_validation.compliance_score"
                expected: {min: 0.8, max: 1.0}
            }
        ]
        database_checks: [
            {
                name: "account_balances_updated"
                description: "Verify that account balances reflect the transactions"
                sql: """
                    SELECT a.id, a.balance, 
                           a.balance - ah.previous_balance as balance_change
                    FROM accounts a
                    JOIN account_history ah ON a.id = ah.account_id
                    WHERE ah.transaction_date > SYSDATE - INTERVAL '5' MINUTE
                """
                expected_pattern: {
                    row_count: {min: 2, max: 10}
                    balance_change: {all_negative: true, total_abs_sum: {max: 50000}}
                }
            },
            {
                name: "transaction_audit_trail"
                sql: """
                    SELECT COUNT(*) as audit_entries,
                           COUNT(DISTINCT transaction_id) as unique_transactions,
                           SUM(CASE WHEN status = 'COMPLETED' THEN 1 ELSE 0 END) as completed_count
                    FROM transaction_audit_log 
                    WHERE created_date > SYSDATE - INTERVAL '10' MINUTE
                """
                expected: {
                    audit_entries: {min: 4}, 
                    unique_transactions: {min: 2},
                    completed_count: {min: 2}
                }
            }
        ]
        side_effects: [
            {
                name: "compliance_alerts_generated"
                validation_sql: """
                    SELECT alert_type, COUNT(*) as alert_count
                    FROM compliance_alerts 
                    WHERE created_date > SYSDATE - INTERVAL '10' MINUTE
                    GROUP BY alert_type
                """
                expected_pattern: {
                    row_count: {max: 3}  // Should not generate many alerts
                }
            },
            {
                name: "settlement_instructions_created"
                validation_sql: """
                    SELECT COUNT(*) as instruction_count,
                           SUM(amount_usd) as total_settlement_amount
                    FROM settlement_instructions 
                    WHERE created_date > SYSDATE - INTERVAL '5' MINUTE
                      AND status = 'PENDING'
                """
                expected: {
                    instruction_count: {min: 1},
                    total_settlement_amount: {max: 500000}
                }
            }
        ]
        performance_checks: [
            {
                name: "execution_time_within_sla"
                source: "@execute_approved_transactions.execution_summary"
                sla_ms: 60000,
                tolerance_pct: 10
            }
        ]
    }
    
    cleanup: {
        tables: ["transaction_temp", "risk_assessment_cache"]
        sequences: []  // Keep sequences for audit
    }
    
    artifacts: {
        collect: ["execution_logs", "compliance_reports", "risk_assessments"]
        retention_days: 90
    }
}
```

## 🔄 Interactive Workflow

### 1. Definition → Preview Dashboard
```bash
# Simple test - one line
echo 'simple_test: {call: "api.validate", expect: "OK"}' > test.cue

# Compilation with preview dashboard
plt-compile test.cue --preview-dashboard
# → test.json (executable)
# → Preview dashboard in browser with planned flow
```

### 2. Execution → Live Dashboard
```bash
# Execution with automatic interactive dashboard
plt-run execute --test-name simple_test --dashboard
# → Browser opens automatically: http://localhost:8080/dashboard/run/456
# → Real-time updates: progress bars, live metrics, visual states
```

### 3. Post-mortem → Analysis Dashboard
```bash
# Complete dashboard for posterior analysis
plt-run dashboard --run-id 456
# → Navigable replay of entire execution
# → Drill-down on any step: traces + metrics + logs + suggestions
```

### 4. Debugging → Comprehensive Observability
- ✅ **Green** - steps that passed with performance metrics
- ❌ **Red** - steps that failed with complete error context
- ⏱️ **Yellow** - timeouts with resource correlation
- ⭕ **Gray** - steps not executed due to previous failures
- **🔄 Animated** - steps executing with real-time progress

## 🎯 Use Cases

### 1. **Quick API Validation** - For developers
```cue
// 30 seconds to validate your API works
quick_check: {call: "my_api.validate_customer", expect: "OK"}
```

### 2. **Integration Testing** - For QA teams
```cue
// Multi-step flows with state validation
integration_flow: {
    pipeline: [...],  // 3-5 typical steps
    validation: {...} // DB + output + side effects
}
```

### 3. **Enterprise Compliance** - For auditors
```cue
// Exhaustive tests with complete traceability
compliance_test: {
    description: "SOX compliance for financial transactions"
    execution_pipeline: [...], // 10+ steps
    validation: {...},         // Comprehensive checks
    artifacts: {collect: ["audit_trail", "compliance_reports"]}
}
```

### 4. **Performance Testing** - For DevOps
```cue
// SLA validation with tolerances
performance_test: {
    execution: {...},
    sla_validation: {
        max_duration_ms: 5000,
        tolerance_pct: 10
    }
}
```

### 5. **Interactive Debugging** - For everyone
```bash
# Complete interactive dashboard with PLTelemetry
plt-run execute --test-name complex_flow --dashboard
# → Web dashboard with:
#   • Interactive diagram (click nodes → drill-down)
#   • Real-time metrics and correlated traces
#   • Data flow visualization between steps
#   • Automatic smart insights with suggestions
```

### 6. **Executive Reporting** - For stakeholders
```bash
# Executive dashboard with trends and compliance
plt-run executive-dashboard --tests "financial_*" --period "last_30_days"
# → High-level view: success rates, performance trends, compliance status
```

## 🛠️ Installation & Setup

### Prerequisites
- Oracle Database 12.2+ (for JSON native types)
- Node.js 16+ (for dashboard)
- CUE 0.6+ (automatically installed)
- Docker (optional, for easy setup)

### Database Setup
```sql
-- Connect as SYSDBA and create PLTestLab schema
@install/create_schema.sql

-- Install framework (as pltestlab user)
@install/install_framework.sql
```

### Configuration
```bash
# Database connection
plt-run config set db.host "oracle-prod.company.com"
plt-run config set db.service "XE"
plt-run config set db.user "pltestlab"
plt-run config set db.password "secure_password"

# Observability (optional)
plt-run config set telemetry.endpoint "http://jaeger:14268"
plt-run config set telemetry.enabled "true"
```

## 📋 CLI Reference

### Basic Commands
```bash
# Compile CUE test to JSON with preview
plt-compile test.cue --preview-dashboard

# Execute specific test with dashboard
plt-run execute --test-id 123 --dashboard

# Run tests in parallel with live updates
plt-run execute --tag UNIT --parallel 4 --dashboard

# Validate without executing (dry-run)
plt-run execute --test-name "my_test" --dry-run

# List available tests
plt-run list

# Show configuration
plt-run config show
```

### Dashboard Commands
```bash
# Dashboard for specific run (post-mortem)
plt-run dashboard --run-id 456

# Compare two executions visually  
plt-run compare-dashboard --run-ids 456,455

# Trends and patterns dashboard
plt-run trends-dashboard --test-name financial_test

# Export dashboard as static HTML
plt-run export-dashboard --run-id 456 --output report.html
```

### Advanced Options
```bash
plt-run execute \
  --tag INTEGRATION \
  --env UAT \
  --dashboard \
  --trace \
  --compare \
  --artifacts \
  --timeout 300 \
  --parallel 4
```

### CI/CD Integration
```yaml
# GitLab CI example
test_database:
  script:
    - plt-run execute --tag UNIT --env CI --dashboard --headless
  artifacts:
    reports:
      junit: artifacts/test-results.xml
    paths:
      - artifacts/dashboard.html
```

## 🗄️ Database Schema

### Core Framework Tables
```sql
-- Test Definitions and Metadata
CREATE TABLE plt_test_definitions (
    test_id                 NUMBER PRIMARY KEY,
    test_name              VARCHAR2(200) NOT NULL UNIQUE,
    description            CLOB,
    cue_source_hash        VARCHAR2(64), -- Hash of original CUE file
    compiled_json          CLOB, -- JSON compiled from CUE
    tags                   VARCHAR2(4000), -- JSON array of tags
    environment            VARCHAR2(50),
    created_date           TIMESTAMP DEFAULT SYSTIMESTAMP,
    created_by             VARCHAR2(100),
    status                 VARCHAR2(20) DEFAULT 'ACTIVE',
    version                NUMBER DEFAULT 1
);

-- Test Execution
CREATE TABLE plt_test_runs (
    run_id                 NUMBER PRIMARY KEY,
    test_id                NUMBER REFERENCES plt_test_definitions(test_id),
    run_timestamp          TIMESTAMP DEFAULT SYSTIMESTAMP,
    environment            VARCHAR2(50),
    status                 VARCHAR2(20), -- RUNNING, PASSED, FAILED, ABORTED
    total_duration_ms      NUMBER,
    executed_by            VARCHAR2(100),
    ci_build_id            VARCHAR2(200),
    trace_id               VARCHAR2(64), -- For OpenTelemetry
    error_message          CLOB,
    artifacts_location     VARCHAR2(500)
);

-- Execution Steps
CREATE TABLE plt_execution_steps (
    step_run_id            NUMBER PRIMARY KEY,
    run_id                 NUMBER REFERENCES plt_test_runs(run_id),
    step_id                VARCHAR2(100), -- Step ID in CUE
    step_order             NUMBER,
    step_type              VARCHAR2(50), -- API_CALL, DATA_TRANSFORM, etc.
    status                 VARCHAR2(20),
    start_timestamp        TIMESTAMP,
    end_timestamp          TIMESTAMP,
    duration_ms            NUMBER,
    input_parameters       CLOB, -- JSON
    output_data            CLOB, -- JSON
    error_details          CLOB,
    retry_count            NUMBER DEFAULT 0,
    span_id                VARCHAR2(64) -- For tracing
);

-- Test Data Setup per Run
CREATE TABLE plt_test_data_setup (
    setup_id               NUMBER PRIMARY KEY,
    run_id                 NUMBER REFERENCES plt_test_runs(run_id),
    table_name             VARCHAR2(128),
    operation_type         VARCHAR2(20), -- INSERT, TRUNCATE, UPDATE
    operation_order        NUMBER,
    rows_affected          NUMBER,
    data_snapshot          CLOB, -- JSON of inserted data
    execution_timestamp    TIMESTAMP DEFAULT SYSTIMESTAMP
);

-- Validations and Results
CREATE TABLE plt_validation_results (
    validation_id          NUMBER PRIMARY KEY,
    run_id                 NUMBER REFERENCES plt_test_runs(run_id),
    validation_name        VARCHAR2(200),
    validation_type        VARCHAR2(50), -- OUTPUT, DATABASE, SIDE_EFFECT
    expected_result        CLOB, -- JSON
    actual_result          CLOB, -- JSON
    comparison_result      VARCHAR2(20), -- PASS, FAIL, TOLERANCE_PASS
    difference_details     CLOB, -- JSON with differences
    tolerance_applied      VARCHAR2(10), -- YES/NO
    execution_timestamp    TIMESTAMP DEFAULT SYSTIMESTAMP
);

-- Context Variables (for @step.output references)
CREATE TABLE plt_execution_context (
    context_id             NUMBER PRIMARY KEY,
    run_id                 NUMBER REFERENCES plt_test_runs(run_id),
    step_id                VARCHAR2(100),
    variable_name          VARCHAR2(200),
    variable_value         CLOB, -- JSON
    variable_type          VARCHAR2(50), -- NUMBER, STRING, JSON, RECORD, TABLE
    created_timestamp      TIMESTAMP DEFAULT SYSTIMESTAMP
);

-- Environment Configuration
CREATE TABLE plt_environments (
    environment_id         NUMBER PRIMARY KEY,
    environment_name       VARCHAR2(50) UNIQUE,
    database_connection    VARCHAR2(500), -- Connection string
    configuration_json     CLOB, -- Environment-specific configuration
    is_active              VARCHAR2(1) DEFAULT 'Y',
    created_date           TIMESTAMP DEFAULT SYSTIMESTAMP
);

-- Artifacts and Logs
CREATE TABLE plt_artifacts (
    artifact_id            NUMBER PRIMARY KEY,
    run_id                 NUMBER REFERENCES plt_test_runs(run_id),
    artifact_type          VARCHAR2(50), -- EXECUTION_LOG, COMPARISON, PERFORMANCE
    artifact_name          VARCHAR2(200),
    artifact_data          CLOB,
    file_location          VARCHAR2(500), -- For large files
    created_timestamp      TIMESTAMP DEFAULT SYSTIMESTAMP,
    retention_until        DATE
);
```

### PL/SQL Types for Complex Data
```sql
-- Types for complex data handling
CREATE TYPE transaction_rec_t AS OBJECT (
    transaction_id         NUMBER,
    from_account          NUMBER,
    to_account            NUMBER,
    amount_original       NUMBER,
    amount_usd            NUMBER,
    currency              VARCHAR2(3),
    transaction_type      VARCHAR2(50),
    reference_data        JSON
);

CREATE TYPE transaction_batch_t AS TABLE OF transaction_rec_t;

-- Type for validation results
CREATE TYPE validation_result_t AS OBJECT (
    validation_name       VARCHAR2(200),
    status                VARCHAR2(20),
    expected_value        JSON,
    actual_value          JSON,
    difference            JSON,
    tolerance_applied     VARCHAR2(10)
);

CREATE TYPE validation_results_t AS TABLE OF validation_result_t;

-- Type for execution context between steps
CREATE TYPE execution_context_t AS OBJECT (
    step_id               VARCHAR2(100),
    variables             JSON,
    timestamps            JSON
);
```

### Performance Indexes
```sql
-- Indexes for frequent queries
CREATE INDEX idx_plt_runs_test_env ON plt_test_runs(test_id, environment, run_timestamp);
CREATE INDEX idx_plt_steps_run_order ON plt_execution_steps(run_id, step_order);
CREATE INDEX idx_plt_context_run_step ON plt_execution_context(run_id, step_id);
CREATE INDEX idx_plt_validations_run ON plt_validation_results(run_id, validation_type);

-- Index for artifacts cleanup
CREATE INDEX idx_plt_artifacts_retention ON plt_artifacts(retention_until);

-- Index for tracing and observability
CREATE INDEX idx_plt_runs_trace ON plt_test_runs(trace_id) WHERE trace_id IS NOT NULL;
```

## 🚀 Compilation & Execution Flow

### 1. CUE Compilation Phase
```bash
# Validate and compile CUE test
cue vet financial_test.cue schema.cue
cue export financial_test.cue --out json > compiled_test.json

# Import to PLTestLab with preview
plt-run import compiled_test.json --validate --environment UAT --preview-dashboard
```

### 2. Execution Phase
```bash
# Execute specific test with interactive dashboard
plt-run execute --test-name "multi_currency_transaction_processing" \
  --environment UAT \
  --dashboard \
  --trace \
  --artifacts \
  --parallel-steps 3

# Real-time status monitoring
plt-run status --run-id 12345 --follow
```

### 3. Validation & Reporting Phase
```bash
# Generate results report
plt-run report --run-id 12345 --format json --output results.json

# Export artifacts for analysis
plt-run export-artifacts --run-id 12345 --destination ./artifacts/

# Generate executive summary
plt-run executive-report --run-id 12345 --output exec_summary.html
```

## 🔍 Key Technical Features

### Context Resolution `@step.output`
- **Runtime resolution** of variables between steps
- **Type safety** with CUE pre-validation
- **Context persistence** in database for debugging
- **Automatic serialization** bidirectional for complex Oracle types

### Complex Oracle Types Support
- **Automatic mapping** CUE → PL/SQL types
- **Native support** for RECORD, TABLE, JSON
- **Transparent serialization** for data exchange
- **Type validation** before execution

### Semantic Tolerance & Comparison
- **JSON comparison** with field exclusion
- **Configurable numeric tolerance** for floating-point comparisons
- **Pattern matching** for database validations
- **Custom comparison functions** for business logic

### Enterprise Observability
- **Distributed tracing** with OpenTelemetry integration
- **Automatic performance metrics** collection
- **Configurable artifact collection** for compliance
- **Real-time dashboard updates** via WebSocket

## 📊 Sample Output

### CLI Output
```bash
$ plt-run execute --test-name "order_processing" --dashboard

╔═══════════════════════════════════════════════════════════════╗
║                      PLTestLab CLI v1.0.0                    ║
║           Enterprise Testing Framework for Oracle             ║
║                                                               ║
║  🚀 JSON-native  📊 Observable  🔄 CI/CD Ready               ║
╚═══════════════════════════════════════════════════════════════╝

[INFO] Starting PLTestLab execution...
[INFO] Environment: UAT
[INFO] Dashboard: http://localhost:8080/dashboard/run/456
[INFO] Tracing: ENABLED
[INFO] Executing test(s)...
[SUCCESS] ✓ Test PASSED
[SUCCESS] Test execution completed in 2.3s
[INFO] Artifacts saved to /tmp/pltestlab_12345/artifacts.json
[INFO] Dashboard available for analysis and sharing
```

### JSON Output
```json
{
  "run_id": 1001,
  "test_id": 123,
  "status": "PASSED",
  "duration_ms": 2340,
  "environment": "UAT",
  "dashboard_url": "http://localhost:8080/dashboard/run/1001",
  "steps": [
    {
      "step_id": "setup_data",
      "step_type": "SETUP_DATA",
      "status": "PASSED",
      "duration_ms": 120,
      "start_timestamp": "2025-07-11T10:45:23.125Z",
      "end_timestamp": "2025-07-11T10:45:23.245Z"
    },
    {
      "step_id": "create_order",
      "step_type": "EXECUTE",
      "status": "PASSED",
      "duration_ms": 890,
      "actual_result": {"order_id": 12345, "total_amount": 1250.00},
      "captured_variables": {
        "order_id": 12345,
        "total_amount": 1250.00
      }
    },
    {
      "step_id": "process_payment",
      "step_type": "EXECUTE", 
      "status": "PASSED",
      "duration_ms": 1330,
      "dependencies_resolved": {
        "@create_order.order_id": 12345,
        "@create_order.total_amount": 1250.00
      }
    }
  ],
  "validations": [
    {
      "validation_name": "payment_result_validation",
      "validation_type": "OUTPUT",
      "status": "PASSED",
      "expected": {"status": "COMPLETED", "transaction_id": ">0"},
      "actual": {"status": "COMPLETED", "transaction_id": 67890},
      "tolerance_applied": false
    },
    {
      "validation_name": "order_status_check",
      "validation_type": "DATABASE",
      "status": "PASSED",
      "sql_executed": "SELECT status FROM orders WHERE order_id = 12345",
      "expected": {"status": "CONFIRMED"},
      "actual": {"status": "CONFIRMED"}
    }
  ],
  "artifacts": [
    {
      "type": "EXECUTION_LOG",
      "name": "detailed_execution_log",
      "location": "/artifacts/execution_1001.log"
    },
    {
      "type": "PERFORMANCE_METRICS",
      "name": "step_performance_analysis",
      "data": {
        "total_db_time_ms": 890,
        "total_api_time_ms": 1450,
        "memory_peak_mb": 45
      }
    },
    {
      "type": "TELEMETRY_TRACE",
      "name": "distributed_trace",
      "trace_id": "abc123def456",
      "jaeger_url": "http://jaeger:16686/trace/abc123def456"
    }
  ],
  "telemetry": {
    "trace_id": "abc123def456",
    "span_count": 15,
    "total_spans_duration_ms": 2340,
    "external_api_calls": 3,
    "database_queries": 8
  }
}
```

## 🔧 Configuration

### Environment Configuration
PLTestLab uses configuration files and environment variables:

```bash
# ~/.pltestlab/config
PLT_DB_HOST="oracle-prod.company.com"
PLT_DB_PORT="1521"
PLT_DB_SERVICE="PROD"
PLT_DB_USER="pltestlab"
PLT_DB_PASSWORD="secure_password"

# Observability
PLT_TELEMETRY_ENDPOINT="http://jaeger:14268"
PLT_TELEMETRY_ENABLED="true"
PLT_PROMETHEUS_ENDPOINT="http://prometheus:9090"

# Dashboard
PLT_DASHBOARD_PORT="8080"
PLT_DASHBOARD_HOST="0.0.0.0"
PLT_DASHBOARD_AUTO_OPEN="true"

# Defaults
PLT_DEFAULT_ENV="DEV"
PLT_DEFAULT_TIMEOUT="300"
PLT_ARTIFACT_RETENTION_DAYS="30"
```

### CUE Schema Configuration
```cue
// config/schema.cue - Global schema definitions
package config

#DatabaseConfig: {
    host: string
    port: int & >0 & <65536
    service: string
    user: string
    password: string
}

#TelemetryConfig: {
    enabled: bool
    endpoint?: string
    sample_rate?: number & >=0 & <=1
}

#DefaultTimeouts: {
    step_timeout_seconds: int & >0 & <=3600
    test_timeout_seconds: int & >0 & <=7200
    connection_timeout_seconds: int & >0 & <=60
}
```

## 📈 Observability & Monitoring

### Grafana Integration
PLTestLab automatically exports metrics to your observability stack:

- **Test execution rates** and success percentages
- **Performance trends** with P95/P99 latencies  
- **Error correlation** with full trace context
- **Environment comparison** (DEV vs UAT vs PROD)
- **Resource usage patterns** (CPU, Memory, DB connections)

### Distributed Tracing
Each test execution creates a complete trace with:

- **Parent span** for the entire test execution
- **Child spans** for each step (SETUP → EXECUTE → VERIFY → CLEANUP)
- **Automatic error propagation** and correlation
- **Custom attributes** for test metadata and business context
- **External service correlation** (API calls, database queries)

### Custom Metrics
```sql
-- Example custom metrics exported
pltestlab_test_duration_seconds{test_name, environment, status}
pltestlab_step_duration_seconds{test_name, step_id, step_type}
pltestlab_validation_results_total{validation_type, status}
pltestlab_artifact_size_bytes{artifact_type}
pltestlab_database_connections_active
pltestlab_api_calls_total{external_service, status_code}
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Development Setup
```bash
git clone https://github.com/yourorg/pltestlab.git
cd pltestlab
./scripts/dev-setup.sh

# Setup local Oracle instance (Docker)
docker-compose up -d oracle

# Run development environment
./scripts/dev-start.sh
```

### Running Tests
```bash
# Test the framework itself
plt-run execute --tag PLT_FRAMEWORK --env DEV --dashboard

# Run integration tests
./scripts/integration-tests.sh

# Run performance benchmarks
./scripts/performance-tests.sh
```

### Architecture Overview for Contributors

```
pltestlab/
├── cmd/                    # CLI commands
│   ├── compile/           # CUE → JSON compilation
│   ├── execute/           # Test execution engine
│   └── dashboard/         # Dashboard server
├── pkg/
│   ├── cue/              # CUE schema and validation
│   ├── oracle/           # Oracle PL/SQL integration
│   ├── dashboard/        # Web dashboard components
│   ├── telemetry/        # OpenTelemetry integration
│   └── artifacts/        # Artifact collection and storage
├── web/                   # Dashboard frontend (React/TypeScript)
│   ├── components/       # Interactive diagram components
│   ├── hooks/            # WebSocket and data hooks
│   └── utils/            # Telemetry correlation utilities
├── install/              # Database installation scripts
│   ├── schema/           # DDL for PLTestLab tables
│   ├── packages/         # PL/SQL packages
│   └── types/            # Oracle type definitions
├── examples/             # Example test definitions
│   ├── simple/           # Level 0-1 examples
│   ├── intermediate/     # Level 2 examples
│   └── enterprise/       # Level 3 examples
└── docs/                 # Documentation
    ├── api/              # API reference
    ├── guides/           # User guides
    └── architecture/     # Technical architecture docs
```

## 📚 Documentation

- **📖 Complete Documentation**: [docs.pltestlab.io](https://docs.pltestlab.io)
- **🎓 Getting Started Guide**: [docs.pltestlab.io/guide](https://docs.pltestlab.io/guide)
- **📋 API Reference**: [docs.pltestlab.io/api](https://docs.pltestlab.io/api)
- **🏗️ Architecture Deep Dive**: [docs.pltestlab.io/architecture](https://docs.pltestlab.io/architecture)
- **💬 Community**: [Slack Channel](https://pltestlab.slack.com)
- **🐛 Issues**: [GitHub Issues](https://github.com/yourorg/pltestlab/issues)
- **📧 Enterprise Support**: support@pltestlab.io

## 🛣️ Roadmap

### v1.1 - Advanced Comparisons (Q3 2025)
- **JSONPath filtering** for complex object comparison
- **TABLE/ROWSET comparison** for multi-row results
- **Custom comparison functions** for business logic
- **Fuzzy matching** for text and approximate comparisons

### v1.2 - Enhanced Observability (Q4 2025)
- **Prometheus metrics export** with custom dashboards
- **Slack/Teams notifications** for test failures
- **HTML test reports** with embedded interactive diagrams
- **Real-time collaboration** features in dashboard

### v1.3 - Enterprise Features (Q1 2026)
- **Test templates and inheritance** for reusable components
- **Approval workflows** for production test execution
- **Advanced scheduling and triggers** for automated testing
- **Multi-environment orchestration** with dependency management

### v1.4 - AI-Powered Features (Q2 2026)
- **Intelligent test generation** from existing PL/SQL code
- **Automatic performance regression detection** with ML
- **Smart error diagnosis** with suggested fixes
- **Predictive test failure analysis** based on code changes

## ⚖️ License

PLTestLab is released under the [MIT License](LICENSE).

## 🌟 Why Choose PLTestLab?

### For Developers
- **⚡ Instant feedback** - 30 seconds from test definition to visual results
- **🔍 Visual debugging** - no more digging through log files
- **📝 Self-documenting** - tests serve as living documentation
- **🚀 Progressive complexity** - start simple, scale as needed

### For QA Teams  
- **🎯 Comprehensive testing** - from unit to integration to compliance
- **📊 Clear reporting** - interactive dashboards for stakeholders
- **🔄 Repeatable processes** - consistent test execution across environments
- **👥 Collaborative debugging** - shared visual understanding of failures

### For DevOps Engineers
- **🔗 CI/CD native** - seamless integration with modern pipelines
- **📈 Observability ready** - OpenTelemetry and Prometheus out-of-the-box
- **⚙️ Infrastructure as code** - test definitions in version control
- **🏗️ Scalable architecture** - parallel execution and distributed tracing

### For Enterprise
- **🛡️ Compliance ready** - audit trails and regulatory reporting
- **💼 Cost effective** - open source alternative to expensive commercial tools
- **🔒 Security focused** - secure handling of sensitive test data
- **📋 Documentation** - complete traceability and change management

---

> **"Tan simple como quieras. Tan complejo como necesites. Hecho para humanos."**

**Built with ❤️ for the Oracle community**

Because enterprise testing shouldn't suck, and humans deserve better tools.

---

### Quick Links

- **🚀 [Get Started](https://docs.pltestlab.io/quickstart)** - 5-minute setup guide
- **📺 [Demo Videos](https://docs.pltestlab.io/demos)** - See PLTestLab in action
- **💬 [Community Chat](https://pltestlab.slack.com)** - Get help and share experiences
- **📧 [Newsletter](https://pltestlab.io/newsletter)** - Stay updated with latest features
- **🎯 [Feature Requests](https://github.com/yourorg/pltestlab/discussions)** - Shape the roadmap

**Star ⭐ this repo if PLTestLab helps you build better Oracle applications!**
