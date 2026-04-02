npm init -y
npm install discord.js fsconst { 
Client, 
GatewayIntentBits, 
EmbedBuilder, 
ActionRowBuilder, 
ButtonBuilder, 
ButtonStyle, 
ChannelType,
PermissionsBitField 
} = require("discord.js");

const fs = require("fs");

const client = new Client({
intents: [GatewayIntentBits.Guilds, GatewayIntentBits.GuildMessages]
});

const TOKEN = "SEU_TOKEN_AQUI";
const PIX = "augusto.anton.gato@gmail.com";

let estoque = {
"Fornecedor 1 Real": [
"login1:senha1",
"login2:senha2"
]
};

client.once("ready", () => {
console.log(`FLOW BOT online`);
});

client.on("interactionCreate", async (interaction) => {

if (!interaction.isButton()) return;

if (interaction.customId === "comprar") {

const embed = new EmbedBuilder()
.setTitle("💰 Pagamento PIX")
.setDescription(`
Valor: R$1,00

Chave PIX:
${PIX}

Após pagar clique em "Já paguei"
`)
.setColor("#000000");

const row = new ActionRowBuilder()
.addComponents(
new ButtonBuilder()
.setCustomId("paguei")
.setLabel("Já paguei")
.setStyle(ButtonStyle.Success)
);

interaction.reply({ embeds: [embed], components: [row], ephemeral: true });

}

if (interaction.customId === "paguei") {

let produto = estoque["Fornecedor 1 Real"].shift();

if (!produto) {
return interaction.reply({ content: "Sem estoque", ephemeral: true });
}

interaction.reply({
content: `✅ Pagamento confirmado\n\n📦 Produto:\n${produto}`,
ephemeral: true
});

}

if (interaction.customId === "ticket") {

const canal = await interaction.guild.channels.create({
name: `ticket-${interaction.user.username}`,
type: ChannelType.GuildText,
permissionOverwrites: [
{
id: interaction.guild.id,
deny: [PermissionsBitField.Flags.ViewChannel]
},
{
id: interaction.user.id,
allow: [PermissionsBitField.Flags.ViewChannel]
}
]
});

canal.send(`🎟️ Ticket aberto por ${interaction.user}`);
interaction.reply({ content: "Ticket aberto", ephemeral: true });

}

});

client.on("messageCreate", async (msg) => {

if (msg.content === "!painel") {

const embed = new EmbedBuilder()
.setTitle("FLOW STORE")
.setDescription(`
Fornecedor por 1 real
Entrega automática
`)
.setColor("#000000");

const row = new ActionRowBuilder()
.addComponents(
new ButtonBuilder()
.setCustomId("comprar")
.setLabel("Comprar")
.setStyle(ButtonStyle.Primary),

new ButtonBuilder()
.setCustomId("ticket")
.setLabel("Suporte")
.setStyle(ButtonStyle.Secondary)
);

msg.channel.send({ embeds: [embed], components: [row] });

}

});

client.login(ac7251af71c5e91c5afa17efb653358239a8ed3ce6d69a1bd614205ca096d0fe);node flow.js!painel
