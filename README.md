# Telegram-bot-habbits
Бот помогает формировать привычки через ежедневные напоминания. Технологии: python-telegram-bot, SQLAlchemy, APScheduler. Фичи: Статистика, уведомления, мотивационные цитаты.  
Пример кода обработчика команд:

from telegram.ext import CommandHandler, Updater
from db import Habit, Session
def start(update, context):
    update.message.reply_text("Привет! Начни новую привычку с /newhabit")
def new_habit(update, context):
    habit_name = " ".join(context.args)
    with Session() as session:
        habit = Habit(name=habit_name, user_id=update.effective_user.id)
        session.add(habit)
        session.commit()
    update.message.reply_text(f"Привычка '{habit_name}' создана!")
updater = Updater("TOKEN")
updater.dispatcher.add_handler(CommandHandler("start", start))
updater.dispatcher.add_handler(CommandHandler("newhabit", new_habit))
updater.start_polling()
