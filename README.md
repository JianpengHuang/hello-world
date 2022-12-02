# hello-world
Testing 
This is Jay from Bentley University
"""
Dashboard created in lecture Week 10
"""

import dash
from dash import dcc
from dash import html
from dash.dependencies import Input, Output
import plotly.express as px
import pandas as pd

stylesheet = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

### pandas dataframe to html table
def generate_table(dataframe, max_rows=10):
    return html.Table([
        html.Thead(
            html.Tr([html.Th(col) for col in dataframe.columns])
        ),
        html.Tbody([
            html.Tr([
                html.Td(dataframe.iloc[i][col]) for col in dataframe.columns
            ]) for i in range(min(len(dataframe), max_rows))
        ])
    ])

app = dash.Dash(__name__, external_stylesheets=stylesheet)
server = app.server

df = pd.DataFrame({
    "University": ["Bentley", "Boston University", "Boston College",
                   "Harvard", "Brandeis", "Northeastern"],
    "Enrollment": [5314, 33720, 14171, 21015, 5825, 22207],
    "City": ["Waltham", "Boston", "Chestnut Hill",
             "Cambridge", "Waltham", "Boston"]
})

fig = px.bar(df, x="University", y="Enrollment", color="City")



app.layout = html.Div([
    html.H1('My First MA705 Dashboard!',
            style={'textAlign' : 'center'}),
    html.A('Link to Bentley',
           href='http://www.bentley.edu',
           target='_blank'),
    dcc.Graph(figure=fig,
              id='plot',
              style={'width' : '80%',
                     'align-items' : 'center'}),
    html.Div([html.H4('Cities to Display:'),
              dcc.Checklist(
                  options=[{'label': 'Waltham', 'value': 'Waltham'},
                           {'label': 'Boston', 'value': 'Boston'},
                           {'label': 'Chestnut Hill', 'value': 'Chestnut Hill'},
                           {'label': 'Cambridge', 'value': 'Cambridge'}],
                  value=['Waltham', 'Boston', 'Cambridge'],
                  id = 'checklist')],
             style={'width' : '50%', 'float' : 'right'}),
    html.Div(id='table'),
    html.Br()
    ])



@app.callback(
    Output("table", "children"),
    Input("checklist", "value")
)
def update_table(cities):
    x = df[df.City.isin(cities)].sort_values('City')
    return generate_table(x)

@app.callback(
    Output("plot", "figure"),
    Input("checklist", "value")
)
def update_plot(cities):
    df2 = df[df.City.isin(cities)].sort_values('Enrollment', ascending=False)
    fig = px.bar(df2, x="University", y="Enrollment", color="City")
    return fig



if __name__ == '__main__':
    app.run_server(debug=True)

dash==2.7.0
gunicorn==20.1.0
itsdangerous==2.1.2
numpy==1.23.4
pandas==1.5.1
plotly==5.11.0

web: gunicorn boston_universities:server

# MA705 Final Project

This repository contains files used in the MA705 dashboard project.

Use Markdown to write your readme.md file.  Here is a [cheatsheet](https://www.markdownguide.org/cheat-sheet/).

The final dashboard is deployed on Heroku [here](https://ma705bostonuniversities.herokuapp.com).

## Dashboard Description

Brief description of dashboard's purpose.

### Data Sources

Brief description of where/how you got the data and how it was processed.

- List of Sources
- List of Sources

### Other Comments

Anything you'd like to add

