<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LMS Home</title>
</head>
<body>
    <h1>Welcome to the Learning Management System</h1>
    <p>This is your portal for professional growth and development.</p>
    <a href="/login">Login</a>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login</title>
</head>
<body>
    <h1>Login</h1>
    <form method="POST">
        <label for="username">Username:</label><br>
        <input type="text" id="username" name="username" required><br><br>
        
        <label for="password">Password:</label><br>
        <input type="password" id="password" name="password" required><br><br>
        
        <button type="submit">Login</button>
    </form>
    <p>Don't have an account? Contact your administrator.</p>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard</title>
</head>
<body>
    <h1>Welcome, {{ user.username }}</h1>
    <h2>Available Trainings</h2>
    <ul>
        {% for training in trainings %}
            <li>
                <strong>{{ training.title }}</strong>: {{ training.description }}<br>
                Progress: {{ training.progress }}%
                <form method="POST" action="/update_progress/{{ training.id }}">
                    <label for="progress">Update Progress (%):</label>
                    <input type="number" name="progress" min="0" max="100" required>
                    <button type="submit">Update</button>
                </form>
            </li>
        {% endfor %}
    </ul>
    <a href="/add_training">Add New Training</a> | <a href="/logout">Logout</a>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Add Training</title>
</head>
<body>
    <h1>Add a New Training</h1>
    <form method="POST">
        <label for="title">Title:</label><br>
        <input type="text" id="title" name="title" required><br><br>
        
        <label for="description">Description:</label><br>
        <textarea id="description" name="description" rows="4" required></textarea><br><br>
        
        <button type="submit">Add Training</button>
    </form>
    <a href="/dashboard">Back to Dashboard</a>
</body>
</html>

