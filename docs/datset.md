# Exploring the Penguins Dataset with Streamlit

In this chapter, we'll explore the [Penguins dataset](https://github.com/dataprofessor/data/blob/master/penguins_cleaned.csv) using Streamlit's interactive features. 

By the end of this chapter you will,

- [x] Loading, preprocessing, and preparing the dataset for visualization
- [x] Using Streamlit Expander to display dataset information and summary statistics
- [x] Creating interactive scatter plots with Streamlit Scatter chart to identify patterns and relationships
- [x] Enhancing the visualization with user interactions and filters

By the end of this chapter, you'll have a solid understanding of how to use Streamlit for data exploration and be ready to move on to building machine learning models.

## Download Datset

Let us download the data locally,

```shell
mkdir -p "$TUTORIAL_HOME/data"
curl -sSL \
  -o data/penguins_cleaned.csv \
  https://raw.githubusercontent.com/dataprofessor/data/refs/heads/master/penguins_cleaned.csv
```

## Displaying the Data

Edit and update the `streamlit_app.py` with the following code,

```py linenums="1" hl_lines="4 10-12"
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

## Application Overview

As part of this machine learning application, we will be building a simple classification model to predict penguin species (y) using input variables (X). Using Streamlit's interactive widgets, we'll display these variables to make our application user-friendly and intuitive.

This classification model will help us categorize penguins into their respective species based on their physical characteristics. The input variables and target variable will be presented through Streamlit's interface, allowing users to easily interact with and understand the prediction process.

## Adding Our First Widget

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

## Displaying the Variables

Let us create and dsisplay the input features(**X**) and target(**y**).

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

Let us visualize the penguins data using a [scatter plot](https://docs.streamlit.io/develop/api-reference/charts/st.scatter_chart){:target=_blank}

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

Now that we have our variables and target displayed for reference, let's move to the next chapter where we'll explore Streamlit's interactive features.