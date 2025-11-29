# express-kupon-bot
```python
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes
from apscheduler.schedulers.background import BackgroundScheduler
import random

TOKEN = "8104055505:AAF8hW6H0eGIUf3tG7jsFurWQsD56QKIihA”

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Salom! /kupon deb yozing — sizga express kupon beraman.")

async def kupon(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(generate_kupon())

def generate_kupon():
    o'yinlar = [
        "Real Madrid vs Elche — Over 2.5",
        "Arsenal vs Chelsea — 1X",
        "Inter vs Napoli — BTTS",
"Bayern vs Dortmund — Over 1.5",
        "PSG vs Lyon — 1",
        "Milan vs Roma — X2",
        "Juventus vs Lazio — Under 3.5"
    ]
    express = random.sample(o'yinlar, 5)
    kupon = "📊 Express kupon:\n\n" + "\n".join(f"{i+1}. {o}" for i, o in enumerate(express))
    return kupon

async def auto_kupon_send(app):
    bot = await app.bot.get_me()
    chat_id = "O'Z_CHAT_IDingiz"  # Avval botga yozing, keyin /start yozing — shunda chat ID bilib olamiz
    await app.bot.send_message(chat_id=chat_id, text=generate_kupon())

app = ApplicationBuilder().token(TOKEN).build()
app.add_handler(CommandHandler("start", start))
app.add_handler(CommandHandler("kupon", kupon))

scheduler = BackgroundScheduler()
scheduler.add_job(lambda: app.create_task(auto_kupon_send(app)), 'interval', hours=1)
scheduler.start()

app.run_polling()
`
