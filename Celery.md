# Celery 

## What is Celery?

Celery is an **asynchronous task queue** based on distributed message passing. It's used to run tasks **in the background** instead of blocking the main application.

**Key Points:**
- Open-source Python library
- Distributed task execution
- Focuses on real-time operation
- Supports scheduling

**Common Use Cases:**
- Sending emails
- Processing images/videos
- Data processing
- Generating reports
- Web scraping
- API calls
- Notifications
- Scheduled/periodic tasks
- Heavy calculations

---

## Why Use Celery?

### Without Celery:
- User waits for task completion 
- Application becomes unresponsive 
- Poor user experience 
- Blocking operations 

### With Celery
- Tasks run in background 
- Instant user response 
- Better performance 
- Scalable architecture 
- Non-blocking operations 

---

## Core Celery Concepts

### 1. Task

A **task** is a unit of work that Celery executes asynchronously.

**Characteristics:**
- Python function decorated with `@task`
- Can be executed synchronously or asynchronously
- Can be retried on failure
- Can be scheduled

**Example:**
```python
from celery import Celery

app = Celery('tasks', broker='redis://localhost:6379/0')

@app.task
def add(x, y):
    return x + y

@app.task
def send_email(email, message):
    # Email sending logic
    pass
```

### 2. Broker (Message Queue)

The **broker** is the message transport system that passes messages between the application and workers.

**What it does:**
- Receives task messages from application
- Stores tasks in queue
- Delivers tasks to available workers
- Ensures reliable message delivery

**Popular Brokers:**
- **Redis** ✅(Most popular, fast, easy setup)
- **RabbitMQ** (Feature-rich, enterprise-grade)
- Amazon SQS
- Apache Kafka

**Broker URL Examples:**
```python
# Redis
broker_url = 'redis://localhost:6379/0'

# RabbitMQ
broker_url = 'amqp://user:password@localhost:5672//'

# Amazon SQS
broker_url = 'sqs://ABCDEFGHIJKLMNOPQRST:ZYXK7NiynG/1234567890/my-queue'
```

### 3. Worker

A **worker** is a process that executes tasks.

**Characteristics:**
- Listens to message queue
- Picks up tasks from broker
- Executes tasks
- Returns results
- Can run multiple concurrent tasks
- Can be scaled horizontally

**Starting a Worker:**
```bash
celery -A project_name worker --loglevel=info
```

**Worker Options:**
```bash
# Specify concurrency
celery -A project_name worker --concurrency=4

# Run in background (daemon)
celery -A project_name worker --detach

# Set pool type
celery -A project_name worker --pool=prefork
```

### 4. Result Backend

The **result backend** stores task results and states.

**Purpose:**
- Store task return values
- Track task status (pending, started, success, failure)
- Query task state
- Retrieve results later

**Common Backends:**
- Redis
- Database (PostgreSQL, MySQL, SQLite)
- MongoDB
- Memcached
- File system

**Note:** Optional - disable if results aren't needed to improve performance.

**Configuration:**
```python
result_backend = 'redis://localhost:6379/0'
# or
result_backend = 'db+postgresql://user:pass@localhost/dbname'
```

### 5. Beat Scheduler (Optional)

**Celery Beat** is a scheduler for periodic tasks (like cron).

**Use Cases:**
- Daily reports
- Weekly backups
- Hourly data sync
- Periodic cleanup tasks

**Example:**
```python
from celery.schedules import crontab

app.conf.beat_schedule = {
    'send-daily-report': {
        'task': 'tasks.send_report',
        'schedule': crontab(hour=9, minute=0),  # Every day at 9 AM
    },
}
```

---

## Celery Architecture

