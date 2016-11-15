```

import ujson as json

import requests





import socks

import socket





def query():

    # 代理

    socks.set_default_proxy(socks.SOCKS5, "localhost", 1080)

    socket.socket = socks.socksocket







    data = {

        "universe":self.universe,

        "key":self.key,

        "rcpts":self.rcpts,

        "message":self.message

    }







    url = "https://api.spl4cn.com/api/trigger/nph-6.pl"







    try:

        Session = requests.session()

        response = Session.post(url, data)

    except Exception as e:

        print(str(e))



    else:

        result = response.json()

        print(result)

    finally:

        Session.close()





    return result

```
