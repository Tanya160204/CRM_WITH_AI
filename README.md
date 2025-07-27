Prototype: AI-Powered CRM (Flask-based)
💡 Key Features:
Add and view customer records

AI-generated summary of interactions

AI-suggested follow-up actions

Simple web interface

📁 File Structure:
cpp
Copy
Edit
ai_crm/
├── app.py
├── templates/
│   └── index.html
├── static/
│   └── styles.css
└── requirements.txt
🔧 app.py
python
Copy
Edit
from flask import Flask, render_template, request, redirect
import openai  # or use Gemini API
import os

app = Flask(__name__)
openai.api_key = "your_openai_api_key"  # Replace with your API key

customers = []

def get_ai_summary_and_followup(notes):
    prompt = f"""
You are a CRM assistant. Given the following interaction notes:

"{notes}"

Provide:
1. A summary of the customer's concern.
2. A recommended follow-up action.
"""

    try:
        response = openai.ChatCompletion.create(
            model="gpt-4",  # Use your preferred model
            messages=[{"role": "user", "content": prompt}]
        )
        reply = response['choices'][0]['message']['content']
        return reply
    except Exception as e:
        return f"AI Error: {e}"

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        name = request.form['name']
        email = request.form['email']
        notes = request.form['notes']

        ai_response = get_ai_summary_and_followup(notes)

        customers.append({
            'name': name,
            'email': email,
            'notes': notes,
            'ai_response': ai_response
        })

        return redirect('/')

    return render_template('index.html', customers=customers)

if __name__ == '__main__':
    app.run(debug=True)
🖼️ templates/index.html
html
Copy
Edit
<!DOCTYPE html>
<html>
<head>
    <title>AI-Powered CRM</title>
    <link rel="stylesheet" href="/static/styles.css">
</head>
<body>
    <h1>AI CRM Prototype</h1>
    <form method="POST">
        <input type="text" name="name" placeholder="Customer Name" required><br>
        <input type="email" name="email" placeholder="Customer Email" required><br>
        <textarea name="notes" placeholder="Interaction Notes" required></textarea><br>
        <button type="submit">Save Customer</button>
    </form>

    <h2>Customer Records</h2>
    {% for customer in customers %}
        <div class="card">
            <strong>{{ customer.name }} ({{ customer.email }})</strong><br>
            <p><em>Notes:</em> {{ customer.notes }}</p>
            <p><strong>AI Response:</strong><br>{{ customer.ai_response }}</p>
        </div>
    {% endfor %}
</body>
</html>
🎨 static/styles.css (optional for basic styling)
css
Copy
Edit
body {
    font-family: Arial, sans-serif;
    margin: 20px;
}
input, textarea {
    width: 100%;
    margin-bottom: 10px;
    padding: 8px;
}
button {
    padding: 10px 20px;
}
.card {
    border: 1px solid #ccc;
    padding: 15px;
    margin-top: 15px;
    background: #f9f9f9;
}
📦 requirements.txt
txt
Copy
Edit
flask
openai
▶️ To Run:
bash
Copy
Edit
pip install -r requirements.txt
python app.py
Open your browser to http://127.0.0.1:5000/
