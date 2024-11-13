# Snowflake Deploy

By the end of this chapter you will understand how to deploy the Streamlit OSS application that we built earlier to Streamlit in Snowflake(SiS).

## Prepare for Deployment

### Database

!!!IMPORTANT
    It is assumed that Snowflake CLI has been installed and you have teste your snowflake connection.

    Set your default connection name using 

    - `snow connection set-default <your snowflake connection>`
    - Set an environment variable named `SNOWFLAKE_DEFAULT_CONNECTION_NAME`
    
    And then for quick check try:

    ```shell
    snow connection test
    ```

For this demo all our Snowflake objects will be housed in a DB called `st_ml_app`.

Create the database

```shell
snow object create database \
  name='st_ml_app' \
  comment='Database used for Streamlit ML App Tutorial'
```

## Schema

Let us create one schema to hold the notebooks.


```shell
# notebooks
snow object create schema \
  name='notebooks' comment='Will hold all notebooks' \
  --database='st_ml_app'
```

Download and import the [notebook](./notebooks/sis_setup.ipynb) and follow the instructions on the notebook to prepare the environment for deployment.

## Deploy App

Create a Snowflake project from a template, to make things easy and clean we will create the application in `$TUTORIAL_HOME/app`.

```shell
snow init app --template 
```