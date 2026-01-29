import requests
from telegram.ext import Updater, CommandHandler
import os

TOKEN = os.getenv("7993942640:AAHgJ1a4KLKwc23trpWzTSbjlMryVhADnEA")  

def get_ton_price_usd():
    url = "https://api.coingecko.com/api/v3/simple/price?ids=toncoin&vs_currencies=usd"
    data = requests.get(url).json()
    return data["toncoin"]["usd"]

def get_exchange_rates():
    # هنا تحط أسعار الصرف حسب السوق (تحديث يدوي أو تستخدم API آخر)
    iqd_rate = 2500  # سعر الأسيا العراقي (مثلاً الدينار العراقي)
    master_rate = 2600  # سعر الماستر العراقي (مثلاً سعر بطاقة أو سعر مختلف)
    usd_rate = 1       # سعر الدولار الأمريكي

    return iqd_rate, master_rate, usd_rate

def prices(update, context):
    ton_usd = get_ton_price_usd()
    iqd_rate, master_rate, usd_rate = get_exchange_rates()

    price_asia = ton_usd * iqd_rate
    price_master = ton_usd * master_rate
    price_usd = ton_usd * usd_rate

    message = (
        f"سعر TON الحالي:\n\n"
        f"• الأسيا العراقي: {price_asia:,.0f} IQD\n"
        f"• الماستر العراقي: {price_master:,.0f} IQD\n"
        f"• الدولار الأمريكي: {price_usd:.2f} USD"
    )
    update.message.reply_text(message)

def main():
    updater = Updater(TOKEN)
    dp = updater.dispatcher

    dp.add_handler(CommandHandler("prices", prices))

    updater.start_polling()
    updater.idle()

if __name__ == "__main__":
    main()
