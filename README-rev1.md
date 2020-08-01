# Restful APIs using Lumen PHP Framework

## Introduction
This APIs are developed to perform following operations
- `Add` new candidate
- `Find` candidate `by ID`
- `List` all registered candidates
- `Search` candidates by `First Name`/ `Last Name`/ `Email`

All of the above API calls require `Authentication` i.e. only the authorized user can consume the APIs

### Get Started
- #### Generate `JWT_SECRET`
    In the `Terminal`, run following command:
    ```
    php artisan jwt:secret
    ```
    This will add `JWT_SECRET` to your `.env` file.

- #### DB Setup
    - Set values to the `DB configuration` parameters in the `.env` file and run follwing command:
    ```
    php artisan migrate
    ```
    This will create required tables under specified `DB`
    

## Table of Contents

<!-- toc -->
- [Requests](#requests)
  * [User](#user)
    + [Register New User](#register-new-user)
    + [User Login](#user-login)
  * [Candidate](#candidate)
    + [Register New Candidate](#register-new-candidate)
    + [Get Candidate Details by ID](#get-candidate-details-by-id)
    + [List Candidates](#list-candidates)
    + [Search Candidates](#search-candidates)
- [Response Status](#response-status)

<!-- tocstop -->
---




## Requests

### User

#### Register New User
```
{{APP_URL}}/v1/register
```

**Description:**
API to register a new User

**Method:**
`POST`

**Request Header:**

| Name | Value |
| ---- | :---: |
| `Accept` | application/json |
| `Content-Type` | application/json |

**Request Body:**
| Name | Description |
| ---- | ----------- |
| `name` | Name of the User |
| `user_name` | User name, to be used while logging in |
| `password` | Password, to be used while logging in |
| `password_confirmation` | Password confirmation |

**Response:**
| Name | Description |
| ---- | ----------- |
| `user` | User Details JSON (`name`, `user_name`, `updated_at`, `created_at`, `id`) |
| `message` | Notifies that the request was successful |
---

#### User Login
```
{{APP_URL}}/v1/login
```

**Description:**
API to login and get `Token` for registered User

**Method:**
`POST`

**Request Header:**

| Name | Value |
| ---- | :---: |
| `Accept` | application/json |
| `Content-Type` | application/json |

**Request Body:**
| Name | Description |
| ---- | ----------- |
| `user_name` | Valid User name |
| `password` | Valid Password |

**Response:**
| Name | Description |
| ---- | ----------- |
| `token` | `Token` to be used to consume APIs that require `authentication` |
| `token_type` | `Authorization Type`, Default: `bearer` |
| `expires_in` | `Token validity` in seconds |

---

### Candidate

#### Register New Candidate
```
{{APP_URL}}/v1/candidates
```

**Description:**
API to register a new Candidate

**Method:**
`POST`

**Authorization:**
`Bearer Token`
| Name | Description |
| ---- | ----------- |
| `Token` | `Token` value retrieved using [User Login](#user-login) API |

**Request Header:**
| Name | Value |
| ---- | :---: |
| `Accept` | application/json |
| `Content-Type` | application/json |

**Request Body:**
| Name | Type | Description |
| ---- | :---: | ----------- |
| `first_name` | _string_ | Candidate's first name |
| `last_name` | _string_ | Optional, Candidate's last name |
| `email` | _string_ | Optional, Candidate's valid email ID |
| `contact_number` | _string_ | Optional, Candidate's valid contact number |
| `gender` | _integer_ | Optional, Candidate's gender, `"1"` for `Male` or `"2"` for `Female` |
| `specialization` | _string_ | Optional, Candidates's specialization |
| `work_ex_year` | _integer_ | Optional, Work experience in Years |
| `candidate_dob` | _integer_ | Optional, Candidate's Date of Birth in `Unix Timestamp(Epoch)` |
| `address` | _string_ | Optional, Candidate's Full address |
| `resume` | _file_ | Optional, Candidate's Resume file, allowed types: `pdf`,`doc`,`docx`,`rtf` |

**Response:**
| Name | Description |
| ---- | ----------- |
| `message` | Notifies that the request was successful |
---

#### Get Candidate Details by ID
```
{{APP_URL}}/v1/candidates/{{id}}
```

**Description:**
API to get details of individual candidate by ID

**Method:**
`GET`

**Authorization:**
`Bearer Token`
| Name | Description |
| ---- | ----------- |
| `Token` | `Token` value retrieved using [User Login](#user-login) API |

**Request Header:**
| Name | Value |
| ---- | :---: |
| `Accept` | application/json |
| `Content-Type` | application/json |

**Request Body:**

_No specified parameters._

**Response:**
| Name | Type | Description |
| ---- | :---: | ----------- |
| `id` | _integer_ | Candidate's unique ID |
| `user_id` | _integer_ | ID of the user who had registered the candidate |
| `first_name` | _string_ | Candidate's first name |
| `last_name` | _string_ | Candidate's last name |
| `email` | _string_ | Candidate's valid email ID |
| `contact_number` | _string_ | Candidate's valid contact number |
| `gender` | _integer_ | Candidate's gender, `"1"` for `Male` or `"2"` for `Female` |
| `specialization` | _string_ | Candidates's specialization |
| `work_ex_year` | _integer_ | Work experience in Years |
| `candidate_dob` | _integer_ | Candidate's Date of Birth in `Unix Timestamp(Epoch)` |
| `address` | _string_ | Candidate's Full address |
| `resume` | _string_ | Candidate's Resume file path |
| `created_at` | _timestamp_ | Candidate created at date & time |
| `updated_at` | _timestamp_ | Candidate updated at date & time |
---

#### List Candidates
```
{{APP_URL}}/v1/candidates
```

**Description:**
API to list all candidates registered by the currently logged in user

**Method:**
`GET`

**Authorization:**
`Bearer Token`
| Name | Description |
| ---- | ----------- |
| `Token` | `Token` value retrieved using [User Login](#user-login) API |

**Request Header:**
| Name | Value |
| ---- | :---: |
| `Accept` | application/json |
| `Content-Type` | application/json |

**Request Body:**
| Name | Type | Description |
| ---- | :---: | ----------- |
| `page` | _integer_ | Specify page number for which data is to be loaded |
| `limit` | _integer_ | No. of records to be displayed per page |

**Response:**
| Name | Type | Description |
| ---- | :---: | ----------- |
| `current_page` | _integer_ | Page number for which the data is returned |
| `data` | _array(Candidate Details)_ | Details of each candidate |
| `first_page_url` | _string_ | URL of the first page |
| `from` | _integer_ | Position of first candidate from `data` _array_ in the DB |
| `next_page_url` | _string_ | URL of the next page |
| `path` | _string_ | Base Path |
| `per_page` | _integer_ | No. of records fetched per page |
| `prev_page_url` | _string_ | URL of the previous page |
| `to` | _integer_ | Position of last candidate from `data` _array_ in the DB |
---

#### Search Candidates
```
{{APP_URL}}/v1/candidates/search
```

**Description:**
API to search candidates by first name/ last name/ email ID

**Method:**
`GET`

**Authorization:**
`Bearer Token`
| Name | Description |
| ---- | ----------- |
| `Token` | `Token` value retrieved using [User Login](#user-login) API |

**Request Header:**
| Name | Value |
| ---- | :---: |
| `Accept` | application/json |
| `Content-Type` | application/json |

**Request Body:**
| Name | Type | Description |
| ---- | :---: | ----------- |
| `first_name` | _string_ | Optional, Specify first name of the candidate to be searched |
| `last_name` | _string_ | Optional, Specify last name of the candidate to be searched |
| `email` | _string_ | Optional, Specify email ID of the candidate to be searched |
| `page` | _integer_ | Specify page number for which data is to be loaded |
| `limit` | _integer_ | No. of records to be displayed per page |

**Response:**
| Name | Type | Description |
| ---- | :---: | ----------- |
| `current_page` | _integer_ | Page number for which the data is returned |
| `data` | _Array(Candidate Details)_ | Details of each candidate |
| `first_page_url` | _string_ | URL of the first page |
| `from` | _integer_ | Position of first candidate from `data` _array_ in the DB |
| `next_page_url` | _string_ | URL of the next page |
| `path` | _string_ | Base Path |
| `per_page` | _integer_ | No. of records fetched per page |
| `prev_page_url` | _string_ | URL of the previous page |
| `to` | _integer_ | Position of last candidate from `data` _array_ in the DB |
---

## Response Status

List of HTTP response status codes:
| Status Code | Description |
| ---- | --- |
| `200 OK` | The request has succeeded |
| `201 Created` | The request has succeeded and a new resource has been created as a result |
| `401 Unauthorized` | When authentication is required and has failed or has not yet been provided |
| `409 Conflict` | Indicates that the request could not be processed because of conflict in the current state of the resource |
| `422 Unprocessable Entity` | The request was well-formed but was unable to be followed due to semantic errors |
