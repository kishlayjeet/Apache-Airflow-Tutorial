# Apache Airflow

Apache Airflow is an open-source tool for orchestrating complex workflows and data pipelines. It allows you to define, schedule, and monitor workflows as code, making it easy to maintain and update them over time. Airflow provides a powerful and flexible framework for building data pipelines that integrate with various data sources and platforms.

![Apache Airflow](https://imgur.com/F0RniLJ.png)

## Table of Contents:

- [Installation](#installation)
- [Configuration](#configuration)
- [Basic Concepts](#basic-concepts)
- [Creating a DAG](#creating-a-dag)
- [Operators](#operators)
- [Sensors](#sensors)
- [Hooks](#hooks)
- [Executors](#executors)

## Installation

You can install Airflow using pip:

```bash
 pip install apache-airflow
```

## Configuration

After installation, you need to configure Airflow. The default configuration file is located at `/usr/local/airflow/airflow.cfg`. You can customize it to suit your needs.

To initialize the Airflow database, run the following command:

```bash
 airflow initdb
```

This will create the necessary database tables for storing information about your workflows, tasks, and their execution history.

## Basic Concepts

Airflow is built around a few key concepts:

### ~ DAGs

A Directed Acyclic Graph (DAG) is a collection of tasks that are organized in a specific order. Each task represents a unit of work that needs to be performed. Tasks can be Python functions or external scripts that are executed by Airflow.

### ~ Operators

An Operator is a type of task that performs a specific function. Airflow provides a wide range of Operators for common tasks such as file manipulation, SQL operations, email sending, and more. You can also create your own custom Operators.

### ~ Sensors

A Sensor is a type of Operator that waits for a specific condition to be met before continuing. For example, you might use a Sensor to wait for a file to appear in a specific location before processing it.

### ~ Hooks

A Hook is a way to connect to external systems from Airflow. Hooks provide a consistent API for interacting with different types of systems such as databases, cloud platforms, and more.

### ~ Executors

An Executor is responsible for executing tasks. Airflow provides several Executors out of the box, including LocalExecutor, SequentialExecutor, and CeleryExecutor.

## Creating a DAG

To create a DAG, you need to define a Python script that contains the DAG definition. Here's an example:

```python
 from airflow import DAG
 from airflow.operators.bash import BashOperator
 from datetime import datetime

 default_args = {
     'owner': 'airflow',
     'depends_on_past': False,
     'start_date': datetime(2023, 3, 1),
     'retries': 1
 }

 with DAG('my_dag', default_args=default_args,  schedule_interval='@daily') as dag:
     task_1 = BashOperator(task_id='task_1', bash_command='echo  "Hello World"')
```

In this example, we define a DAG called my_dag that runs a Bash command (echo "Hello World") every day. We also specify some default arguments such as the owner of the DAG, the start date, and the number of retries. Finally, we create a BashOperator called `task_1` that executes the Bash command.

## Operators

Airflow provides many Operators for common tasks. Here are a few examples:

### BashOperator

The BashOperator executes a Bash command.

```python
 from airflow.operators.bash import BashOperator

 task_1 = BashOperator(task_id='task_1', bash_command='echo  "Hello World"')
```

### PythonOperator

The PythonOperator executes a Python function.

```python
 from airflow.operators.python import PythonOperator

 def my_function():
     print('Hello World')

 task_1 = PythonOperator(task_id='task_1',  python_callable=my_function)
```

### SQLOperator

The SQLOperator executes a SQL query.

```python
 from airflow.operators.sql import SQLOperator

 task_1 = SQLOperator(task_id='task_1', sql='SELECT * FROM  my_table', database='my_database')
```

### DockerOperator

The DockerOperator executes a Docker container.

```python
 from airflow.operators.docker_operator import DockerOperator

 task_1 = DockerOperator(task_id='task_1', image='my_docker_image', command='python my_script.py',
                         api_version='auto', auto_remove=True)
```

### S3FileTransformOperator

The S3FileTransformOperator allows you to perform transformations on a file in S3.

```python
 from airflow.providers.amazon.aws.operators.s3_file_transform import S3FileTransformOperator

 task_1 = S3FileTransformOperator(task_id='task_1', source_s3_key='input.json', dest_s3_key='output.json',
                                  replace=False, transform_script='transform.py', source_aws_conn_id='aws_default',
                                  dest_aws_conn_id='aws_default')

```

## Sensors

Sensors are a special type of operator that wait for a specific condition to be met before continuing with the execution of the workflow. Here are a few examples:

### FileSensor

The FileSensor waits for a file to appear in a specific location.

```python
 from airflow.sensors.filesystem import FileSensor

 task_1 = FileSensor(task_id='task_1',  fs_conn_id='my_filesystem', filepath='/path/to/my/file')
```

### HttpSensor

The HttpSensor waits for an HTTP endpoint to return a specific response.

```python
 from airflow.sensors.http import HttpSensor

 task_1 = HttpSensor(task_id='task_1',  http_conn_id='my_http_connection', endpoint='/my/endpoint')
```

### TimeDeltaSensor

The TimeDeltaSensor waits for a specific amount of time to pass before continuing.

```python
 from airflow.sensors.time_delta import TimeDeltaSensor

 task_1 = TimeDeltaSensor(task_id='task_1', delta=datetime.timedelta(hours=2))
```

## Hooks

Airflow provides Hooks for various types of systems. Here are a few examples:

### PostgresHook

The PostgresHook allows you to connect to a PostgreSQL database.

```python
 from airflow.hooks.postgres_hook import PostgresHook

 hook = PostgresHook(postgres_conn_id='my_postgres_connection')
```

### S3Hook

The S3Hook allows you to interact with Amazon S3.

```python
 from airflow.hooks.S3_hook import S3Hook

 hook = S3Hook(aws_conn_id='my_aws_connection')
```

### HttpHook

The HttpHook allows you to interact with HTTP endpoints.

```python
 from airflow.hooks.http_hook import HttpHook

 hook = HttpHook(http_conn_id='my_http_connection')
```

## Executors

Airflow provides several Executors out of the box. Here are a few examples:

### LocalExecutor

The LocalExecutor runs tasks locally in separate processes.

```python
 from airflow.executors.local_executor import LocalExecutor

 executor = LocalExecutor()
```

### SequentialExecutor

The SequentialExecutor runs tasks sequentially in a single process.

```python
 from airflow.executors.sequential_executor import  SequentialExecutor

 executor = SequentialExecutor()
```

### CeleryExecutor

The CeleryExecutor runs tasks using Celery.

```python
 from airflow.executors.celery_executor import CeleryExecutor

 executor = CeleryExecutor()
```

## Conclusion

In this tutorial, we covered the basics of Apache Airflow, including how to install and configure it, the key concepts of DAGs, Operators, Sensors, Hooks, and Executors, and some examples of each. With Airflow, you can build complex data pipelines and workflows with ease.

With the knowledge gained from this tutorial, you should be well-equipped to start using Airflow in your own projects.

I hope you found this tutorial helpful. If you have any questions or feedback, please feel free to reach out to me at contact.kishlayjeet@gmail.com.

## Contributing

We welcome contributions from the community! If you have an example or improvement you'd like to contribute, please open a pull request with your changes.
