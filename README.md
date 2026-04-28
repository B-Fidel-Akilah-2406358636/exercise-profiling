# Exercise: Profiling

A Spring Boot + PostgreSQL exercise focused on performance profiling. The
application exposes endpoints over a seeded dataset of **20,000 students**,
**10 courses**, and **40,000 student-course relations**.

## Endpoints

| Endpoint | Method | Description |
|---|---|---|
| `/all-student` | GET | List all students with their courses |
| `/all-student-name` | GET | Concatenated student names |
| `/highest-gpa` | GET | Student with the highest GPA |
| `/seed-data-master` | GET | Seed students and courses |
| `/seed-student-course` | GET | Seed student-course relations |

## Performance Testing

Performance testing was performed with **Apache JMeter 5.6.3**. Each test
plan simulates **10 concurrent users**, with each user issuing one request
against the corresponding endpoint.

### Test Plan 1 — `/all-student`

**View Results Tree**

![Test Plan 1 — View Results Tree](pict/Screenshot%202026-04-28%20at%2017.53.43.png)

**View Results in Table**

![Test Plan 1 — View Results in Table](pict/Screenshot%202026-04-28%20at%2020.22.00.png)

### Test Plan 2 — `/all-student-name`

**Summary Report**

![Test Plan 2 — Summary Report](pict/Screenshot%202026-04-28%20at%2020.26.14.png)

**View Results in Table**

![Test Plan 2 — View Results in Table](pict/Screenshot%202026-04-28%20at%2020.26.10.png)

**View Results Tree**

![Test Plan 2 — View Results Tree](pict/Screenshot%202026-04-28%20at%2020.26.19.png)

**Graph Results**

![Test Plan 2 — Graph Results](pict/Screenshot%202026-04-28%20at%2020.26.23.png)

### Test Plan 3 — `/highest-gpa`

**Summary Report**

![Test Plan 3 — Summary Report](pict/Screenshot%202026-04-28%20at%2020.28.05.png)

**View Results in Table**

![Test Plan 3 — View Results in Table](pict/Screenshot%202026-04-28%20at%2020.28.10.png)

**View Results Tree**

![Test Plan 3 — View Results Tree](pict/Screenshot%202026-04-28%20at%2020.27.58.png)

**Graph Results**

![Test Plan 3 — Graph Results](pict/Screenshot%202026-04-28%20at%2020.28.02.png)

## Command-Line Execution

The same test plans were re-executed through the JMeter CLI, following the
recommended practice of running large-load tests without the GUI:

```bash
jmeter -n -t "Test Plan 1.jmx" -l testresults1.jtl
jmeter -n -t "Test Plan 2.jmx" -l testresults2.jtl
jmeter -n -t "Test Plan 3.jmx" -l testresults3.jtl
```

The resulting log files (`testresults1.jtl`, `testresults2.jtl`,
`testresults3.jtl`) are committed alongside this README. The summarised
metrics are:

| Endpoint | Samples | Avg (ms) | Min (ms) | Max (ms) | Throughput | Errors |
|---|---:|---:|---:|---:|---:|---:|
| `/all-student` | 10 | 43,593 | 42,790 | 44,068 | 0.2/s | 0 |
| `/all-student-name` | 10 | 1,263 | 979 | 1,464 | 5.3/s | 0 |
| `/highest-gpa` | 10 | 50 | 47 | 57 | 10.4/s | 0 |

The `/all-student` endpoint is by far the slowest — a strong candidate for
profiling and optimisation in the next steps of the exercise.
