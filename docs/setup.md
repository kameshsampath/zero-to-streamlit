# Setup

Before we get to tutorial let us prepare our Snowflake environment


## Database

!!!IMPORTANT
    It is assumed that Snowflake CLI has been installed and you have teste your snowflake connection. For quick check try 
    ```shell
    # for a connection named trial
    snow connection test -c trial
    ```

For this demo all our Snowflake objects will be housed in a DB called `st_ml_app`.

Create the database

```shell
snow object create database \
  name='st_ml_app' \
  comment='Database used for Streamlit ML App Tutorial'
```

## Schemas

Let us create few schemas to group our Snowflake objects,

|Schema | Use|
|------- |----------------|
| apps | Will hold all applications e.g. Streamlit|
| data | Will hold all data tables  |
| github | Will hold all github repository integration |
| i_stages | All local stages |
| notebooks| Will hold all notebooks|

Create `apps` schema,

```shell
# apps
snow object create schema \
  name='apps' comment='Will hold all applications e.g. Streamlit' \
  --database='st_ml_app'
```

Create `data` schema,

```shell
# data
snow object create schema \
  name='data' comment='Will hold all data tables' \
  --database='st_ml_app'
```

Create `github` schema,

```shell
# github
snow object create schema \
  name='github' comment='Will hold all github repository integration' \
  --database='st_ml_app'
```

Create `i_stages` schema,

```shell
# i_stages
snow object create schema \
  name='i_stages' comment='All local stages' \
  --database='st_ml_app'
```

Create `notebooks` schema,

```shell
# notebooks
snow object create schema \
  name='notebooks' comment='Will hold all notebooks' \
  --database='st_ml_app'
```

## Project

Let us create a base Streamlit project from scratch using Streamlit [application starter kit](https://github.com/streamlit/app-starter-kit).

Navigate to the folder where you want to create the tutorial

```shell
cd <your app root>
```

Create the application folder,

```shel
export TUTORIAL_HOME='st-mt-app'
mkdir -p $TUTORIAL_HOME
cd $TUTORIAL_HOME
```

Create an the remote Git Repository from the template,

```shel
gh repo create st-mt-app \
   --template='streamlit/app-starter-kit' \
   --public 
```

Clone the created repository to current directory,

```shell
gh repo clone st-mt-app .
```
