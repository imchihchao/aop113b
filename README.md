# Application of Programming

[Repos](github_repos.md)

## 00 Introduction

[00 Introduction Slides](https://htmlpreview.github.io/?https://github.com/imchihchao/aop113b/blob/main/slides/00-Introduction.html)

[00 Notebook](nb00.ipynb)

## 01 Python Overview

[01 Python Overview Slides](https://htmlpreview.github.io/?https://github.com/imchihchao/aop113b/blob/main/slides/01-python_overview.html)

[01 Notebook](nb01.ipynb)

- [EX01-01 加法器](exercises/ex01-01.png)
- [EX01-02 BMI 計算機](exercises/ex01-02.png)
- [EX01-03 Rock-Paper-Scissors](exercises/ex01-03.png)
- [EX01-04 終極密碼](exercises/ex01-04.png)
- [EX01-05 Caesar Cipher: Encode and decode](exercises/ex01-05.png)
- [EX01-06 簡易購物車](exercises/ex01-06.png)
- [EX01-07 美食資訊查詢](exercises/ex01-07.png) / [同學分享美食名單](foods.md)

## 02 Web Crawler

[02 Web Crawler Slides](https://htmlpreview.github.io/?https://github.com/imchihchao/aop113b/blob/main/slides/02-web_crawler.html)

[02 Notebook](nb02.ipynb)

- [EX02-01 My Chatbot](exercises/ex02-01.png)
- [EX02-02 高雄紅橘線捷運車站位置查詢](exercises/ex02-02.png)
- [EX02-03 Yahoo 新聞儲存 Google 試算表](exercises/ex02-03.png)
- [EX02-04 Download Pokemon Images](exercises/ex02-04.png)
- [EX02-05 PTT Gossiping](exercises/ex02-05.png)
- [EX02-06 開眼電影](exercises/ex02-06.png)
- [EX02-07 KKDay](exercises/ex02-07.png)

## 03 Data Analysis

[03 Data Analysis Slides](https://htmlpreview.github.io/?https://github.com/imchihchao/aop113b/blob/main/slides/03-data_analysis.html)

[03 Notebook](nb03.ipynb)

- [EX03-01 公司薪資概況Ⅰ](exercises/ex03-01.png)
- [EX03-02 公司薪資概況Ⅱ](exercises/ex03-02.png)
- [EX03-03 空氣品質指標(AQI)](exercises/ex03-03.png)
- [EX03-04 薪情平台](exercises/ex03-04.png)
- [EX03-05 台灣股票市場個股每日成交資訊](exercises/ex03-05.png)
- [EX03-06 出生人口數](exercises/ex03-06.png)
- [EX03-07 Tips](exercises/ex03-07.png)

## 05 Web API

[05 Web API Slides](https://htmlpreview.github.io/?https://github.com/imchihchao/aop113b/blob/main/slides/05-web_api.html)

[05 Notebook](nb05.ipynb)

- [EX05-01 Echo Bot](exercises/ex05-01.png)



```python
# for colab
from google.colab import userdata
from pyngrok import ngrok
from flask_ngrok import run_with_ngrok
def ngrok_start():
    ngrok.set_auth_token(userdata.get('NGROK_AUTHTOKEN'))
    ngrok.connect(5000)
    run_with_ngrok(app)


from flask import Flask, request, abort

from linebot.v3 import WebhookHandler
from linebot.v3.exceptions import InvalidSignatureError
from linebot.v3.webhooks import MessageEvent, TextMessageContent
from linebot.v3.messaging import (
    Configuration, ApiClient, MessagingApi,
    ReplyMessageRequest,
    TextMessage
)

app = Flask(__name__)

configuration = Configuration(access_token=userdata.get('LINE_CHANNEL_ACCESS_TOKEN'))
handler = WebhookHandler(userdata.get('LINE_CHANNEL_SECRET'))

@app.route("/callback", methods=['POST'])
def callback():
    signature = request.headers['X-Line-Signature']
    body = request.get_data(as_text=True)
    try:
        handler.handle(body, signature)
    except InvalidSignatureError:
        abort(400)
    return 'OK'


@handler.add(MessageEvent, message=TextMessageContent)
def handle_message(event):
    with ApiClient(configuration) as api_client:
        line_bot_api = MessagingApi(api_client)
        line_bot_api.reply_message(
            ReplyMessageRequest(
                reply_token=event.reply_token,
                messages=[
                    TextMessage(text=event.message.text)
                ]
            )
        )



ngrok_start() # for colab
if __name__ == "__main__":
    app.run()
```






