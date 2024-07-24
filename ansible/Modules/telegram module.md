# telegram module 

for sending notifications via telegram

## Synopsis

Send notifications via telegram bot, to a verified group or user

## Parameters

| Parameter                                                    | Choices/Defaults                                             | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ | :----------------------------------------------------------- |
| **chat_id**                                                    -                                             / required | ****                                                         | Telegram group or user chat_id                               |
| **msg**                                                    -                                         / required |                                                              | What message you wish to send.                               |
| **msg_format**                                                    -                                                                                added in 2.4 | **Choices:**                                                                                                                                                                                            **plain** ←                                                                                                                                                                                                                            markdown                                                                                                                                                                                                                            html | Message format. Formatting  options `markdown` and `html` described in Telegram API docs  (https://core.telegram.org/bots/api#formatting-options). If option  `plain` set, message will not be formatted. |
| **token**                                                    -                                             / required |                                                              | Token identifying your telegram bot.                         |



## Notes

You will require a telegram account and create telegram bot to use this module.

## Example

```ini
name: send a message to chat in playbook
telegram:
  token: '814363734:AAHVcl4uAIgzhyU0sHlCcoi9THd2lCOPYOY'
  chat_id: '233306866'
  msg: "Ansible task finished http://google.com"
  msg_format: "markdown"
environment:
  https_proxy: "https:/Address.com:65515"
tags: teler2
```