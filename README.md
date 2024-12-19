Импорт библиотек:

импортировать ведение журнала импортировать запросы из aiogram импортировать Бот, Диспетчер, исполнитель, типы

logging: используется для регистрации событий в приложении. requests: библиотека для выполнения HTTP-запросов. aiogram: библиотека для работы с Telegram Bot API.

Константы:

TOKEN_API = '7741709346:AAHkFecrqeooCrBwlhEyxUJufI7d9kH3KHM' КЛЮЧ_ПОГОДОВОГО_API = '1f86be97bad23fc8703e21181e0d64af'

API_TOKEN: токен для аутентификации бота в Telegram. WEATHER_API_KEY: ключ API для получения данных о погоде от OpenWeatherMap.

Настройка логирования:

ведение журнала.basicConfig(уровень = ведение журнала.ИНФОРМАЦИЯ)

Эта строка устанавливает уровень логирования, чтобы видеть сообщения уровня INFO и выше.

Инициализация бота и диспетчера:

bot = Бот (токен = API_TOKEN) dp = Диспетчер (бот)

Создается объект Bot с токеном. Диспетчер управляет обработчиками сообщений.

Обработчик команд /start и /help:

@dp.message_handler(команды=['start', 'help']) async def send_welcome(сообщение: типы.Message): """ Этот обработчик будет вызываться, когда пользователь отправит команду /start или /help """ await message.reply("Привет!\nЯ EchoBot!\nРаботает на aiogram.")

Этот обработчик отвечает пользователю приветственным сообщением, когда отправляются команды /start или /help.

Обработчик команды /weather:

@dp.message_handler(команды=['погода']) async def send_weather(сообщение: типы.Message): информация о погоде = get_weather_samara() await message.answer(информация о погоде)

Этот обработчик вызывает функцию get_weather_samara() и отправляет пользователю информацию о погоде.

Функция получения погоды:

def get_weather_samara(): город = «Самара» url = f"http://api.openweathermap.org/data/2.5/weather?q={город}&appid={КЛЮЧ_ПОГОДОВОГО_API}&units=metric&lang=ru" response = requests.get(url)

Устанавливается переменная city с названием города. Формируется URL для HTTP-запроса к API погоды.

Обработка ответа API:

if response.status_code == 200: data = response.json() temp = data['main']['temp'] feels_like = data['main']['feels_like'] weather_desc = data['weather'][0]['description'].capitalize() humidity = data['main']['humidity'] wind_speed = data['wind']['speed'] city_name = data['name'] country = data['sys']['country'] return ( f"🌤 Погода в {city_name}, {country}:\n" f"Температура: {temp}°C (ощущается как {feels_like}°C)\n" f"Описание: {weather_desc}\n" f"Влажность: {humidity}%\n" f"Скорость ветра: {wind_speed} м/с" )

Если ответ успешный (код 200), данные о погоде извлекаются из JSON-ответа. Извлекаются различные параметры погоды: температура, описание, влажность, скорость ветра и название города.

Обработка ошибки:

в противном случае: верните сообщение «❌ Не удалось получить данные о погоде».

Если произошла ошибка при запросе, возвращается сообщение об ошибке.

Запуск бота:

if name == 'main': executor.start_polling(dp, skip_updates=True) Этот блок кода запускает бота, обрабатывая входящие обновления. skip_updates=True позволяет игнорировать старые обновления, оставшиеся после работы в автономном режиме.
