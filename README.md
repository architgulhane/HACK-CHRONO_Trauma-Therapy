
# Trauma Therapy Chatbot

This project is a sophisticated, multilingual therapeutic AI assistant designed to provide support for individuals dealing with trauma, depression, anxiety, and other mental health challenges. The chatbot is capable of understanding and responding in both English and Swahili, leveraging deep learning for natural language understanding and machine translation for multilingual capabilities.

## Features

- **Intelligent Conversation:** Utilizes a deep learning model to understand user intent and provide relevant, empathetic responses based on a predefined set of conversational patterns.
- **Multilingual Support:** Automatically detects whether the user is communicating in English or Swahili and seamlessly translates the conversation to provide support in their native language.
- **Mental Health Focus:** The chatbot's knowledge base (`intents.json`) is specifically designed to handle conversations about a wide range of mental health topics, including depression, anxiety, stress, grief, and suicidal thoughts.
- **Web-Based Interface:** A simple and intuitive web interface built with Flask and basic HTML/CSS/JS allows users to interact with the chatbot easily.
- **Scalable Architecture:** The project is built with a modular structure, making it easy to extend the chatbot's knowledge, add new languages, or integrate different machine learning models.

## How It Works

The application consists of two main parts: the training pipeline and the inference/serving application.

### 1. Training (`training.py`)

The chatbot's intelligence is derived from a neural network model trained on the data provided in `intents.json`.

1.  **Data Loading:** The `intents.json` file, which contains conversational tags, user patterns, and bot responses, is loaded.
2.  **Preprocessing:** Each pattern is tokenized and lemmatized using `NLTK`. A vocabulary of all unique words is created.
3.  **Bag-of-Words:** The preprocessed text patterns are converted into a bag-of-words numerical format.
4.  **Model Architecture:** A Keras Sequential model with Dense layers and Dropout is defined. The final layer uses a `softmax` activation function to output the probability for each intent (class).
5.  **Training:** The model is trained on the bag-of-words data and the corresponding one-hot encoded intents.
6.  **Artifacts:** The script saves three critical files:
    -   `model.h5`: The trained Keras model.
    -   `texts.pkl`: The vocabulary of the chatbot.
    -   `labels.pkl`: The list of possible intents/tags.

### 2. Inference (`app.py`)

The Fast web application serves the chatbot and handles user interactions.

1.  **User Input:** The user sends a message through the web interface.
2.  **Language Detection:** `spacy-langdetect` identifies the language of the input message (English or Swahili).
3.  **Translation (Swahili to English):** If the detected language is Swahili, the message is translated to English using a pre-trained `Hugging Face Transformers` model (`Rogendo/sw-en`).
4.  **Intent Prediction:**
    - The (now English) message is cleaned and converted into a bag-of-words vector using the vocabulary from `texts.pkl`.
    - The trained Keras model (`model.h5`) predicts the intent of the message from the vector.
5.  **Response Generation:** A suitable response is randomly selected from the list of responses corresponding to the predicted intent in `intents.json`.
6.  **Translation (English to Swahili):** If the original message was in Swahili, the selected English response is translated back to Swahili using the `Rogendo/en-sw` model.
7.  **Display Response:** The final response is sent back to the user's browser.

## Technologies Used

- **Backend:** Python, Fast
- **Machine Learning:** TensorFlow, Keras, NLTK, Scikit-learn
- **Natural Language Processing:** Hugging Face Transformers (for translation), Spacy (for language detection)
- **Frontend:** HTML, CSS, JavaScript (with Fetch API)
<img width="1022" height="483" alt="image" src="https://github.com/user-attachments/assets/ed2dd9d6-315c-402b-a7c0-bd7b9cd15f24" />




  

## Project Structure

```
.
├── app.py              # Main Flask application for serving the chatbot
├── intents.json        # The chatbot's knowledge base with patterns and responses
├── model.h5            # The pre-trained Keras model for intent classification
├── training.py         # Script to train the neural network model
├── requirements.txt    # Python dependencies
├── static/             # Frontend assets (CSS, JS, images)
│   ├── styles/style.css
│   └── js/bot.js
└── templates/
    └── index.html      # HTML template for the chat interface
```

## Setup and Installation

Follow these steps to run the project locally.

1.  **Clone the Repository**
    ```bash
    git clone <repository-url>
    cd HACK-CHRONO_Trauma-Therapy
    ```

2.  **Create a Python Virtual Environment**
    ```bash
    python -m venv venv
    venv\Scripts\activate  # On Windows
    # source venv/bin/activate  # On macOS/Linux
    ```

3.  **Install Dependencies**
    Make sure you have Python 3.8+ installed.
    ```bash
    pip install -r requirements.txt
    ```
    The first time you run the application, `nltk` will download necessary packages like `punkt` and `wordnet`.

4.  **Run the Training Script (Optional)**
    If you modify `intents.json` or want to retrain the model, run the training script.
    ```bash
    python training.py
    ```
    This will regenerate `model.h5`, `texts.pkl`, and `labels.pkl`.

5.  **Run the Fast Application**
    ```bash
    python app.py
    ```

6.  **Access the Chatbot**
    Open your web browser and navigate to `http://127.0.0.1:5000`. You can now start chatting with the bot.
