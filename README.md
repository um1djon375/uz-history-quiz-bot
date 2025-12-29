# uz-history-quiz-bot
O'zbekistonning eng yangi tarixi bo‘yicha Telegram quiz bot
questions.py
bot.py
requirements.txt
import random

QUESTIONS = [
    {
        "question": "O'zbekistonning eng yangi tarixi qaysi yillarni o'z ichiga oladi?",
        "correct": "1991–hozirgi kun",
        "wrong": ["1917–1991", "1924–1990", "1990–2000"]
    },
    {
        "question": "Amir Temur qachon Movarounnahr amiri deb e'lon qilingan?",
        "correct": "1370-yil",
        "wrong": ["1365-yil", "1380-yil", "1395-yil"]
    },
    {
        "question": "Movarounnahr so'zining ma'nosi nima?",
        "correct": "Daryolar oraligʻi",
        "wrong": ["Togʻlar oraligʻi", "Choʻl hududi", "Vodiy"]
    },
]

def get_random_questions(count=20):
    return random.sample(QUESTIONS, count)
