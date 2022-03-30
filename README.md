
const { MessageEmbed } = require("discord.js");
const Command = require("../../classes/Command");
const fetch = require('node-fetch');
const generateErrorEmbed = require("../../classes/ErrorMessage");
const beauty = require('../embeds/status.json');

const command = new Command('ajuda', 'help')
    .setAliases(['comandos'])
    .setExecute(async execParams => {
        const { message, status, db } = execParams;
        
        let embed = new MessageEmbed()
            .setTitle(beauty.title_emoji + ' Atendimento UPDUOr')
            .setColor(beauty.color)
            .setTimestamp(new Date())
            .setFooter(beauty.footer)
            .addField('gay')

        let botoes = new MessageActionRow().addComponents(
            // botao 1
            new MessageButton()
                .setEmoji("◀")
                .setLabel("Voltar")
                .setStyle("PRIMARY")
                .setCustomId("voltar"),
            // botao 2
            new MessageButton()
                .setEmoji("📚")
                .setLabel("Moderação")
                .setStyle("DANGER")
                .setCustomId("mod"),
            // botao 3
            new MessageButton()
                .setEmoji("💵")
                .setLabel("Economia")
                .setStyle("SUCCESS")
                .setCustomId("eco"),
            // botao 4
            new MessageButton()
                .setEmoji("🎵")
                .setLabel("Música")
                .setStyle("PRIMARY")
                .setCustomId("musica"),
            // botao 5
            new MessageButton()
                .setEmoji("😂")
                .setLabel("Diversão")
                .setStyle("SECONDARY")
                .setCustomId("diversao")
        )

        message.author.send({
            embeds: [
                embed
            ],
            components: [
                botoes
            ]
        }).then(async (msg) => {
            let c = await msg.createMessageComponentCollector({ componentType: "BUTTON", time: 15000 })

            c.on("collect", async (int) => {
                if (!int.channel.type === "DM") return;
                if (int.user.id === message.author.id) {
                    if (int.customId === "voltar") {
                        msg.edit({
                            embeds: [
                                embed
                            ]
                        })
                    } else if (int.customId === "mod") {
                        msg.edit({
                            embeds: [
                                embed.setDescription("Comandos de Moderação:\n\n`!say [text]`")
                            ]
                        })
                    } else if (int.customId === "eco") {
                        msg.edit({
                            embeds: [
                                embed.setDescription("Comandos de Economia:\n\n`!pay [@pessoa] [quantia]`")
                            ]
                        })
                    } else if (int.customId === "musica") {
                        msg.edit({
                            embeds: [
                                embed.setDescription("Comandos de Musica:\n\n`!play [musica]`")
                            ]
                        })
                    } else if (int.customId === "diversao") {
                        msg.edit({
                            embeds: [
                                embed.setDescription("Comandos de Diversao:\n\n`!kiss [@pessoa]`")
                            ]
                        })
                    }
                }
            })

            c.on("end", coletado => {
                msg.edit({ content: "Tempo esgotado.", embeds: [], components: [] })
            })

        })
    })
command.info = 'Mostra o status do servidor';

module.exports = command;
