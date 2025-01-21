from flask import Flask, render_template, redirect, url_for, request, flash
from flask_sqlalchemy import SQLAlchemy
from flask_login import LoginManager, UserMixin, login_user, login_required, logout_user, current_user

# Initialize the Flask app
app = Flask(__name__)
app.secret_key = 'securekey'
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///lms.db'
db = SQLAlchemy(app)

# Initialize Flask-Login
login_manager = LoginManager()
login_manager.init_app(app)
login_manager.login_view = 'login'

# User Model
class User(UserMixin, db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(150), unique=True, nullable=False)
    password = db.Column(db.String(150), nullable=False)

# Training Model
class Training(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(200), nullable=False)
    description = db.Column(db.Text, nullable=False)
    progress = db.Column(db.Float, default=0.0)

@login_manager.user_loader
def load_user(user_id):
    return User.query.get(int(user_id))

# Routes
@app.route('/')
def home():
    return render_template('home.html')

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        user = User.query.filter_by(username=username).first()
        if user and user.password == password:
            login_user(user)
            return redirect(url_for('dashboard'))
        else:
            flash('Invalid username or password', 'danger')
    return render_template('login.html')

@app.route('/dashboard')
@login_required
def dashboard():
    trainings = Training.query.all()
    return render_template('dashboard.html', user=current_user, trainings=trainings)

@app.route('/logout')
@login_required
def logout():
    logout_user()
    return redirect(url_for('home'))

@app.route('/add_training', methods=['GET', 'POST'])
@login_required
def add_training():
    if request.method == 'POST':
        title = request.form['title']
        description = request.form['description']
        new_training = Training(title=title, description=description)
        db.session.add(new_training)
        db.session.commit()
        flash('Training added successfully!', 'success')
        return redirect(url_for('dashboard'))
    return render_template('add_training.html')

@app.route('/update_progress/<int:training_id>', methods=['POST'])
@login_required
def update_progress(training_id):
    training = Training.query.get_or_404(training_id)
    progress = request.form['progress']
    training.progress = float(progress)
    db.session.commit()
    flash('Progress updated successfully!', 'success')
    return redirect(url_for('dashboard'))

if __name__ == '__main__':
    db.create_all()  # Ensure the database tables are created
    app.run(debug=True)# LMS
