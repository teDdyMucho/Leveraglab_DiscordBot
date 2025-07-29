const { Client, GatewayIntentBits } = require('discord.js');
const fetch = require('node-fetch');

// Railway ENV vars
const DISCORD_BOT_TOKEN = process.env.DISCORD_BOT_TOKEN;
const N8N_WEBHOOK_URL = process.env.N8N_WEBHOOK_URL;

const client = new Client({
  intents: [
    GatewayIntentBits.Guilds,
    GatewayIntentBits.GuildMessages,
    GatewayIntentBits.MessageContent
  ]
});

client.on('ready', () => {
  console.log(`✅ Bot logged in as ${client.user.tag}`);
});

client.on('messageCreate', async (message) => {
  if (message.author.bot) return; // ignore bot messages

  try {
    await fetch(N8N_WEBHOOK_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        id: message.id,
        channel_id: message.channel.id,
        author: {
          id: message.author.id,
          username: message.author.username
        },
        content: message.content,
        timestamp: message.createdAt
      })
    });
    console.log(`📩 Forwarded: ${message.content}`);
  } catch (err) {
    console.error("❌ Error forwarding message:", err);
  }
});

client.login(DISCORD_BOT_TOKEN);
