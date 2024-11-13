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

### Create Streamlit Project

Create a Snowflake project from a template, to make things easy and clean we will create the application in `$TUTORIAL_HOME/sis`.

```shell
snow init sis --template example_streamlit
```

!!! NOTE
    The application init uses the `example_streamlit` template from  <https://github.com/snowflakedb/snowflake-cli-templates>{:target=_blank}


### Update App 

Edit and update the `$TUTORIAL_HOME/sis/streamlit_app.py` with,

```py linenums="1" hl_lines="7 18"
import streamlit as st

# import pandas to read the our data file
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from snowflake.snowpark.context import get_active_session

session = get_active_session()

st.title("🤖 Machine Learning App")

st.write("Welcome to world of Machine Learning with Streamlit.")

with st.expander("Data"):
    st.write("**Raw Data**")
    # read the csv file
    df = session.table("data.penguins")
    df
    # define and display
    st.write("**X**")
    X_raw = df.drop("species", axis=1)
    X_raw

    st.write("**y**")
    y_raw = df.species
    y_raw

with st.expander("Data Visualization"):
    st.scatter_chart(
        df,
        x="bill_length_mm",
        y="body_mass_g",
        color="species",
    )

# Ineractivity
# Columns:
# 'species', 'island', 'bill_length_mm', 'bill_depth_mm',
# 'flipper_length_mm', 'body_mass_g', 'sex'
with st.sidebar:
    st.header("Input Features")
    # Islands
    islands = df.island.unique().astype(str)
    island = st.selectbox(
        "Island",
        islands,
    )
    # Bill Length
    min, max, mean = (
        df.bill_length_mm.min(),
        df.bill_length_mm.max(),
        df.bill_length_mm.mean().round(2),
    )
    bill_length_mm = st.slider(
        "Bill Length(mm)",
        min_value=min,
        max_value=max,
        value=mean,
    )
    # Bill Depth
    min, max, mean = (
        df.bill_depth_mm.min(),
        df.bill_depth_mm.max(),
        df.bill_depth_mm.mean().round(2),
    )
    bill_depth_mm = st.slider(
        "Bill Depth(mm)",
        min_value=min,
        max_value=max,
        value=mean,
    )
    # Filpper Length
    min, max, mean = (
        df.flipper_length_mm.min().astype(float),
        df.flipper_length_mm.max().astype(float),
        df.flipper_length_mm.mean().round(2),
    )
    flipper_length_mm = st.slider(
        "Flipper Length(mm)",
        min_value=min,
        max_value=max,
        value=mean,
    )
    # Body Mass
    min, max, mean = (
        df.body_mass_g.min().astype(float),
        df.body_mass_g.max().astype(float),
        df.body_mass_g.mean().round(2),
    )
    body_mass_g = st.slider(
        "Body Mass(g)",
        min_value=min,
        max_value=max,
        value=mean,
    )
    # Gender
    gender = st.radio(
        "Gender",
        ("male", "female"),
    )

# Dataframes for Input features
data = {
    "island": island,
    "bill_length_mm": bill_length_mm,
    "bill_depth_mm": bill_depth_mm,
    "flipper_length_mm": flipper_length_mm,
    "body_mass_g": body_mass_g,
    "sex": gender,
}
input_df = pd.DataFrame(data, index=[0])
input_penguins = pd.concat([input_df, X_raw], axis=0)

with st.expander("Input Features"):
    st.write("**Input Penguins**")
    input_df
    st.write("**Combined Penguins Data**")
    input_penguins

## Data Prepration

## Encode X
X_encode = ["island", "sex"]
df_penguins = pd.get_dummies(input_penguins, prefix=X_encode)
X = df_penguins[1:]
input_row = df_penguins[:1]

## Encode Y
target_mapper = {
    "Adelie": 0,
    "Chinstrap": 1,
    "Gentoo": 2,
}


def target_encoder(val_y: str) -> int:
    return target_mapper[val_y]


y = y_raw.apply(target_encoder)

with st.expander("Data Preparation"):
    st.write("**Encoded X (input penguins)**")
    input_row
    st.write("**Encoded y**")
    y


with st.container():
    st.subheader("**Prediction Probability**")
    ## Model Training
    rf_classifier = RandomForestClassifier()
    # Fit the model
    rf_classifier.fit(X, y)
    # predict using the model
    prediction = rf_classifier.predict(input_row)
    prediction_prob = rf_classifier.predict_proba(input_row)

    # reverse the target_mapper
    p_cols = dict((v, k) for k, v in target_mapper.items())
    df_prediction_prob = pd.DataFrame(prediction_prob)
    # set the column names
    df_prediction_prob.columns = p_cols.values()
    # set the Penguin name
    df_prediction_prob.rename(columns=p_cols)

    st.dataframe(
        df_prediction_prob,
        column_config={
            "Adelie": st.column_config.ProgressColumn(
                "Adelie",
                help="Adelie",
                format="%f",
                width="medium",
                min_value=0,
                max_value=1,
            ),
            "Chinstrap": st.column_config.ProgressColumn(
                "Chinstrap",
                help="Chinstrap",
                format="%f",
                width="medium",
                min_value=0,
                max_value=1,
            ),
            "Gentoo": st.column_config.ProgressColumn(
                "Gentoo",
                help="Gentoo",
                format="%f",
                width="medium",
                min_value=0,
                max_value=1,
            ),
        },
        hide_index=True,
    )

# display the prediction
st.subheader("Predicted Species")
st.success(p_cols[prediction[0]])
```

### Verify enviroment.yml

Edit and update `$TUTORIAL_HOME/sis/environment.yml` to be like, ensuring that the packages used locally and in Snowflake are same.

```yaml
name: sf_env
channels:
  - snowflake
dependencies:
  - streamlit=1.35.0
  - snowflake-snowpark-python
  - scikit-learn=1.3.0
  - pandas=2.0.3
  - numpy=1.24.3
```


### Verify snowflake.yml

Ensure the `$TUTORIAL_HOME/sis/snowflake.yml` is upto date with your settings.

Navigate to the SiS application folder,

```shell
cd sis
```
Run the following command to deploy the application to Snowflake,

```shell
snow streamlit deploy --replace \
  --database='st_ml_app'  --schema='apps'
```

## Further Reading

__TODO__: add links