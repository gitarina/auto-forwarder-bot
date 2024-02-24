# Auto Forwarder Bot

[EN](README.md) | [RU](README_ru.md)

This bot handles, processes, and forwards messages from a number of group chats to a single group chat to create some kind of a knowledge base. The bot automatically finds hashtags in messages sent to source chats, and forwards a message to one or more topic in the source group based on the hashtags found.

Built using the `python-telegram-bot` library v20.8, tested and run on Ubuntu 22.04 with Python v3.11.4.

> [!NOTE]
> This bot is developed for the technical writers community on Telegram and works in the [largest group](https://t.me/technicalwriters) of writers. The bot's settings prohibit anybody to add it to any other groups.

## Usage

Basically, the bot listens to all messages in source groups, parses these messages to find hashtags, and then sends messages to each topic that corresponds to a message hashtags. The bot ignores messages in the following cases:

1. A text message content consists only of hashtags (while it's a valid case for photos or document captions);
1. Hashtags are shorter than 4 symbols including the hash symbol, like `#1` or `#hh`.

In case a message contains text content and hashtags but these hashtags don't correspond to any topics in the target group, the bot forwards this message to the *Miscellaneous* topic.

For more information on this process, see [Hashtags and Topics](#hashtags-and-topics).

### Manual Forwarding

Sometimes you might want to save a message that initially didn't have any hashtags, or these hashtags were added later when the author edited the message. Then you can send the `/save` command as a reply to the message you want to forward to the target chat.

You can call the `/save` command either without arguments or pass any number of space-separated hashtags. Let's see these two cases in more detail:

* You see a message that contains one or more hashtags, and you want to save this message. To forward the message, just reply to it with the `/save` command, and the bot will send it to all corresponding topics based on the hashtags in the message.

* You see a message that is of interest but doesn't contain any hashtags. In this case, you can reply to this message with the `/save` command with a sequence of space-separated hashtags as follows: `/save #knowledge #text #tools`. The bot will forward the message to which you reply to all corresponding topics, once to each topic.

  When you use this method, the bot ignores the message content and selects target topics based only on hashtags passed as arguments. See the full list of available hashtags in the [Hashtags and Topics](#hashtags-and-topics) section.

> [!NOTE]
> The bot allows forwarding text messages, photos, documents, videos, and audio files. The poll forwarding is partially supported - you can only save polls via the `/save` command with arguments. All other message types such as stories, giveaways, stickers, and others, are not supported.

### Hashtags and Topics

The bot tries to map the hashtags received to the values from a from a [predefined dictionary](./hashtag_map.py). This dictionary contains hashtags and corresponding topic IDs. Hashtags are unique and serve as keys while the topic IDs can repeat so that a single topic can contain messages having several different hashtags.

You don't need topic IDs until you want to contribute to the above mentioned dictionary. The bot accepts only hashtags as commands.

If a message has one or more hashtags that are not listed in the table below, and has no messages from the table, the bot sends such message to the *Miscellaneous* topic whose ID is `30`.

The following table displays available hashtags and topics (in Russian):

| Hashtag            |    Topic   | ID  |
|--------------------|------------|-----|
| #вакансия          | Вакансии и стажировки |   289  |
| #стажировка        | Вакансии и стажировки |   289  |
| #подработка        | Вакансии и стажировки |   289  |
| #резюме            |   Резюме   | 290 |
| #карьера           | Карьерные вопросы | 288 |
| #наставник         | Карьерные вопросы | 288 |
| #знания            | Знания и пр. |  6  |
| #инструменты       | Знания и пр. |  6  |
| #практики          | Знания и пр. |  6  |
| #стандарты         | Знания и пр. |  6  |
| #текст             |   Тексты   |  7  |
| #шаблон            |   Тексты   |  7  |
| #сделано           | Сообщество | 283 |
| #идея              | Сообщество | 283 |
| #флешмоб           | Сообщество | 283 |
| #батл              | Сообщество | 283 |
| #мем               |    Мемы    | 284 |
| #мемы              |    Мемы    | 284 |
| #мероприятие       | Мероприятия | 372 |
| #чат               |    _adm    | 485 |
| #админское         |    _adm    | 485 |
