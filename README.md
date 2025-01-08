   import discord
   from discord.ext import commands

   # Создаем экземпляр бота
   bot = commands.Bot(command_prefix='!')

   # Пример базы данных пользователей
   user_data = {}

   @bot.event
   async def on_ready():
       print(f'Бот {bot.user} запущен!')

   @bot.command(name='profile')
   async def profile(ctx):
       user_id = str(ctx.author.id)
       
       # Получаем данные о пользователе
       profile_info = user_data.get(user_id, {"balance": 0})
       balance = profile_info["balance"]

       # Формируем ответ
       response = f'**Профиль пользователя:**\n' \
                  f'Имя: {ctx.author.name}\n' \
                  f'Идентификатор: {ctx.author.id}\n' \
                  f'Баланс: {balance} монет'
       
       await ctx.send(response)

   @bot.command(name='setbalance')
   @commands.has_permissions(administrator=True)  # Команда доступна только администраторам
   async def set_balance(ctx, member: discord.Member, amount: int):
       user_id = str(member.id)
       user_data[user_id] = {"balance": amount}
       await ctx.send(f'Баланс для {member.name} установлен на {amount} монет.')

   # Запускаем бота с токеном
   bot.run('YOUR_BOT_TOKEN_HERE')
