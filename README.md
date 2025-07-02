// Importações necessárias
const qrcode = require('qrcode-terminal');
const { Client, Buttons, List, MessageMedia } = require('whatsapp-web.js');
const client = new Client();

// Função de delay
const delay = ms => new Promise(resolve => setTimeout(resolve, ms));

// Geração do QR Code
client.on('qr', qr => {
    qrcode.generate(qr, { small: true });
});

// Conexão bem-sucedida
client.on('ready', () => {
    console.log('Tudo certo! WhatsApp conectado.');
});

// Inicializa o cliente
client.initialize();

// Tratamento de mensagens recebidas
client.on('message', async msg => {
    if (msg.fromMe) return; // Ignora mensagens enviadas pelo próprio bot

    const body = (msg.body || '').trim();

    // Saudação e menu principal
    if (body.match(/(menu|Menu|dia|tarde|noite|oi|Oi|Olá|olá|ola|Ola)/i) && msg.from.endsWith('@c.us')) {
        const chat = await msg.getChat();
        const contact = await msg.getContact();
        const name = contact.pushname || 'Cliente';

        await delay(3000);
        await chat.sendStateTyping();
        await delay(3000);

        await client.sendMessage(msg.from,
            `oi! ${name.split(" ")[0]}, sou a atendente Thatiane. Como posso ajudá-lo hoje? Por favor, digite uma das opções abaixo:\n\n` +
            `1 - Como funciona\n2 - Valores dos planos\n3 - Orçamento de peça específica\n4 - Pacotes personalizados\n5 - Outras perguntas`
        );
    }

    // Opção 1 - Como funciona
    if (body === '1' && msg.from.endsWith('@c.us')) {
        const chat = await msg.getChat();

        await delay(3000);
        await chat.sendStateTyping();
        await delay(3000);
        await client.sendMessage(msg.from,
            'Somos uma lavanderia 100% delivery, atendendo com excelência as regiões de Planaltina-DF e Arapoangas-DF.\n\n' +
            'Realizamos orçamentos personalizados para todas as regiões do Distrito Federal, com acréscimo de taxa de entrega conforme a localidade.\n\n' +
            'Nossos prazos de entrega variam entre 3 a 7 dias úteis, de acordo com o tipo de serviço solicitado.'
        );

        await delay(3000);
        await chat.sendStateTyping();
        await delay(3000);
        await client.sendMessage(msg.from,
            'COMO FUNCIONA?\nÉ muito simples.\n\n' +
            '1º Passo\nFaça seu cadastro para que possamos continuar seu atendimento.\n\n' +
            '2º Passo\nApós seu cadastro, faremos seu orçamento e marcaremos os dias para recolher e entregar. :)'
        );

        await delay(3000);
        await chat.sendStateTyping();
        await delay(3000);
        await client.sendMessage(msg.from,
            'RECIBO DE SERVIÇO – LAVANDERIA\n' +
            'Telefone para contato:\n' +
            'Cliente:\n' +
            'Serviço:\n' +
            'Endereço de retirada:\n' +
            'Valor total: R$\n' +
            'Forma de pagamento:\n' +
            'Data da retirada:\n' +
            'Previsão de entrega:\n' +
            'Prazo de entrega: 3 dias úteis\n\n' +
            'Condições de pagamento:\n' +
            'O pagamento poderá ser realizado a qualquer momento, entre a retirada e a entrega final das peças.'
        );
    }

    // Opção 2 - Valores dos planos + imagem
    if (body === '2' && msg.from.endsWith('@c.us')) {
        const chat = await msg.getChat();

        await delay(3000);
        await chat.sendStateTyping();
        await delay(3000);

        const media = MessageMedia.fromFilePath('C:/Users/Lucas/Desktop/chatbot lavanderia/PACOTE.JPG.jpgnode chatbot.js');
        await client.sendMessage(msg.from, media);

        await client.sendMessage(msg.from,
            'Nossos planos são flexíveis e podem ser ajustados conforme a sua necessidade.'
        );
    }

    // Opção 3 - Orçamento específico
    if (body === '3' && msg.from.endsWith('@c.us')) {
        const chat = await msg.getChat();

        await delay(3000);
        await chat.sendStateTyping();
        await delay(3000);
        await client.sendMessage(msg.from,
            "Aqui trabalhamos com atenção especial às peças delicadas. Se possível, envie fotos para que possamos fazer um orçamento detalhado."
        );
    }

    // Opção 4 - Pacotes personalizados
    if (body === '4' && msg.from.endsWith('@c.us')) {
        const chat = await msg.getChat();

        await delay(3000);
        await chat.sendStateTyping();
        await delay(3000);
        await client.sendMessage(msg.from,
            "Com isso, conseguimos verificar a quantidade de peças e identificar quais são de maior interesse para incluir no seu pacote de forma personalizada."
        );
    }

    // Opção 5 - Outras dúvidas
    if (body === '5' && msg.from.endsWith('@c.us')) {
        const chat = await msg.getChat();

        await delay(3000);
        await chat.sendStateTyping();
        await delay(3000);
        await client.sendMessage(msg.from,
            'Se você tiver outras dúvidas ou precisar de mais informações, podemos continuar nosso atendimento por aqui. Qual é sua dúvida?'
        );
    }
});
