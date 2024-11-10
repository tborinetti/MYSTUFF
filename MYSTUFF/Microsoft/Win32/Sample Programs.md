#c #win32 #basics #applications 

## Each main function takes the following arguments:
##### Handle to the programs .exe file in memory:
```c
HINSTANCE hInstance
```
*Can be used to load resources or other tasks performed on a per-module basis - usually either an EXE or DLL. *

##### Handle the previous instance of a program in Win16 (Always NULL in Win32):
```c
HINSTANCE hPrevInstance
```
*Legacy*

##### Command line args as a string (excluding program name):
```c
LPSTR lpCmdLine
```

##### Int dictating how the window is shown (minimized, maximized, etc.):
```c
int nCmdShow
```
*Legacy*

--- 
# Simple Win32 Program (Alert box):

```C
#include <windows.h>

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, 
    LPSTR lpCmdLine, int nCmdShow)
{
    MessageBox(NULL, "Goodbye, cruel world!", "Note", MB_OK);
    return 0;
}
```
*Note: whether this runs depends on the compiler. 
On Visual Studio in a 'Console App' project, this will spit at compile time due to:* 
`Unresolved symbol main referenced in function invoke_main()`
*This can be fixed by changing the Linker -> Subsystem to Windows*

---
# Simple Win32 window
##### Full code below:
```c
#include <windows.h>

const char g_szClassName[] = "myWindowClass";

// Step 4: the Window Procedure
LRESULT CALLBACK WndProc(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)
{
    switch(msg)
    {
        case WM_CLOSE:
            DestroyWindow(hwnd);
        break;
        case WM_DESTROY:
            PostQuitMessage(0);
        break;
        default:
            return DefWindowProc(hwnd, msg, wParam, lParam);
    }
    return 0;
}

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance,
    LPSTR lpCmdLine, int nCmdShow)
{
    WNDCLASSEX wc;
    HWND hwnd;
    MSG Msg;

    //Step 1: Registering the Window Class
    wc.cbSize        = sizeof(WNDCLASSEX);
    wc.style         = 0;
    wc.lpfnWndProc   = WndProc;
    wc.cbClsExtra    = 0;
    wc.cbWndExtra    = 0;
    wc.hInstance     = hInstance;
    wc.hIcon         = LoadIcon(NULL, IDI_APPLICATION);
    wc.hCursor       = LoadCursor(NULL, IDC_ARROW);
    wc.hbrBackground = (HBRUSH)(COLOR_WINDOW+1);
    wc.lpszMenuName  = NULL;
    wc.lpszClassName = g_szClassName;
    wc.hIconSm       = LoadIcon(NULL, IDI_APPLICATION);

    if(!RegisterClassEx(&wc))
    {
        MessageBox(NULL, "Window Registration Failed!", "Error!",
            MB_ICONEXCLAMATION | MB_OK);
        return 0;
    }

    // Step 2: Creating the Window
    hwnd = CreateWindowEx(
        WS_EX_CLIENTEDGE,
        g_szClassName,
        "The title of my window",
        WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, CW_USEDEFAULT, 240, 120,
        NULL, NULL, hInstance, NULL);

    if(hwnd == NULL)
    {
        MessageBox(NULL, "Window Creation Failed!", "Error!",
            MB_ICONEXCLAMATION | MB_OK);
        return 0;
    }

    ShowWindow(hwnd, nCmdShow);
    UpdateWindow(hwnd);

    // Step 3: The Message Loop
    while(GetMessage(&Msg, NULL, 0, 0) > 0)
    {
        TranslateMessage(&Msg);
        DispatchMessage(&Msg);
    }
    return Msg.wParam;
}
```

---
## Breakdown:
### Step 1. Registering Window class
*Note: Window Classes are not related to C++/Object Oriented classes*

Window classes are registered in WinMain and can be used to create windows without respecifying attributes. 

##### Window Class name:
```c 
const char g_szClassName[] = "myWindowClass"
```

##### Window Class information:
```c
WNDCLASSEX wc;
```

