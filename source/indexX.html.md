--- 

title: Detection API 

language_tabs: 
   - shell 

toc_footers: 
   - <a href='#'>Sign Up for a Developer Key</a> 
   - <a href='https://github.com/lavkumarv'>Documentation Powered by lav</a> 

includes: 
   - errors 

search: true 

--- 

# Introduction 

This is the documentation for the detections API 

**Version:** 1.0.0 

# /DETECT/CREDIT
## ***POST*** 

**Summary:** Given a list of transactions, find the regular credits

**Description:** 

### HTTP Request 
`***POST*** /detect/credit` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| transactions | body | A list of transactions which are considered to be credit transactions | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 500 | Something went wrong |

# /DETECT/DEBITS
## ***POST*** 

**Summary:** Given a list of transactions, find the regular debits

**Description:** 

### HTTP Request 
`***POST*** /detect/debits` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| transactions | body | A list of transactions which are considered to be debit transactions | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 500 | Something went wrong |

# /PING
## ***GET*** 

**Summary:** Test endpoint to ensure the service is alive

**Description:** 

### HTTP Request 
`***GET*** /ping` 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |

# /GROUP/CREDIT
## ***POST*** 

**Summary:** Given a list of transactions, group them into sets

**Description:** 

### HTTP Request 
`***POST*** /group/credit` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| transactions | body | A list of transactions which are considered to be credit transactions | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 500 | Something went wrong |

# /GROUP/DEBIT
## ***POST*** 

**Summary:** Given a list of transactions, group them

**Description:** 

### HTTP Request 
`***POST*** /group/debit` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| transactions | body | A list of transactions which are considered to be debit transactions | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 500 | Something went wrong |

# /REGULARITY/CREDIT
## ***POST*** 

**Summary:** Given a group of credit transactions, identify the regular aspects

**Description:** 

### HTTP Request 
`***POST*** /regularity/credit` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| transactions | body | A list of transactions which are considered to be both credit transactions AND all members of a singular group | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 500 | Something went wrong |

# /REGULARITY/DEBIT
## ***POST*** 

**Summary:** Given a group of debit transactions, identify the regular aspects

**Description:** 

### HTTP Request 
`***POST*** /regularity/debit` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| transactions | body | A list of transactions which are considered to be both credit transactions AND all members of a singular group | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 500 | Something went wrong |

<!-- Converted with the swagger-to-slate https://github.com/lavkumarv/swagger-to-slate -->
