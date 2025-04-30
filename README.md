# Dashboard-Development

#installing necessary packages
pip install dash pandas plotly

import dash
from dash import dcc, html, Input, Output
import pandas as pd
import plotly.express as px

# Sample data (can be replaced with your own)
df = pd.DataFrame({
    "Date": pd.date_range(start="2024-01-01", periods=100),
    "Category": ["A"] * 50 + ["B"] * 50,
    "Value": list(range(50)) + list(range(50, 100))
})

# Create the Dash app
app = dash.Dash(__name__)
app.title = "Data Dashboard"

# App layout
app.layout = html.Div([
    html.H1("Interactive Data Dashboard", style={"textAlign": "center"}),

    html.Div([
        html.Label("Select Category:"),
        dcc.Dropdown(
            id="category-dropdown",
            options=[{"label": cat, "value": cat} for cat in df["Category"].unique()],
            value="A",
            clearable=False
        ),
    ], style={"width": "30%", "margin": "auto"}),

    dcc.Graph(id="line-chart"),
    dcc.Graph(id="bar-chart"),
])

# Callback to update charts based on selected category
@app.callback(
    Output("line-chart", "figure"),
    Output("bar-chart", "figure"),
    Input("category-dropdown", "value")
)
def update_charts(selected_category):
    filtered_df = df[df["Category"] == selected_category]

    line_fig = px.line(filtered_df, x="Date", y="Value", title="Line Chart Over Time")
    bar_fig = px.bar(filtered_df, x="Date", y="Value", title="Bar Chart Over Time")

    return line_fig, bar_fig

# Run the app
if __name__ == "__main__":
    app.run_server(debug=True)
