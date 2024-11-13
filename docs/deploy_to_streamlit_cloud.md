# Deploy Application

At the end of this module you would have understood,

- [x] Installed Python packages needed for this tutorial
- [x] Deloyed the Application to Streamlit cloud

Navgiate to the `$TUTORIAL_HOME` and open the project in VS Code,

```shell
cd $TUTORIAL_HOME
code .
```


## Update packages

!!!NOTE
    Making sure we use right package version that will allow us to deploy the application to SiS in the later module.

Update the `requirements.txt` to be like,

```requirements
snowflake-cli-labs>=3.1.0,<4.0.0
streamlit==1.35.0
pandas==2.0.3
numpy==1.24.3
scikit-learn==1.3.0
```

## Create Python Environment

We will use [`miniconda`](https://docs.anaconda.com/miniconda/) to create the Python environment and install all the packages.

Create an environment file `environment.yml` like,

```yaml
name: st_ml_app
channels:
  - snowflake
  - conda-forge
dependencies:
  - python=3.11
  - pip
  - pip:
    - -r requirements.txt
```

Run the following command to create the Python virtual environment,

```shell
conda env create -f environment.yml 
```

And activate the environment,

```
conda activate st_ml_app
```

!!!TIP
    - Using [direnv](https://direnv.net) declutters your environment and you can create Python virtual environment with just one like `layout_python`
    - The project is also enabled with DevContainers, in case you want to use it with your VS Code

## Application Update

Edit and update the `streamlit_app.py` as shown

```py
import streamlit as st

st.title("🤖 Machine Learning App")

st.write("Welcome to world of Machine Learning with Streamlit.")
```

Commit and push the code your remote Github repository.


## Deploy to Streamlit Community Cloud

To deploy the application naviagate to <https://streamlit.io>, Sign-In and click **Create app**.

You will need the following details for deploying the app,

- **GitHub Repo name** - `<your-gh-user>/st-ml-app`
- **Branch** - `master`
- **Main file path** - `streamlit_app.py`
- **App URL** - choose a public url for your application e.g. `<your gh user>-ml-app`.

Any commit and push to your repository will trigger a new application update. Give it a try!

