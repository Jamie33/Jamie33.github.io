
---

title: How to Scrape All ETH Addresses from A Certain Telegram Chat
date:  2018.5.7
categories:  Spider
tags:  Telegram
keywords:  Telegram

---

## First attempt: Telethon (doesn't work)

Telegram is a popular messaging application. This library is meant to make it easy for you to write Python programs that can interact with Telegram. 


```python
#Creating a client

from telethon import TelegramClient
import socks

api_id = 240869
api_hash= '2e5e8cb9cda41113da881a986271029d'

client = TelegramClient('sqlite3',api_id,api_hash,proxy=(socks.SOCKS5,'localhost',1080))
client.connect()
phone_number = '8613480975957'
client.send_code_request(phone_number)
myself = client.sign_in(phone_number,input('Enter code:'))
```

    Enter code:5603960942
    
<!-- more -->

```python
# Doing stuff

Naus = client.get_entity('t.me/NausGlobal')
print(Naus)
```

    Channel(id=1249144664, title='Naus.io_Global', photo=ChatPhoto(photo_small=FileLocation(dc_id=5, volume_id=852712166, local_id=188596, secret=6383482124705140361), photo_big=FileLocation(dc_id=5, volume_id=852712166, local_id=188598, secret=7217383776991144713)), date=datetime.utcfromtimestamp(1523907796), version=0, creator=False, left=False, editor=False, broadcast=False, verified=False, megagroup=True, restricted=False, democracy=True, signatures=False, min=False, access_hash=-9112681265137568112, username='NausGlobal', restriction_reason=None, admin_rights=None, banned_rights=None, participants_count=None)
    


