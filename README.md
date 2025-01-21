<!DOCTYPE html>
<html lang="nl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sprang Staal LMS - Home</title>
</head>
<body>
    <h1>Welkom bij het Sprang Staal Learning Management System</h1>
    <p>Ontwikkel je vaardigheden en volg je voortgang eenvoudig.</p>
    <a href="/login">Inloggen</a>
</body>
</html>
<!DOCTYPE html>
<html lang="nl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Inloggen</title>
</head>
<body>
    <h1>Inloggen in LMS</h1>
    <form method="POST">
        <label for="username">Gebruikersnaam:</label><br>
        <input type="text" id="username" name="username" required><br><br>
        
        <label for="password">Wachtwoord:</label><br>
        <input type="password" id="password" name="password" required><br><br>
        
        <button type="submit">Inloggen</button>
    </form>
    <p>Hulp nodig? Neem contact op met de beheerder.</p>
</body>
</html>
<!DOCTYPE html>
<html lang="nl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard</title>
</head>
<body>
    <h1>Welkom, {{ user.username }}</h1>
    <h2>Beschikbare trainingen:</h2>
    <ul>
        {% for training in trainings %}
            <li>
                <strong>{{ training.title }}</strong>: {{ training.description }}<br>
                Verplicht: {{ 'Ja' if training.mandatory else 'Nee' }}<br>
                <a href="/pdp">Toevoegen aan PDP</a>
            </li>
        {% endfor %}
    </ul>
    <h2>Jouw Persoonlijke Ontwikkelplannen (PDP's):</h2>
    <ul>
        {% for pdp in pdps %}
            <li>
                <strong>Training:</strong> {{ pdp.training_id }}<br>
                <strong>Doel:</strong> {{ pdp.goal }}<br>
                <strong>Status:</strong> {{ pdp.status }}<br>
                <form method="POST" action="/update_progress/{{ pdp.id }}">
                    <label for="progress">Voortgang bijwerken (%):</label>
                    <input type="number" name="progress" min="0" max="100" required>
                    <button type="submit">Bijwerken</button>
                </form>
            </li>
        {% endfor %}
    </ul>
    <a href="/add_training">Nieuwe training toevoegen</a> | <a href="/logout">Uitloggen</a>
</body>
</html>
<!DOCTYPE html>
<html lang="nl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Training Toevoegen</title>
</head>
<body>
    <h1>Nieuwe Training Toevoegen</h1>
    <form method="POST">
        <label for="title">Titel van de training:</label><br>
        <input type="text" id="title" name="title" required><br><br>
        
        <label for="description">Beschrijving:</label><br>
        <textarea id="description" name="description" rows="4" required></textarea><br><br>
        
        <label for="mandatory">Verplicht:</label>
        <input type="checkbox" id="mandatory" name="mandatory"><br><br>
        
        <button type="submit">Toevoegen</button>
    </form>
    <a href="/dashboard">Terug naar Dashboard</a>
</body>
</html>
<!DOCTYPE html>
<html lang="nl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PDP Aanmaken</title>
</head>
<body>
    <h1>Maak jouw Persoonlijk Ontwikkelplan (PDP)</h1>
    <form method="POST">
        <label for="training_id">Selecteer een training:</label><br>
        <select id="training_id" name="training_id" required>
            {% for training in trainings %}
                <option value="{{ training.id }}">{{ training.title }}</option>
            {% endfor %}
        </select><br><br>
        
        <label for="goal">Jouw doel:</label><br>
        <textarea id="goal" name="goal" rows="4" required></textarea><br><br>
        
        <button type="submit">PDP Aanmaken</button>
    </form>
    <a href="/dashboard">Terug naar Dashboard</a>
</body>
</html>
