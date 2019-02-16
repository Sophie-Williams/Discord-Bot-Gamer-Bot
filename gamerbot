import random
import asyncio
import aiohttp
import json
from discord import Game
from discord.ext.commands import Bot
import time
import datetime
import random
import discord

BOT_PREFIX = ("^")
TOKEN = "XXXXXXX"

client = Bot(command_prefix=BOT_PREFIX)

@client.command(name='8ball',
                description="Answers a yes/no question.",
                brief="Answers from the beyond.",
                aliases=['eight_ball', 'eightball', '8-ball'],
                pass_context=True)
async def eight_ball(context):
    possible_responses = [
        'That is a resounding no',
        'It is not looking likely',
        'Too hard to tell',
        'It is quite possible',
        'Definitely',
    ]
    await client.say(random.choice(possible_responses) + ", " + context.message.author.mention)


@client.command()
async def square(number):
    squared_value = int(number) * int(number)
    await client.say(str(number) + " squared is " + str(squared_value))

@client.command(pass_context = True)
async def kick(ctx, *, member : discord.Member = None):
    if not ctx.message.author.server_permissions.ban_members:
        await client.say(":x: Privilege too low!")
        return 
 
    if not member:
        return await client.say(ctx.message.author.mention + "Specify a user to kick!")
    try:
        await client.kick(member)
    except Exception as e:
        if 'Privilege is too low' in str(e):
            return await client.say(":x: Privilege too low!")
 
    embed = discord.Embed(description = "**%s** has been kicked!"%member.name, color = 0xFF0000)
    return await client.say(embed = embed)

@client.command(pass_context=True)
async def embed(ctx, *, text=None):
    if not text:
        text = 'What do I Put here'
    else:
        text = text.replace(' ','_')
    channel = ctx.message.channel
    embed = discord.Embed(description = "**{}**".format(text), color = 0xFF0000)
    return await client.say(embed = embed)
    
@client.command(pass_context = True)
async def dm(ctx, member : discord.Member, *, message):
    if ctx.message.author.server_permissions.administrator:
        await client.delete_message(ctx.message)
        return await client.send_message(member, message)
    if not ctx.message.author.server_permissions.administrator:
        await client.say("**You do not have permissions for this command!**")

@client.command(pass_context = True)
async def s(ctx, *args):
    if ctx.message.author.server_permissions.administrator:
        mesg = ' '.join(args)
        await client.delete_message(ctx.message)
        return await client.say(mesg)
    if not ctx.message.author.server_permissions.administrator:
        await client.say("**You do not have permissions for this command!**")

@client.command(pass_context = True, no_pm = True)
async def announce(ctx, *, announcement: str):
    if ctx.message.author.server_permissions.administrator:
     """Sends an announcement in the channel you use the command"""
    embed=discord.Embed(title = "__Announcement__", description= announcement, color = 0xFF0000)
    await client.delete_message(ctx.message)
    await client.say(embed = embed)
    if not ctx.message.author.server_permissions.administrator:
        await client.say("**You do not have permissions for this command!**")

@client.command(aliases=['p'])
async def ping():
    pingtime = time.time()
    pingms = await client.say("Please wait . . . ")
    ping = time.time() - pingtime
    await client.edit_message(pingms, "Pong!:ping_pong: The ping time is `%.01f seconds`" % ping)



@client.event
async def on_ready():
    await client.change_presence(game=Game(name="with humans"))
    print("Logged in as " + client.user.name)


@client.command()
async def bitcoin():
    url = 'https://api.coindesk.com/v1/bpi/currentprice/BTC.json'
    async with aiohttp.ClientSession() as session:  # Async HTTP request
        raw_response = await session.get(url)
        response = await raw_response.text()
        response = json.loads(response)
        await client.say("Bitcoin price is: $" + response['bpi']['USD']['rate'])
		

async def list_servers():
    await client.wait_until_ready()
    while not client.is_closed:
        print("Current servers:")
        for server in client.servers:
            print(server.name)
        await asyncio.sleep(600)


client.loop.create_task(list_servers())
client.run(TOKEN)
