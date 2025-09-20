#telebot


import telebot


TOKEN = "8496860374:AAGTeTLaza-ZHu05UQcbxmSZnXd1keGDyHU"
bot = telebot.TeleBot(TOKEN)


@bot.message_handler(commands=['start'])
def send_welcome(message):
    bot.reply_to(message, "سلام! 👋 من ربات مدیریت گروه هستم.")


@bot.message_handler(content_types=['new_chat_members'])
def welcome_new_member(message):
    for member in message.new_chat_members:
        bot.send_message(
            message.chat.id,
            f"خوش آمدی {member.first_name}! 🌹"
        )


@bot.message_handler(commands=['ban'])
def ban_user(message):
    if not message.reply_to_message:
        bot.reply_to(message, "برای بن کردن باید روی پیام کاربر ریپلای کنید.")
        return
    user_id = message.reply_to_message.from_user.id
    try:
        bot.kick_chat_member(message.chat.id, user_id)
        bot.reply_to(message, "کاربر بن شد 🚫")
    except Exception as e:
        bot.reply_to(message, f"خطا در بن کردن: {e}")


@bot.message_handler(func=lambda m: "http" in m.text.lower() if m.text else False)
def delete_links(message):
    try:
        bot.delete_message(message.chat.id, message.message_id)
        bot.send_message(message.chat.id, "ارسال لینک ممنوع است 🚫")
    except Exception as e:
        bot.send_message(message.chat.id, f"خطا در حذف پیام: {e}")


print("ربات روشن شد...")
bot.infinity_polling()


