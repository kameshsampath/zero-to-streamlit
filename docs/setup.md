# Setup

By the end of this chapter you would have created and setup a GitHub Project from [Stream Starter Kit](https://github.com/streamlit/app-starter-kit).


## Create Project

Let us create a base Streamlit project from scratch using Streamlit application starter kit.

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
