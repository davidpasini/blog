---
layout: post
title: A Telegram bot for iBridgePy errors (..and more)
excerpt_separator: <!--more-->
---
{% if page.title == "Home" %}
  ![Telegram bot](../images/telegram_bot.jpg)
{% else %}
  ![telegram bot](/images/telegram_bot.jpg)
{% endif %}

Ok, so this is still in it's rough form, and it might not be the perfect toy yet but it surely helps to keep your mind at easy when you're away from your pc/server.


As I begin to forward-test my algos with Interactive Brokers' Trader WorkStation TWS (I cannot use IB Gateway because it does not provide Delayed Market Data, or real time market data, unless you deposit a minimum of 2000 euro on your IB account), I am in constant fear, even with my monopoly money, that for whatever reason my algo misbehaves or the IB server becomes unhappy, leaving me with an interrupted algo and jibberish error messages that sit there unattended while I am away travelling the world. Sure, Teamviewer, VNC connect, an ssh connection and restarting your machine on power-on with a smart plug helps you a great deal but a more timely solution would be to receive notifications when something goes bad.


> This technique can also be applied to receiving whatever print out from your algo and iBridgePy, I am yet to discover how! :stuck_out_tongue_closed_eyes:
<!--more-->


Below the steps to get you started and that will catch RuntimeError (IB servers not responding for some reason / Unknown reasons) as well as for catching errorMessage from 'def error(self, errorId, errorCode, errorMessage)'


---


### Step 1
Create a Telegram bot as per this tutorial [here](https://www.youtube.com/watch?v=M9IGRWFX_1w).


### Step 2
Insert the following code snippets in these 3 locations:

> ../broker_client_factory/callbacks.py line 125

```python
import requests
def send_msg(text):
    token = "insert_here_your_token"
    chat_id = "insert_here_your_chat_id"
    url_req = "https://api.telegram.org/bot"+ token +"/sendMessage" + "?chat_id=" + chat_id + "&text=" + text
    results = requests.get(url_req)
    return results.json()
if errorMessage != 'Already connected.':
    send_msg(errorMessage)
```
I Ifed out the message 'Already connected.' as it is sent back from the server every such seconds.


> ../broker_client_factory/BrokerClient.py line 290 and line 291, just above and inline with the 2 `raise RuntimeError`

```python
import requests
def send_msg(text):
    token = "1445443253:AAGHDU2b4puOnKweaOOpgxLKEDx4jQEOh6A"
    chat_id = "1447072292"
    url_req = "https://api.telegram.org/bot"+ token +"/sendMessage" + "?chat_id=" + chat_id + "&text=" + text
    results = requests.get(url_req)
    return results.json()
send_msg('Server did not respond. Script terminated.)
```
You can change the message inside send_msg() by compying and pasting the 2 RuntimeError messages like this:
* send_msg('IB historical server did not response on the historical data request at this moment. Consider to try it later.')
* send_msg('Unknown reasons')


### DONE!!
You can now relax that you will be notified on Telegram each time these error messages are raised. I will update this guide whenever I encounter other errors of this type.


Surely this is not the most elegant way of injecting code into iBridgePy and also it is not so convenient to go back and edit all the locations eachtime you update iBridgePy (mandatory for non-premium users) but for now it works. Should you have a more elegant solution, please dm or submit a pull request for [NVDA_EMAs_crossover](https://github.com/davidpasini/NVDA_EMAs_crossover).


Next step (but not in a hurry): let the bot send to telegram all the print out from the iPython console :metal: :+1: :muscle:
