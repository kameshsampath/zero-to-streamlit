# Interactivity

The power of Streamlit excels when we want to add user interactivity to the visualization.

By the end of this module you will have learnt how to use the following widgets,

- [x] [Sidebar](https://docs.streamlit.io/develop/api-reference/layout/st.sidebar){:target=blank}
- [x] [Selectbox](https://docs.streamlit.io/develop/api-reference/widgets/st.selectbox){:target=blank}
- [x] [Slider](https://docs.streamlit.io/develop/api-reference/widgets/st.slider){:target=blank}
- [x] [Radio](https://docs.streamlit.io/develop/api-reference/widgets/st.radio){:target=blank}

## Build Sidebar

Let us build the sidebar with the widgets to filter the input features.

Edit and update the `streamlit_app.py` with the following code,

```py linenums="1" hl_lines="39-43 50-54 62-66 74-79 86-91 93-96"
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
        ("Male", "Female"),
    )
```

## Input Features Dataframe

__TODO__: explain what we are doing here 

Edit and update the `streamlit_app.py` with the following code,

```py linenums="1" hl_lines="99-114"
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
input_penguins = pd.concat([input_df, x], axis=0)

with st.expander("Input Features"):
    st.write("**Input Penguins**")
    input_df
    st.write("**Combined Penguins Data**")
    input_penguins
```

