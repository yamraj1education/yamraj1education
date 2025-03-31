from flask import Flask, request, jsonify
import wikipedia

app = Flask(__name__)

@app.route('/')
def home():
    return "Welcome to Gyan Sanchar - AI-based Q&A Platform"

@app.route('/ask', methods=['GET'])
def ask_question():
    question = request.args.get('q')
    if not question:
        return jsonify({"error": "No question provided"})
    
    try:
        summary = wikipedia.summary(question, sentences=2)
        return jsonify({"question": question, "answer": summary})
    except wikipedia.exceptions.DisambiguationError as e:
        return jsonify({"error": "Too many possible answers, be more specific", "options": e.options})
    except wikipedia.exceptions.PageError:
        return jsonify({"error": "No relevant information found"})
    
if __name__ == '__main__':
    app.run(debug=True)
