import logging
import telebot
from telebot import types

# Задаем уровень логирования
logging.basicConfig(level=logging.INFO)

# Создаем объект бота
bot = telebot.TeleBot('6934970051:AAFNHzO7lhsa4Au_Hf4M2SleHiWwAHQQeIo')

# Обработчик команды /start
@bot.message_handler(commands=['start'])
def start_command(message):
    # Отправляем приветственное сообщение и кнопку "Подписаться на биржу"
    keyboard = types.InlineKeyboardMarkup()
    subscribe_button = types.InlineKeyboardButton(text="Подписаться на биржу", url="https://t.me/Youtubebirzha_1")
    keyboard.add(subscribe_button)
    bot.send_message(chat_id=message.chat.id, text="Привет! Это Ютуб Биржа Бот.", reply_markup=keyboard)

# Обработчик команды /menu
@bot.message_handler(commands=['menu'])
def menu_command(message):
    # Отправляем меню и кнопку "Выложить объявление"
    keyboard = types.InlineKeyboardMarkup()
    upload_ad_button = types.InlineKeyboardButton(text="Выложить объявление", callback_data="upload_ad")
    keyboard.add(upload_ad_button)
    bot.send_message(chat_id=message.chat.id, text="Меню:", reply_markup=keyboard)

# Обработчик нажатия на кнопку "Выложить объявление"
@bot.callback_query_handler(func=lambda call: call.data == 'upload_ad')
def upload_ad(callback_query):
    bot.answer_callback_query(callback_query.id)
    bot.send_message(chat_id=callback_query.from_user.id, text="Отправьте фотографии и текст объявления.Принимается только 1 фотография")

# Обработчик отправки фотографии с текстом на канал
@bot.message_handler(content_types=['photo'])
def send_ad_to_channel(message):
    caption = message.caption
    photo_id = message.photo[-1].file_id
    bot.send_photo(chat_id='@Youtubebirzha_1', photo=photo_id, caption=caption)
    bot.send_message(chat_id=message.chat.id, text="Опубликовано!")

# Запускаем бота
if __name__ == '__main__':
    bot.polling(none_stop=True)
