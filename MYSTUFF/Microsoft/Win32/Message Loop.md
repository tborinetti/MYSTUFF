#c #basics #win32 #applications 

## Sending messages
Every windows message can have up to two parameters: `wParam` and `lParam`. Both are 32-bit. 
#### To send messages, the following can be used:
###### `PostMessage()`
- Puts the message into the Message Queue and returns immediately
- Once a call to this function is made, the message may or may not have been processed yet. 

###### `SendMessage()`
- Sends the message directly to the window 
- Does not return until the window has finished processing it. 

Not all messages require the `wParam` and `lParam` values. `WM_CLOSE` is an example of this, which is called as following:

```c
PostMessage(hwnd, WM_CLOSE, 0, 0);
```

---
## Message Queue 
If a message is currently being handled, and a user inputs a sequence of keyboard presses/mouse movements, these messages are put into the queue and wait for the current process to finish. 

```c 
while(GetMessage(&Msg, NULL, 0, 0) > 0)
{
    TranslateMessage(&Msg);
    DispatchMessage(&Msg);
}
```

1. The message loop runs continuously, checking the message queue for an event. 

2. When an event occurs, it is added to the queue. `GetMessage()` returns a **positive integer** for a message to be processed, returns **zero** if `WM_QUIT` is found, and a **negative integer** if an error occurs.

3. If `GetMessage()` returns a positive integer, `TranslateMessage()` is called on the current message. This does additional processing, translating virtual key messages into character messages. This is not always required, but **will break certain things if not present.** 

4. After translation, the message is passed to `DispatchMessage()`, which takes the message, checks it's destination window, and looks up the relevant `WinProc()`. This procedure is called, sending the window handle, the message, `wParam`, and `lParam`.

5. In window procedures, the message and its parameters are checked before execution of user code.  If the message isn't being specifically handled, `DefWinProc()` calls the default actions (often nothing).

6. Once the processing is complete, the `WinProc()` returns, `DispatchMessage()` returns, and the loop begins again