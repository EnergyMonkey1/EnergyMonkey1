@dp.message_handler(commands=['start', 'help'])
async def send_welcome(message: Hi, ich bin ein Chatbot für dich.):
    
    await message.reply(f"Hallo *{message.from_user.first_name}* \n"
                        f"Ich bin Cool. \n"
                        f"Hinweis: Katzen sind _nicht_ mein Ding",parse_mode='Markdown')
                        
TOKEN = "5815513903:AAHTAN0NhPs1myAruVFjOBD7ikPj-PP8cCc"

app = Sanic("TecDaysSimpleServer")
bot = Bot(token=TOKEN)

app.static('/public', './public')


@app.get("/")
async def say_hello(request):
    return text("Hello")


@app.get("/setScore")
async def set_score(request):
    user_id = request.args.get('uid')
    score = request.args.get('score', 0)
    inline_message_id = request.args.get('imid', None)
    message_id = request.args.get('mid', None)
    chat_id = request.args.get('cid', None)
    try:
        await bot.set_game_score(
            user_id=user_id,
            score=int(score),
            inline_message_id=inline_message_id,
            message_id=message_id,
            chat_id=chat_id
        )
        logger.info(f"Score updated {score}")
    except BadRequest as e:
        logger.info(f"Score unchanged {score}")
        return empty(status=304)
    return empty(status=200)
