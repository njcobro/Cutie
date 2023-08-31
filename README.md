# Cutie
# Import the necessary modules.
from flask import Flask, request, redirect, url_for
from flask_sqlalchemy import SQLAlchemy
# Create a Flask app.
app = Flask(__name__)
# Configure the database.
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///db.sqlite'
db = SQLAlchemy(app)
# Create a model for the users.
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    password = db.Column(db.String(80), nullable=False)
# Create a route for the login page.
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        # Get the username and password from the form.
        username = request.form['username']
        password = request.form['password']
        # Check if the user exists in the database.
        user = User.query.filter_by(username=username, password=password).first()
        # If the user exists, redirect to the home page.
        if user:
            return redirect(url_for('home'))
        # If the user does not exist, show an error message.
        else:
            return 'Invalid username or password.'
    # If the method is GET, show the login page.
    else:
        return '''
        <form method="POST">
            <input type="text" name="username" placeholder="Username">
            <input type="password" name="password" placeholder="Password">
            <input type="submit" value="Login">
        </form>
        '''
# Create a route for the home page.
@app.route('/home')
def home():
    return 'Welcome to the home page.'
# Run the app.
if __name__ == '__main__':
    app.run()
