# Discord-Bot

const { Collection, Client, Intents, Discord } = require('discord.js');
const client = new Client({ intents: [Intents.FLAGS.GUILDS, Intents.FLAGS.GUILD_MESSAGES]});
const schema = require('./schema');
const mongo = require('mongoose');

mongo.connect("mongodb+srv://ZiggyZonko:Zachary1@cluster0.ronej.mongodb.net/test", {
    useUnifiedTopology: true,
    useNewURLParser: true
})
const path = require('path')
const fs = require('fs')
const config = require('./config.json');
module.exports = client;
client.commands = new Collection();
client.prefix = config.prefix;
client.aliases = new Collection();
client.categories = fs.readdirSync(path.resolve('src/commands'));
["command"].forEach(handler => {
    require(path.resolve(`src/handlers/${handler}`))(client);
}); 

// functions
client.bal = (id) => new Promise(async ful => {
    const data = await schema.findOne({id});
    if(!data) return ful(0);
    ful
})
client.add = (id, coins) => {
    schema.findOne({ id }, async(err, data) => {
        if(err) throw err;
        if(data) {
            data.coins += coins; 
        } else {
            data = new schema({ id, coins })
        }
        data.save();
    })
}

client.rmv = (id, coins) => {
    schema.findOne({ id }, async(err, data) => {
        if(err) throw err;
        if(data) {
            data.coins -= coins; 
        } else {
            data = new schema({ id, coins: -coins })
        }
        data.save();
    })
}

client.login(config.token);