```python
#!/usr/bin/python3
# -*- coding:utf8 -*-
import codecs
import re

address = r'(0x[a-z0-9A-Z]{40})'
name = r'\[@([a-z0-9A-Z]+)\]'
date = r'\d{2}\.\d{2}\.\d{4} \d{2}\:\d{2}\:\d{2}'
ID = r', (.+) \['

with open(r'telegram.txt','r',encoding="utf8") as f:
    for line in f.readlines():
        print(line)
        if re.search(ID,line):
            #username = re.search(name,line)
            time = re.search(date,line)
            Telid = re.search(ID,line)
            print('---------------------------')
            #print(username.group(1))
            print(time.group())
            print(Telid.group(1))
            print('---------------------------')
        
```

    ﻿.....11.03.2018 16:15:50, carl cui [@cuiyysw]: >>upgraded the group to a supergroup<<
    
    ---------------------------
    11.03.2018 16:15:50
    carl cui
    ---------------------------
    .....11.03.2018 16:18:02, carl cui [@cuiyysw]: >>changed group name to "Naus.io_Global"<<
    
    ---------------------------
    11.03.2018 16:18:02
    carl cui
    ---------------------------
    .....11.03.2018 23:29:14, carl cui [@cuiyysw]: >>invited 小魔女 灵儿 [@loloplay]<<
    
    ---------------------------
    11.03.2018 23:29:14
    carl cui [@cuiyysw]: >>invited 小魔女 灵儿
    ---------------------------
    .....12.03.2018 21:58:00, Den: >>joined the group<<
    
    .....12.03.2018 21:58:49, Федя [@Ovod13]: >>joined the group<<
    
    ---------------------------
    12.03.2018 21:58:49
    Федя
    ---------------------------
    12.03.2018 21:59:54, Den: Арим
    
    .....12.03.2018 22:00:30, Den: >>left the group<<
    
    .....12.03.2018 22:01:07, Nata [@Nataly9900]: >>joined the group<<
    
    ---------------------------
    12.03.2018 22:01:07
    Nata
    ---------------------------
    .....12.03.2018 22:01:16, Sergey Lizunov [@strahara]: >>joined the group<<
    
    ---------------------------
    12.03.2018 22:01:16
    Sergey Lizunov
    ---------------------------
    12.03.2018 22:02:03, Sergey Lizunov [@strahara]: 0xfa7c4fe34cEA9289f97bED26f7106AcB3523001F
    
    ---------------------------
    12.03.2018 22:02:03
    Sergey Lizunov
    ---------------------------
    .....12.03.2018 22:03:47, Van Pham [@vanphamn]: >>joined the group<<
    
    ---------------------------
    12.03.2018 22:03:47
    Van Pham
    ---------------------------
    12.03.2018 22:03:52, Van Pham [@vanphamn]: 0x4907d01FAD88F4c5F4fF0c6877328b9234FBcef5
    
    ---------------------------
    12.03.2018 22:03:52
    Van Pham
    ---------------------------
    .....12.03.2018 22:05:44, Денис Назаров [@Astroden]: >>joined the group<<
    
    ---------------------------
    12.03.2018 22:05:44
    Денис Назаров
    ---------------------------
    .....12.03.2018 22:05:54, SAJIB MAX [@sajibmax113]: >>joined the group<<
    
    ---------------------------
    12.03.2018 22:05:54
    SAJIB MAX
    ---------------------------
    .....12.03.2018 22:06:00, Jean Nguessan [@Jeanlenoir]: >>joined the group<<
    
    ---------------------------
    12.03.2018 22:06:00
    Jean Nguessan
    ---------------------------
    .....12.03.2018 22:06:32, vu khuong: >>joined the group<<
    
    12.03.2018 22:06:41, vu khuong: 0x2c3c4ED46A54f3107CFDfC9CBf95C4dFAc3487E2
    
    .....12.03.2018 22:07:47, Степан Рязанов [@m171pe]: >>joined the group<<
    
    ---------------------------
    12.03.2018 22:07:47
    Степан Рязанов
    ---------------------------
    12.03.2018 22:07:50, 小魔女 灵儿 [@loloplay]: HI ALL，Thank you for your participation. Our event has just ended. If you are interested in our next NAB token reward, please join our Telegram group or WeChat group for more information. （
    
    ---------------------------
    12.03.2018 22:07:50
    小魔女 灵儿
    ---------------------------
    .....12.03.2018 22:08:23, 小魔女 灵儿 [@loloplay]: >>unsupported service message type: messageActionPinMessage<<
    
    ---------------------------
    12.03.2018 22:08:23
    小魔女 灵儿
    ---------------------------
    .....12.03.2018 22:08:38, Sergey Lizunov [@strahara]: >>left the group<<
    
    ---------------------------
    12.03.2018 22:08:38
    Sergey Lizunov
    ---------------------------
    .....12.03.2018 22:11:02, Andrey Mishin [@Zoar1975]: >>joined the group<<
    
    ---------------------------
    12.03.2018 22:11:02
    Andrey Mishin
    ---------------------------
    12.03.2018 22:11:18, Andrey Mishin [@Zoar1975]: 0xb72bf806bf1b11d955f84a0871f158322987e3c8
    
    ---------------------------
    12.03.2018 22:11:18
    Andrey Mishin
    ---------------------------
    .....12.03.2018 22:11:20, raju malakomda [@malakomda]: >>joined the group<<
    
    ---------------------------
    12.03.2018 22:11:20
    raju malakomda
    ---------------------------
    12.03.2018 22:11:21, SAJIB MAX [@sajibmax113]: thanks
    ---------------------------
    12.03.2018 22:11:21
    SAJIB MAX
    ---------------------------
    

## Second Attempt: Chrome extension to save telegram chat history (works well)

Dumps your Telegram chat history with an active peer to plaintext.
Perhaps, many of us like Telegram messenger and use it for conversations with friends. However, we were a bit disappointed by the absence of any sort of easy-accessible "Save Conversation" feature in the messenger. Therefore, we've written a Chrome extension for it! More info at https://github.com/pigpagnet/save-telegram-chat-history.

https://chrome.google.com/webstore/detail/save-telegram-chat-histor/kgldnbjoeinkkgpphaaljlfhfdbndfjc?utm_source=www.crx4chrome.com


```python
# test to get name

import re
text = r'12.03.2018 22:03:52, Van Pham [@vanphamn]: 0x4907d01FAD88F4c5F4fF0c6877328b9234FBcef5'
name = r'\[@([a-z0-9A-Z]+)\]'
print(re.search(name,text).group(1))
```

    vanphamn
    