##### Window Class attributes:
```c
wc.cbSize        = sizeof(WNDCLASSEX);                              // Struct size
wc.style         = 0;                          // Class styles -- != Window styles
wc.lpfnWndProc   = WndProc;             // Pointer to window procedure for this WC
wc.cbClsExtra    = 0;             // Amount of extra data for this class in memory
wc.cbWndExtra    = 0;                 // Amount of extra data per window in memory
wc.hInstance     = hInstance;      // Handle to App. Instance - 1st arg of WinMain
wc.hIcon         = LoadIcon(NULL, IDI_APPLICATION);      // Icon seen when Alt+Tab
wc.hCursor       = LoadCursor(NULL, IDC_ARROW);          // Cursor to be displayed
wc.hbrBackground = (HBRUSH)(COLOR_WINDOW+1);       // Bg brush to color the window
wc.lpszMenuName  = NULL;            // Name of menu resource for this WC instances
wc.lpszClassName = g_szClassName;                   // Name to identify this class
wc.hIconSm       = LoadIcon(NULL, IDI_APPLICATION);        // Icon seen in taskbar

if(!RegisterClassEx(&wc))
{
	MessageBox(NULL, "Window Registration Failed!", "Error!",
		MB_ICONEXCLAMATION | MB_OK);
	return 0;
}
```

***REMEMBER!!** 
C functions are often interacting with pointers (references) to objects in memory, so we enclose function calls within an if statement to check the return value for errors. 
This function call doesn't look like it does much, but it is registering the window class AND handling errors. See both [[Data Types]] and [[Pointers]]

--- 
### Step 2. Creating the window

After a Window Class is registered, a window can be created. 

##### Handle to new window:
```c
HWND hwnd;
```
##### Creating a new window:
```c
hwnd = CreateWindowEx(
	WS_EX_CLIENTEDGE,            // Extended window style of window being created
	g_szClassName,                                                     // WC name
	"The title of my window",                  // Name that goes on the title bar
	WS_OVERLAPPEDWINDOW,                                // Window style parameter 
	CW_USEDEFAULT,                    // X Coordinates for window top left corner
	CW_USEDEFAULT,                    // Y Coordinates for window top left corner
	240,                                                       // Width of window
	120,                                                      // Height of window
	NULL,                                              // Handle to parent window
	NULL,                                                     // Handle to a menu 
	hInstance,     // Handle to 'instance of module to be associated' = first arg
	NULL                                       // Pointer to window creation data 
);

if(hwnd == NULL)
{
	MessageBox(NULL, "Window Creation Failed!", "Error!",
		MB_ICONEXCLAMATION | MB_OK);
	return 0;
}
```
##### After creation, window is drawn:
```c
ShowWindow(hwnd, nCmdShow); 
UpdateWindow(hwnd);
```
*The second parameter in ShowWindow is optional - `SW_SHOWNORMAL` can be used as a default.*

### Step 3: The Message Loop
The "heart" of the program, everything that happens is passed through this statement:

```c
while(GetMessage(&Msg, NULL, 0, 0) > 0)
{
	TranslateMessage(&Msg);
	DispatchMessage(&Msg);
}
return Msg.wParam;
```
###### `GetMessage()` 
Gets a message from the applications message queue. Any mouse movements, keyboard presses, menu clicks, are noted by the system and entered into the program's message queue. 
Calling GetMessage requests the next available message be removed from the queue and returned for processing. This continues indefinitely, and waits if nothing is in the queue. 
###### `TranslateMessage()`
Translates virtual-key messages into character messages.  Will generate a `WM_CHAR` message to go alongside a `WM_KEYDOWN` message. 
###### `DispatchMessage()`
Sends the message to the window it was sent to. The system takes care of this. 

### Step 4: The Window Procedure
The "brain" of the program, this is where messages are processed. 

```c
LRESULT CALLBACK WndProc(
	HWND hwnd, // Handle of the window the message originates from	
	UINT msg,  // Message to be processed
	WPARAM wParam, 
	LPARAM lParam
)
{
    switch(msg)
    {
        case WM_CLOSE: // When user presses X / Close button or ALT-F4
            DestroyWindow(hwnd); // Sends WM_DESTROY message - see below
        break;
        case WM_DESTROY: // Recieved from above case
            PostQuitMessage(0); // Sends WM_QUIT message - closing applciation
        break;
        default:
            return DefWindowProc(hwnd, msg, wParam, lParam);
    }
    return 0;
}
```

The application breaks before fully receiving the WM_QUIT message, as this makes 
`GetMessage() == 0` which breaks the application/message loop. 
