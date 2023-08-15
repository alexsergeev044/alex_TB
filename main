import telebot
from extensions import *
from config import TOKEN, keys

bot = telebot.TeleBot(TOKEN)


@bot.message_handler(commands=['start', 'help'])
def send_welcome(message):
    if message.chat.username is not None:
        text = f"Добро пожаловать, {message.chat.username}!\n\n"
    else:
        text = "Добро пожаловать!\n\n"
    bot.reply_to(message, text + "Отправьте сообщение через пробел в виде"
                                 "\n< название валюты, из которой нужно конвертировать >"
                                 "\n< название валюты, в которую нужно конвертировать >"
                                 "\n< количество конвертируемой валюты >"
                                 "\n\nКоманды:\n/start и /help - инструкция по применению;"
                                 "\n/values - список валют.")


@bot.message_handler(commands=['values'])
def values(message):
    text = "Доступные валюты:\n"
    for key in keys.keys():
        text += f"\n{key}"
    bot.reply_to(message, text)


@bot.message_handler(content_types=['text'])
def convert(message):
    try:
        val = message.text.lower().split(" ")
        result = CurrencyExcange.get_price(val, keys)
        bot.send_message(message.chat.id, result)
    except APIException as e:
        bot.send_message(message.chat.id, f"Ошибка:\n\n{e}\n\nПовторите ввод!"
                                          f"\n\nДля изучения функционала используйте команды:"
                                          f"\n/start и /help - инструкция по применению;"
                                          f"\n/values - список валют.")


bot.polling(none_stop=True)