### Simple Flow:
```
┌─────────────┐
│   Client    │ (Your Application)
│  (Django/   │
│   Flask)    │
└──────┬──────┘
       │ 1. Send task
       ▼
┌─────────────┐
│   Broker    │ (Redis/RabbitMQ)
│   (Queue)   │
└──────┬──────┘
       │ 2. Store & forward
       ▼
┌─────────────┐
│   Worker    │ (Celery Worker Process)
│  (Execute)  │
└──────┬──────┘
       │ 3. Store result
       ▼
┌─────────────┐
│   Result    │ (Redis/Database)
│  Backend    │
└─────────────┘
```

### Detailed Flow:
1. **Application** creates a task
2. Task sent to **Broker** (message queue)
3. **Broker** stores task in queue
4. **Worker** picks up task from broker
5. **Worker** executes task
6. Result stored in **Result Backend**
7. **Application** can retrieve result

---

## Task Execution

### Calling Tasks

**1. Asynchronous Execution (Background):**
```python
# Using .delay()
result = add.delay(4, 6)

# Using .apply_async()
result = add.apply_async(args=[4, 6])
```

**2. Synchronous Execution (Immediate):**
```python
result = add(4, 6)  # Runs immediately, blocks
```

### Advanced Task Options

```python
# Execute with countdown (delay)
result = send_email.apply_async(
    args=['user@example.com'],
    countdown=60  # Execute after 60 seconds
)

# Execute at specific time
from datetime import datetime, timedelta
eta = datetime.now() + timedelta(hours=1)
result = send_email.apply_async(
    args=['user@example.com'],
    eta=eta
)

# Set expiration
result = process_data.apply_async(
    args=[data],
    expires=300  # Task expires in 5 minutes
)

# Set priority
result = urgent_task.apply_async(priority=9)  # 0-9, higher = higher priority
```

### Task Results

```python
# Check if task completed
result.ready()  # True/False

# Get result (blocks until complete)
result.get()

# Get result with timeout
result.get(timeout=10)  # Wait max 10 seconds

# Check task state
result.state  # PENDING, STARTED, SUCCESS, FAILURE, RETRY

# Get task ID
result.id

# Check if task succeeded
result.successful()

# Check if task failed
result.failed()
```

---

## Task Configuration

### Task Decorators

```python
from celery import Task

# Basic task
@app.task
def simple_task():
    return "Done"

# Task with options
@app.task(
    bind=True,  # Pass task instance as first argument
    max_retries=3,
    default_retry_delay=60
)
def task_with_retry(self, x, y):
    try:
        return x / y
    except ZeroDivisionError as exc:
        raise self.retry(exc=exc)

# Ignore result
@app.task(ignore_result=True)
def task_no_result():
    print("No result stored")

# Set time limit
@app.task(time_limit=300, soft_time_limit=250)
def long_task():
    # Task killed after 300 seconds
    # Warning sent at 250 seconds
    pass
```

### Task Retry

```python
@app.task(bind=True, max_retries=3)
def fetch_data(self, url):
    try:
        response = requests.get(url)
        return response.json()
    except requests.RequestException as exc:
        # Retry after 5 seconds
        raise self.retry(exc=exc, countdown=5)

# Exponential backoff
@app.task(bind=True, autoretry_for=(Exception,), retry_backoff=True)
def task_with_backoff(self):
    # Automatically retries with exponential backoff
    pass
```

---

## Celery Configuration

### Basic Configuration

```python
from celery import Celery

app = Celery('myapp')

app.conf.update(
    # Broker settings
    broker_url='redis://localhost:6379/0',
    
    # Result backend
    result_backend='redis://localhost:6379/0',
    
    # Serialization
    task_serializer='json',
    accept_content=['json'],
    result_serializer='json',
    
    # Timezone
    timezone='UTC',
    enable_utc=True,
    
    # Task settings
    task_track_started=True,
    task_time_limit=30 * 60,  # 30 minutes
    task_soft_time_limit=25 * 60,  # 25 minutes
    
    # Result settings
    result_expires=3600,  # Results expire after 1 hour
    
    # Worker settings
    worker_prefetch_multiplier=4,
    worker_max_tasks_per_child=1000,
)
```