```python
# test to get Eth address

import re
text = '''Your Telegram History in "Naus.io_Global [@NausGlobal]"

.....11.03.2018 16:15:50, carl cui [@cuiyysw]: >>upgraded the group to a supergroup<<
.....11.03.2018 16:18:02, carl cui [@cuiyysw]: >>changed group name to "Naus.io_Global"<<
.....11.03.2018 23:29:14, carl cui [@cuiyysw]: >>invited 小魔女 灵儿 [@loloplay]<<
.....12.03.2018 21:58:00, Den: >>joined the group<<
.....12.03.2018 21:58:49, Федя [@Ovod13]: >>joined the group<<
12.03.2018 21:59:54, Den: Арим
.....12.03.2018 22:00:30, Den: >>left the group<<
.....12.03.2018 22:01:07, Nata [@Nataly9900]: >>joined the group<<
.....12.03.2018 22:01:16, Sergey Lizunov [@strahara]: >>joined the group<<
12.03.2018 22:02:03, Sergey Lizunov [@strahara]: 0xfa7c4fe34cEA9289f97bED26f7106AcB3523001F
.....12.03.2018 22:03:47, Van Pham [@vanphamn]: >>joined the group<<
'''
content = text.split('\n')
address = r'(0x[a-z0-9A-Z]{40})'

for i in content:
    if re.search(address,i):
            eth = re.search(address,i)
            print(eth)
```

    <_sre.SRE_Match object; span=(49, 91), match='0xfa7c4fe34cEA9289f97bED26f7106AcB3523001F'>
    


```python
# first try

#!/usr/bin/python3
# -*- coding:utf8 -*-
import codecs
import re

address = r'(0x[a-z0-9A-Z]{40})'
name = r'\[@([a-z0-9A-Z]+)\]'
date = r'\d{2}\.\d{2}\.\d{4} \d{2}\:\d{2}\:\d{2}'
ID = r', (.+) \['

with open(r'telegram_group_history__Naus.io_Global [@NausGlobal]_from20180311-1615_to20180504-1504__downloaded2018_05_04--18-22-55.txt','r',encoding='utf-8') as f:
    with open('telegram20180504.csv','w',encoding='utf-8') as w: 
        for line in f.readlines():
            if re.search(address,line) and re.search(name,line):
                eth = re.search(address,line).group()
                username = re.search(name,line).group(1)
                time = re.search(date,line).group()
                telid = re.search(ID,line).group(1)
                w.write('{},{},{},{}\n'.format(time,telid,username,eth))
print('The End')            
```

    The End
    


```python
# first try has bug in name, it will not get chat of users who have set usernames

import re
n1 ='02.05.2018 15:07:00, wu long: 0xb1A45043Ed0aDABc6e2F658BaE0eb8EA4d311294'
n2 ='.....02.05.2018 15:08:07, Yasin Kartal [@yasinnkrtl]: >>joined the group<<'
name = r',(.+):'
print(re.search(name,n1).group(1).strip())
print(re.search(name,n2).group(1).strip())
```

    wu long
    Yasin Kartal [@yasinnkrtl]
    


```python
# second try

#!/usr/bin/python3
# -*- coding:utf8 -*-
import codecs
import re

address = r'(0x[a-z0-9A-Z]{40})'
name =  r',(.+):'
date = r'\d{2}\.\d{2}\.\d{4} \d{2}\:\d{2}\:\d{2}'


with open(r'telegram_group_history__Naus.io_Global [@NausGlobal]_from20180311-1615_to20180504-1504__downloaded2018_05_04--18-22-55.txt','r',encoding='utf-8') as f:
    with open('telegram20180505.csv','w',encoding='utf-8') as w: 
        for line in f.readlines():
            if re.search(address,line)and re.search(name,line):
                eth = re.search(address,line).group()
                username = re.search(name,line).group(1)
                time = re.search(date,line).group()
                w.write('{},{},{}\n'.format(time,username,eth))
print('The End')   
```

    The End
    
