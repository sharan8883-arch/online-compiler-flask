from flask import Flask, render_template, request, redirect
from pymongo import MongoClient

app = Flask(__name__)

# MongoDB Connection
# Ensure MongoDB is running locally on port 27017.
client = MongoClient("mongodb://localhost:27017/")
db = client["football_db"]
matches = db["matches"]

# Home Page - list all matches
@app.route("/")
def index():
    data = list(matches.find())
    return render_template("index.html", matches=data)

# Add Match Page - GET shows form, POST adds match
@app.route("/add", methods=["GET", "POST"])
def add():
    if request.method == "POST":
        try:
            team1 = request.form["team1"]
            team2 = request.form["team2"]
            score1 = int(request.form["score1"])
            score2 = int(request.form["score2"])

            matches.insert_one({
                "team1": team1,
                "team2": team2,
                "score1": score1,
                "score2": score2
            })
            return redirect("/")
        except Exception as e:
            # For debugging purpose, print error or better show error message in template
            print(f"Error adding match: {e}")
            return render_template("add_match.html", error=str(e))
    return render_template("add_match.html")

if __name__ == "__main__":
    app.run(debug=True)
