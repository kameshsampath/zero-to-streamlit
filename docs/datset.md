# Dataset

Any machine learning application requries a dataset. To keep things simple and easy for this tutorial we gonna use the data set from from 
<https://github.com/dataprofessor/data/blob/master/penguins_cleaned.csv>.

Let us download the data locally,

```shell
mkdir -p data
curl -sSL \
  -o data/penguins_cleaned.csv \
  https://raw.githubusercontent.com/dataprofessor/data/refs/heads/master/penguins_cleaned.csv
```

## Display the data

Edit and update the `streamlit_app.py` with the following code,

```py
import streamlit as st

# import pandas to read the our data file
import pandas as pd

st.title("🤖 Machine Learning App")

st.write("Welcome to world of Machine Learning with Streamlit.")

# read the csv file
df = pd.read_csv("data/penguins_cleaned.csv")
df
```

Stage, commit and push the code to the repository.

```shell
git add .
git commit -a -m "Read and display data file"
```

Push the code to be deployed,

```shell

git push origin master
```

In few seconds you should notice the your application on Streamlit cloud refreshed with the changes.

## Add our First widget

Let us add our first Streamlit widget [expander](https://docs.streamlit.io/develop/api-reference/layout/st.expander){:target="_blank"} to allow expand and collapse of the data frame.

Edit and update the `streamlit_app.py` with the following code,

```py linenums="1" hl_lines="10-14"
import streamlit as st

# import pandas to read the our data file
import pandas as pd

st.title("🤖 Machine Learning App")

st.write("Welcome to world of Machine Learning with Streamlit.")

with st.expander("Data"):
    st.write("**Raw Data**")
    # read the csv file
    df = pd.read_csv("data/penguins_cleaned.csv")
    df
```

## Variables

As part of this machine learning application we will be predicting the penguin species(**y**) using the input variables(**X**). Let us have them displayed for our reference and clarity.

Edit and update the `streamlit_app.py` with the following code,

```py linenums="1" hl_lines="16-22"
import streamlit as st

# import pandas to read the our data file
import pandas as pd

st.title("🤖 Machine Learning App")

st.write("Welcome to world of Machine Learning with Streamlit.")

with st.expander("Data"):
    st.write("**Raw Data**")
    # read the csv file
    df = pd.read_csv("data/penguins_cleaned.csv")
    df
    # define and display
    st.write("**X**")
    x = df.drop("species",axis=1)
    x

    st.write("**y**")
    y = df.species
    y
```

## Data Visualization

Let is visualize the penguins data using a [scatter plot](https://docs.streamlit.io/develop/api-reference/charts/st.scatter_chart){:target=_blank}

Edit and update the `streamlit_app.py` with the following code,

```py linenums="1" hl_lines="16-18 20-22 25-30"
import streamlit as st

# import pandas to read the our data file
import pandas as pd

st.title("🤖 Machine Learning App")

st.write("Welcome to world of Machine Learning with Streamlit.")

with st.expander("Data"):
    st.write("**Raw Data**")
    # read the csv file
    df = pd.read_csv("data/penguins_cleaned.csv")
    df
    # define and display
    st.write("**X**")
    x = df.drop("species", axis=1)
    x

    st.write("**y**")
    y = df.species
    y

with st.expander("Data Visualization"):
    st.scatter_chart(
        df,
        x="bill_length_mm",
        y="body_mass_g",
        color="species",
    )
```