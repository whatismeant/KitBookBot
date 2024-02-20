# KitBookBot
Подскажите в чем ошибка, не работает клавиатура в телеграмм боте


# импортируем нужые библиотеки и модули
import telebot
from telebot import types
import random
import sqlite3 as sq
# инициализируем бота
bot = telebot.TeleBot('6837840968:AAHufsZJPxaJKH9mppx_CMiS0UDdLWAEGZM')

# прописываем команды
# команда старт, начало работы с ботом
@bot.message_handler(commands=['start'])
def main(message):
    bot.send_message(message.chat.id, f'Привет,  {message.from_user.first_name}, это книжный бот KitBook и я помогу тебе подобрать книгу по вкусу!')

# переход к выбору жанра
@bot.message_handler(commands=['genres'])
def viev_genres(message):
        keyboard = types.InlineKeyboardMarkup()
        keyboard.add(types.InlineKeyboardButton("Ужасы", callback_data='scare'))
        keyboard.add(types.InlineKeyboardButton("Триллеры", callback_data='thriller'))
        keyboard.add(types.InlineKeyboardButton("Фантастика", callback_data='fiction'))
        keyboard.add(types.InlineKeyboardButton("Фэнтези", callback_data='fantasy'))
        keyboard.add(types.InlineKeyboardButton("Детективы", callback_data='detective'))
        keyboard.add(types.InlineKeyboardButton("Проза", callback_data='prose'))
        keyboard.add(types.InlineKeyboardButton("Классика", callback_data='classics'))
        keyboard.add(types.InlineKeyboardButton("Зарубежная классика", callback_data='forgeign classics'))
        bot.send_message(message.chat.id, 'Выбери любимый жанр:', reply_markup= keyboard)

@bot.callback_query_handler(lambda c: c.data == 'scare')
def scare_keyboard(call):
    if call.data == 'scare':
        kb_scare = types.InlineKeyboardMarkup()
        kb_scare.add(types.InlineKeyboardButton("вариант 1", callback_data= 'test1'))
        kb_scare.add(types.InlineKeyboardButton("вариант 2", callback_data= 'test2'))
        bot.send_message(call.message.chat.id, "Выберите понравившееся произведение", reply_markup= kb_scare)
        
# команда для перехода к списку функций

@bot.message_handler(commands=['help'])
def main(message):
    bot.send_message(message.chat.id, 'Команда /start поможет тебе начать работу с ботом\nКоманда /book предложит тебе случайную книгу\nКоманда /random_genre предложит тебе список книг случайного жанра\nКоманда /genres покажет список доступных жанров\nКоманда /help вернет к списку команд')

# ответ на любое другое не предусмотренное сообщение
@bot.message_handler()
def info(message):
    bot.send_message(message.chat.id, 'Прости, я не знаю такой команы(')

bot.polling(none_stop=True, interval=0)
