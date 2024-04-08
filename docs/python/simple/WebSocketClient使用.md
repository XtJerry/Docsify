### WebSocket Client使用
```python
import websocket

def get_wss_url(room_id):


def on_message(ws,message):
    print(message)

def on_error(ws,error):
    print(error)

def on_close(ws):
    print("closed")
def on_open(ws):
    print("opend")

websocket.enableTrace(True)
ws=websocket.WebSocketApp("wss://**.com/ws",on_message=on_message,on_error=on_error,on_close=on_close)
ws.on_open=on_open
ws.run_forever()

```