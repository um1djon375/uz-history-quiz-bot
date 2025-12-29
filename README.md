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
import os
import random
from telegram import Update, ReplyKeyboardMarkup
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, ContextTypes, filters
from questions import get_random_questions

TOKEN = os.getenv(8392945300:AAFrsHCqs0l9mFilL_uPg4inv6OpMOC1Pek)  

user_data = {}

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    questions = get_random_questions(20)
    user_data[update.effective_chat.id] = {
        "questions": questions,
        "index": 0,
        "score": 0
    }
    await send_question(update, context)

async def send_question(update, context):
    chat_id = update.effective_chat.id
    data = user_data[chat_id]
    q = data["questions"][data["index"]]

    options = q["wrong"] + [q["correct"]]
    random.shuffle(options)

    keyboard = ReplyKeyboardMarkup(
        [[opt] for opt in options],
        one_time_keyboard=True,
        resize_keyboard=True
    )

    await context.bot.send_message(
        chat_id=chat_id,
        text=f"{data['index']+1}/20\n\n{q['question']}",
        reply_markup=keyboard
    )

async def answer(update: Update, context: ContextTypes.DEFAULT_TYPE):
    chat_id = update.effective_chat.id
    data = user_data.get(chat_id)

    if not data:
        return

    q = data["questions"][data["index"]]
    if update.message.text == q["correct"]:
        data["score"] += 1

    data["index"] += 1

    if data["index"] < 20:
        await send_question(update, context)
    else:
        percent = int((data["score"] / 20) * 100)
        await context.bot.send_message(
            chat_id=chat_id,
            text=f"✅ Test tugadi!\nTo‘g‘ri javoblar: {data['score']}/20\nFoiz: {percent}%"
        )
        user_data.pop(chat_id)

async def restart(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await start(update, context)

app = ApplicationBuilder().token(TOKEN).build()
app.add_handler(CommandHandler("start", start))
app.add_handler(CommandHandler("restart", restart))
app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, answer))

app.run_polling()
python-telegram-bot==20.7
python bot.py
