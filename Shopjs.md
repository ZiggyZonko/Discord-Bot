# Discord-Bot

const { Client, Message, MessageEmbed } = require('discord.js');
const items = require('../../shopItems');

module.exports = {
    name: 'shop',
    /**
     * @param {Client} client 
     * @param {Message} message
     * @param {String[]} args
     */
    run: async(client, message, args) => {
        if(items.length === 0 ) return message.reply('there are no items for sale');

        const shopList = items.map((value, index) => {
                return `**${index + 1})** ${value.item} -> ${value.price}coins!`;
            });
            
            shopList.join(', ')
            message.reply(shopList);
    },
};
