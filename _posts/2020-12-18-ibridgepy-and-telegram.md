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


As I begin to forward-test my algos with Interactive Brokers' Trader WorkStation TWS (I cannot use IB Gateway because it does not provide Delayed Market Data, or real time market data, unless you deposit a minimum of 2000 euro on your IB account), I am in constant fear, even with my monopoly money, that for whatever reason my algo misbehaves or the IB server becomes unhappy, leaving me with an interrupted algo and jibberish error messages that sit there unattended while I am away travelling the world. Sure, Teamviewer, xxx, an ssh connection and restarting your machine on power on help you to a great deal, but a more timely solution would be to receive notifications when something goes bad.


> This technique can also be applied to receiving whatever print out from your algo and iBridgePy, I am yet to discover how! :stuck_out_tongue_closed_eyes:
<!--more-->


Below the steps to get you started and that will catch RuntimeError (IB servers not responding for some reason / Unknown reasons) as well as for catching errorMessage from 'def error(self, errorId, errorCode, errorMessage)'


---


### Step 1
Create a Telegram bot as per this tutorial [here](https://www.youtube.com/watch?v=M9IGRWFX_1w).

