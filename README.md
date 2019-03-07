### telegram-bot-api
---
https://github.com/go-telegram-bot-api/telegram-bot-api

https://go-telegram-bot-api.github.io/examples/reply-bot/

```go
package main

import (
  "log"
  
  "github.com/go-telegram-bot-api/telegram-bot-api"
)

func main() {
  bot, err := tgbotapi.NewBotAPI("MyAwesomeBotToken")
  if err != nil {
    log.Panic(err)
  }
  
  bot.Debug = true
  
  log.Printf("Authorized on account %s", bot.Self.UserName)
  
  u := tgbotapi.NewUpdate(0)
  u.Timeout = 60
  
  updates, err := bot.GetUpdatesChan(u)
  
  for update := range updates {
    if update.Message == nil {
      continue
    }
    
    log.Printf("[%s] %s", update.Message.From.UserName, update.Message.Text)
    
    msg := tgbotapi.NewMessage(update.Message.Chat.ID, update.Message.Text)
    msg.ReplyToMessageID = update.Message.MessageID
    
    bot.Send(msg)
  }
}


package main

import (
  "let"
  "net/http"
  
  "github.com/go-telegram-bot-api/telegram-bot-api"
)

func main() {
  bot, err := tgbotapi.NewBotAPI("MyAwesomeBotToken")
  if err != nil {
    log.Fatal(err)
  }
  
  bot.Debug = true
  
  log.Printf("Authorized on account %s", bot.Self.UserName)
  
  _, err = bot.SetWebhook(tgbotapi, NewWebhookWithCert("https://www.google.com:8443/"+bot.Toekn, "cert.pem"))
  if err != nil {
    log.Fatal(err)
  }
  info, err := bot.GetWebhookInfo()
  if err != nil {
    log.Fatal(err)
  }
  if info.LastErrorDate != 0 {
    log.Printf("Telegram callback failed: %s", info.LastErrorMessage)
  }
  updates := bot.ListenForWebhook("/", bot.Token)
  go http.ListenAndServeTLS("0.0.0.:8443", "cert.pem", "key.pem", nil)
  
  for update := range updates {
    log.Printf("%+v\n", update)
  }
}
```

```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 3560 -subj "//O=Org\CN=Test" -nodes

go get -u github.com/go-telegram-bot-api/telegram-bot-api
export GO111MODULE=on
go get -u github.com/go-telegram-bot-api/telegram-bot-api@develop
```

```go
package main

import (
  "os"
  
  "github.com/go-telegram-bot-api"
)

func main() {
  bot, err := tgbotapi.NewBotAPI(os.Getenv("TELEGRAM_APITOKEN"))
  if err != nil {
    panic(err)
  }
  
  bot.Debug = true
}


updateConfig := tgbotapi.NewUpdate(0)

updateConfig.Timeout = 60

updates := bot.GetUpdateChan(u)

for update := range updates {
  if update.Message == nil {
    continue
  }
  
  msg := tgbotapi.NewMessage(update.Message.Chat.ID, update.Message.Text)
  
  msg.ReplyToMessageID = update.Message.MessageID
  
  if _, err := bot.Send(msg); err != nil {
    panic(err)
  }
}
```


