# Apache_Airflow_Practice
## Exercise1: Operator Plugins
Airflow was built with the itention of allowing its users to extend and customize its functionality through <b>plugins</b>.<br>
Custom <b>Operators</b> are typically used to capture frequently used operations into a resusable form.</br>
<b>Operators</b> define the atomic steps of work that make up a DAG.</br>
In this bike share example, we can reuse the codes for "loading data from S3 into Redshift", and "check has rows" by replacing them by reusable Operators. Replacing those usages of Python operator in our DAGs with custom operators can simplify the code and our DAG definition. Another big benefit is the next time we or somebody else in the organization needs to perform the same function, they only need to use the operator we defined rather than duplicating work we've already done.<br>
In this exercise, we’ll consolidate repeated code into Operator Plugins
  1. Move the data quality check logic into a custom operator<br>
  2. Replace the data quality check PythonOperators with our new custom operator<br>
  3. Consolidate both the S3 to RedShift functions into a custom operator<br>
  4. Replace the S3 to RedShift PythonOperators with our new custom operator<br>
  5. Execute the DAG<br>

## Exercise2: Refactor a DAG
<b>Taks Boundaries</b><br>
DAG tasks should be designed such that they are:
- Atomic and have a single purpose
  - Tasks should have one well-defined purpose
  - The more work a task performs, the less clear its purpose becomes
  - Big tasks are detrimental to maintainability, understanding data lineage, and speed
- Maximize parallelism
  - Properly scoped tasks minimize dependencies and are often more easily parallelized.
  - Parallelization can offer a significant speedup in the execution of Airflow DAGs
- Make failure states obvious
  - Debugging errors in your DAGs is much simpler if your tasks perform a single task
  - By simply looking at the Airflow UI you can pinpoint precisely what went wrong when something failed
<br>
In this exercise, we’ll refactor a DAG with a single overloaded task into a DAG with several tasks with well-defined boundaries<br>
  1. Read through the DAG and identify points in the DAG that could be split apart<br>
  2. Split the DAG into multiple PythonOperators<br>
  3. Run the DAG<br>

## Exercise3: SubDAGs
Commonly repeated series of tasks within DAGs can be captured as reusable <b>subDAGs</b>. A <b>subDAG</b> is similar to a normal DAG, except that it's created by a factory function that parameterizes and returns the DAG configuration. <b>SubDAGs</b> are always used as a piece of another DAG, they do not stand on their own.
<br>
In this exercise, we’ll place our S3 to RedShift Copy operations into a SubDag.
  1. Consolidate HasRowsOperator into the SubDag<br>
  2. Reorder the tasks to take advantage of the SubDag Operators<br>

## Exercise4: Building a Full DAG
In this exercise you will construct a DAG and custom operator end-to-end on your own. Our bikeshare company would like to create a trips facts table every time we update the trips data. You've decided to make the facts table creation a custom operator so that it can be reused for other tables in the future.<br>

## Folder structure
DAG: airflow\dags\lesson3<br>
> exercise1.py<br>
> exercise2.py<br>
> exercise3\dag.py<br>
> exercise3\subdag.py<br>
> exercise4.py<br>

Operators: airflow\plugins\operators<br>
> facts_calculator.py<br>
> has_rows.py<br>
> s3_to_redshift.py<br>

