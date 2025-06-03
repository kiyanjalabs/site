### Content Tabs

### Generic Content

=== "Python"

```py title="Python code Block" linenums="1" hl_lines="19-25"

import streamlit as st
import pandas as pd
import numpy as np
import re
import nltk
import sklearn
from sklearn.feature_extraction.text import CountVectorizer
from nltk.corpus import stopwords
import string
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

def predict(text):
    labels = ['Not Spam', 'Spam']
    x = cv.transform(text).toarray()
    p = model.predict(x)
    s = [str(i) for i in p]
    v = int(' '.join(s))
    return str('This message is looking to be: '+labels[v])
def print_data(df, opt: str):
    """_summary_
    """
    if opt == 's': print(df.shape)    # print dataset shape ~ 2 colmns,3rows 
    if opt == 'h': print(df.head())
    if opt == 'x': print(x)
def clean_data(message):
    """_summary_
       remoe all useless characters and string from the messages in the dataset
    Args:
        message (_type_): _description_
    """
    message_without_punc = [character for character in message if character not in string.punctuation]
    message_without_punc = ''.join(message_without_punc)
    separator = ' '
    return separator.join([word for word in message_without_punc.split() if word.lower() not in stopwords.words('english')])
# NLP Model
df = pd.read_csv(r'./spam.csv')
df = df.drop(['Unnamed: 2', 'Unnamed: 3', 'Unnamed: 4'], axis=1)    # drop empty columns!
df.rename(columns = {'v1':'labels', 'v2':'message'}, inplace=True)  # rename columns!
df['labels'] = df['labels'].map({'ham':0,'spam':1}) # 2colmns,3rows 
df.drop_duplicates(inplace = True)

df['message'] = df['message'].apply(clean_data)

x = df['message']
y = df['labels'] 

cv = CountVectorizer()
x = cv.fit_transform(x)

x_train, x_test, y_train, y_test = train_test_split(x,y, test_size = 0.2, random_state = 0)

model = MultinomialNB().fit(x_train, y_train)
predictions = model.predict(x_test)

# WEBAPP DESIGN
st.title('Spam Classifier')
st.image('./spam.jpeg')
user_input = st.text_input('Please enter your message')
submit = st.button('Predict')
if submit:
    answer = predict([user_input])
    st.text(answer)
```


=== "Javascript"

```js   title="Javascript code block" hl_lines="11-13"
    const express = require('express');
    require("dotenv").config();

    const {configViewEngine} = require('./config/viewEngine');
    const {initAllWebRoutes} = require('./routes/web')
    const port = process.env.PORT || 8080;
    const app = express();

    //config view Engine
    //configViewEngine(app);
    app.use(express.static("./src/public"));
    app.set("view engine", "ejs");
    app.set("views","./src/views");

    /* routes */
    app.use('/', (req, resp) => {
        resp.render('homepage.ejs')
    })

    app.listen(port, ()=>{
        console.log(`App is running at the port ${port}`);
    });

```