### Common Configuration Options

```python
# Broker connection retry
broker_connection_retry_on_startup = True

# Task routing
task_routes = {
    'tasks.send_email': {'queue': 'emails'},
    'tasks.process_image': {'queue': 'images'},
}

# Task compression
task_compression = 'gzip'

# Task annotations
task_annotations = {
    'tasks.slow_task': {'rate_limit': '10/m'}  # 10 per minute
}

# Worker pool
worker_pool = 'prefork'  # or 'eventlet', 'gevent', 'solo'
worker_concurrency = 4  # Number of concurrent workers
```

---

## Monitoring & Management

### Task States

- `PENDING` - Task waiting for execution
- `STARTED` - Task started
- `SUCCESS` - Task completed successfully
- `FAILURE` - Task failed
- `RETRY` - Task is being retried
- `REVOKED` - Task revoked/cancelled

### Inspecting Workers

```bash
# List active workers
celery -A project_name inspect active

# Check worker stats
celery -A project_name inspect stats

# List registered tasks
celery -A project_name inspect registered

# Check scheduled tasks
celery -A project_name inspect scheduled

# Active tasks
celery -A project_name inspect active_queues
```

### Controlling Workers

```bash
# Shutdown worker
celery -A project_name control shutdown

# Pool restart
celery -A project_name control pool_restart

# Autoscale workers
celery -A project_name control autoscale 10 3  # max 10, min 3

# Rate limit
celery -A project_name control rate_limit tasks.send_email 10/m

# Revoke task
celery -A project_name control revoke <task_id>
```

---

## Celery Beat (Periodic Tasks)

### Installation

```bash
pip install celery[beat]
```

### Configuration

```python
from celery.schedules import crontab

app.conf.beat_schedule = {
    # Every 30 seconds
    'task-every-30-seconds': {
        'task': 'tasks.test_task',
        'schedule': 30.0,
    },
    
    # Every Monday morning at 7:30 AM
    'monday-morning-report': {
        'task': 'tasks.generate_report',
        'schedule': crontab(hour=7, minute=30, day_of_week=1),
    },
    
    # Every day at midnight
    'cleanup-at-midnight': {
        'task': 'tasks.cleanup',
        'schedule': crontab(hour=0, minute=0),
        'args': (16,),  # Pass arguments
    },
}
```

### Running Beat Scheduler

```bash
# Run beat scheduler
celery -A project_name beat --loglevel=info

# Run beat and worker together
celery -A project_name worker --beat --loglevel=info
```

### Crontab Examples

```python
# Every minute
crontab()

# Every 5 minutes
crontab(minute='*/5')

# Every hour
crontab(minute=0)

# Every day at noon
crontab(hour=12, minute=0)

# Every Monday at 8 AM
crontab(hour=8, minute=0, day_of_week=1)

# First day of month
crontab(hour=0, minute=0, day_of_month=1)

# Every weekday at 5 PM
crontab(hour=17, minute=0, day_of_week='1-5')
```

---

## When to Use Celery

###  Use Celery When:

- Task takes >2-3 seconds
- Task doesn't require immediate response
- Task can run independently
- Need to process tasks asynchronously
- Need scheduled/periodic tasks
- Need to handle high task volumes
- Need distributed task processing

###  Don't Use Celery For:

- Simple CRUD operations
- Instant UI feedback required
- Real-time user interactions
- Lightweight operations (<1 second)
- Simple background threads suffice

---

## Summary

1. **Celery** = Distributed task queue for async processing
2. **Broker** = Message queue (Redis/RabbitMQ)
3. **Worker** = Process that executes tasks
4. **Result Backend** = Stores task results (optional)
5. **Beat** = Scheduler for periodic tasks

### Basic Workflow:

```
App → Task → Broker → Worker → Result Backend
```
