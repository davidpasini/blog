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

Ok, so this is still in its rough form, and it might not be the perfect toy yet but it surely helps to keep your mind at easy when you're away from your pc/server.


As I begin to forward-test my algos with Interactive Brokers' Trader WorkStation TWS (I cannot use IB Gateway because it does not provide Delayed Market Data, or real time market data, unless you deposit a minimum of 2000 euro on your IB account), I am in constant fear, even with my monopoly money, that for whatever reason my algo misbehaves or the IB server becomes unhappy, leaving me with an interrupted algo and jibberish error messages that sit there unattended while I am away travelling the world. Sure, Teamviewer, VNC connect, an ssh connection and restarting your machine on power-on with a smart plug helps you a great deal but a more timely solution would be to receive notifications when something goes bad.


> This technique can also be applied to receiving whatever print out from your algo and iBridgePy, I am yet to discover how!
<!--more-->


Below the steps to get you started and that will catch RuntimeError (IB servers not responding for some reason / Unknown reasons)


---


### Step 1
Create a Telegram bot as per this tutorial [here](https://www.youtube.com/watch?v=M9IGRWFX_1w).


### Step 2
Create a .py file in the main iBridgePy folder (i called mine my_ibrodgepy_tools.py.

Inside this file copy the following code and save:
```python
import requests

def send_telegram(text):
    token = "<insert_here_your_token>"
    chat_id = "1447072292"
    
    url_req = "https://api.telegram.org/bot"+ token +"/sendMessage" + "?chat_id=" + chat_id + "&text=" + text + "&parse_mode=html"
    results = requests.get(url_req)
    return results.json()
```
> Remember to change `<insert_here_your_token>` and `<insert_here_your_chat_id>` with your own token and chat_id from step 1.


### Step 3
Insert the following code snippets in the following location:

> ../broker_client_factory/BrokerClient.py line 15

```python
from my_ibridgepy_tools import send_telegram
```

> ../broker_client_factory/BrokerClient.py line 280, push`self._log.error` to line 281

```python
send_telegram('Alert! Alert!!')
```
You can change the message inside `send_msg()` by inserting the string that you want to show in Telegram, like this:
* send_msg('IB historical server did not response on the historical data request at this moment. Consider to try it later.')
* send_msg('Unknown reasons')

> As of 2020-12-24 iBridgePy version for Linux, the Runtime Error 'Unknown reasons has benn deprecated. I am not sure why or how but I will address if and when this error will be raised again. So far the wording'Unknown Reasons' is not present in any files.


### DONE!!
You can now relax that you will be notified on Telegram each time these error messages are raised. I will update this guide whenever I encounter other errors of this type.


Surely this is not the most elegant way of injecting code into iBridgePy and also it is not so convenient to go back and edit all the locations eachtime you update iBridgePy (mandatory for non-premium users) but for now it works. Should you have a more elegant solution, please dm or submit a pull request for [NVDA_EMAs_crossover](https://github.com/davidpasini/NVDA_EMAs_crossover).


Next step (but not in a hurry): let the bot send to telegram all the print outs from the iPython console.


Bye for now. d.
