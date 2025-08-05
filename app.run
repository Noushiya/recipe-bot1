from flask import Flask, render_template, request, jsonify
import os
from google.genai import Client
from dotenv import load_dotenv

# Load API key from .env
load_dotenv()
GEMINI_API_KEY = os.getenv("AIzaSyB0J2kp2SZQQ5q17eVCAYdJzVRbgPiffes")

# Initialize Gemini client
client = Client(api_key="AIzaSyB0J2kp2SZQQ5q17eVCAYdJzVRbgPiffes")

app = Flask(__name__)

@app.route("/")
def home():
    return render_template("index.html")

@app.route("/chat", methods=["POST"])
def chat():
    user_message = request.json.get("message", "").lower().strip()
    print("User message received:", user_message)

    # Handle greetings and small talk
    if user_message in ["hi", "hello", "hey", "hai", "hii", "hola", "hellooo", "Hii"]:
        return jsonify({"reply": "Hi there! 👋 How can I help you today? Ask me for recipes or cooking tips."})
    if "how are you" in user_message:
        return jsonify({"reply": "I'm just a bot, but I'm doing great! How about you?"})
    if "thanks" in user_message or "thank you" in user_message:
        return jsonify({"reply": "You're welcome! 😊"})
    if user_message in ["bye", "goodbye", "see you"]:
        return jsonify({"reply": "Goodbye! Have a tasty day ahead! 🍽"})

    # Build recipe prompt
    prompt = f"""
You are a recipe assistant. The user may ask for recipes or cooking tips.
Provide:
- 2-3 recipe suggestions (title + brief description)
- Ingredients
- Step-by-step instructions
Format clearly in markdown (use **bold** for titles, bullet points for ingredients and steps).

User request: {user_message}
"""

    try:
        # Call Gemini API
        response = client.models.generate_content(
            model="gemini-2.5-flash",
            contents=prompt
        )

        # Safely extract reply text
        reply = None
        if hasattr(response, 'text') and response.text:
            reply = response.text
        elif hasattr(response, 'candidates'):
            reply = response.candidates[0].content.parts[0].text

        return jsonify({"reply": reply})

    except Exception as e:
        print("Error occurred:", e)
        return jsonify({"reply": "Error fetching reply. Please check API key or console logs."})

if __name__ == "__main__":
    port = int(os.environ.get("PORT", 5000))
    app.run(host="0.0.0.0", port=port, debug=True)
