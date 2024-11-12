# AIMERS
Artificial Intelligence Project

#Telegram Chat Bot
#AIMER Society - Indian Servers
!pip install pyTelegramBotAPI
!pip install openai
!pip install google-generativeai #For Google Gemini #AIMERS
!pip install anthropic
TelegramBOT_TOKEN = '7096176919:AAHRThrKHqMo3wW1xJBayQlsGLNg0pu9nFk'


#general

#Latest version #Gemini API #AIMER Society #IndianServers
import telebot
import os


"""
Install the Google AI Python SDK

$ pip install google-generativeai

See the getting started guide for more information:
https://ai.google.dev/gemini-api/docs/get-started/python
"""

import os

import google.generativeai as genai

genai.configure(api_key="AIzaSyB56p_5paUJyFyPvHdwXTlqFEEcoJpg0oA")

# Create the model
# See https://ai.google.dev/api/python/google/generativeai/GenerativeModel
generation_config = {
  "temperature": 1,
  "top_p": 0.95,
  "top_k": 64,
  "max_output_tokens": 8192,
  "response_mime_type": "text/plain",
}

model = genai.GenerativeModel(
  model_name="gemini-1.5-flash",
  generation_config=generation_config,
  # safety_settings = Adjust safety settings
  # See https://ai.google.dev/gemini-api/docs/safety-settings
)

chat_session = model.start_chat(
  history=[
  ]
)


bot = telebot.TeleBot(TelegramBOT_TOKEN)

@bot.message_handler(commands=['start', 'help'])
def send_welcome(message):
    bot.reply_to(message, "Welcome! Indian Servers AI Powerful BOT, Ask your questions related to UPSC.")

@bot.message_handler(func=lambda message: True)
def handle_message(message):
 try :
  print(message)
  response=chat_session.send_message(message.text)
  bot.reply_to(message, response.text)

 except Exception as e:
        print(f"An error occurred: {e}")
        bot.reply_to(message, "Sorry, I couldn't process your request.")

bot.polling()

#Latest version
import telebot
import os
import openai
from openai import OpenAI


OPENAI_API_KEY = "sk-proj-bKcwWzKAPvMr8fnkj90tT3BlbkFJaViNbMV9mKNhG8pWUIYF"
client = OpenAI(api_key=OPENAI_API_KEY)


bot = telebot.TeleBot(TelegramBOT_TOKEN)

@bot.message_handler(commands=['start', 'help'])
def send_welcome(message):
    bot.reply_to(message, "Welcome! The MOST POWERFUL AI BOT from IndianServers")

@bot.message_handler(func=lambda message: True)
def handle_message(message):
 try :
  print(message)
  completion = client.chat.completions.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": message.text},
  ]
    )
  bot.reply_to(message, completion.choices[0].message.content)
 except Exception as e:
        print(f"An error occurred: {e}")
        bot.reply_to(message, "Sorry, I couldn't process your request.")

bot.polling()

import anthropic

client = anthropic.Anthropic(
    # defaults to os.environ.get("ANTHROPIC_API_KEY")
    api_key="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",
)

bot = telebot.TeleBot(TelegramBOT_TOKEN)

@bot.message_handler(commands=['start', 'help'])
def send_welcome(message):
    bot.reply_to(message, "Welcome! The MOST POWERFUL AI BOT from IndianServers")

@bot.message_handler(func=lambda message: True)
def handle_message(message):
    try :
        print(message)
        message = client.messages.create(
        model="claude-3-opus-20240229",
        max_tokens=1000,
        temperature=0,
        messages=[
        {
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": message.text
                }
            ]
        }
        ]
        )
        bot.reply_to(message, message.content)
    except Exception as e:
        print(f"An error occurred: {e}")
        bot.reply_to(message, "Sorry, I couldn't process your request.")

bot.polling()

#Telegram Weather bot
!pip install pyTelegramBotAPI requests

import telebot
import requests
import json

TELEGRAM_API_KEY = '7096176919:AAHRThrKHqMo3wW1xJBayQlsGLNg0pu9nFk'
OPENWEATHER_API_KEY = '033f19189a8ab629d8ac47ab098e86ef'

bot = telebot.TeleBot(TELEGRAM_API_KEY)

@bot.message_handler(commands=['start', 'help'])
def send_welcome(message):
    bot.reply_to(message, "Hello! Send me a city name and I'll provide the current temperature.")

@bot.message_handler(func=lambda message: True)
def echo_all(message):
    print(message)
    city_name = message.text
    response = requests.get(f'http://api.openweathermap.org/data/2.5/weather?q={city_name}&appid={OPENWEATHER_API_KEY}')
    data = json.loads(response.text)
    if data['cod'] == 200:
        temp = data['main']['temp'] - 273.15  # Convert from Kelvin to Celsius
        bot.reply_to(message, f'The current temperature in {city_name} is {temp:.2f}°C.')
    else:
        bot.reply_to(message, 'Sorry, I could not find that city.')

bot.polling()


#Text to image 

!pip install pyTelegramBotAPI requests
!pip install torch diffusers
!pip install accelerate
!pip install transformers

import telebot
import requests
from PIL import Image
import io
from diffusers import StableDiffusionPipeline
import torch


# Set up the bot with your token
bot_token = '1468262237:AAF_-KFX_7MW36831Rx-tp4n3q1WRrKBsU0'
bot = telebot.TeleBot(bot_token)

@bot.message_handler(func=lambda message: True)
def handle_message(message):
    # Retrieve the message text
    text = message.text

    model_id = "dreamlike-art/dreamlike-photoreal-2.0"
    pipe = StableDiffusionPipeline.from_pretrained(model_id, torch_dtype=torch.float16)
    pipe = pipe.to("cuda")
    prompt = text
    image = pipe(prompt).images[0]
    bot.send_photo(message.chat.id, image)


bot.polling()
