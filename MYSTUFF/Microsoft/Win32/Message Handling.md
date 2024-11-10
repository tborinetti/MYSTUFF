#c #basics #applications #win32 
```c
LRESULT CALLBACK WndProc(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)
{
    switch(msg)
    {
		// case added here
			// with desired instructions here
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
```

To add handling for mouse clicks, a switch case is added with the property `WM_LBUTTONDOWN`
(or relevant handler).

##### Example:
###### Output the program's file path on Left-Mouse click:
```c
LRESULT CALLBACK WndProc(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)
{
    switch(msg)
    {
        case WM_LBUTTONDOWN:
        {
            char szFileName[MAX_PATH];
            HINSTANCE hInstance = GetModuleHandle(NULL);

            GetModuleFileName(hInstance, szFileName, MAX_PATH);
            MessageBox(hwnd, 
			           szFileName, 
			           "This program is:", 
			           MB_OK | MB_ICONINFORMATION
			);
        }
        break;
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
```

Since `GetModuleFileName()` requires a `HINSTANCE` variable referring to the .exe, `GetModuleHandle(NULL)` can be used. The `NULL` parameter specifies the return value be "a handle to the file used to create the calling process (.exe file)" [See Docs](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-getmodulehandlea)

The second parameter for `GetModuleFileName()` refers to a "pointer to a buffer that receives the path and file name of the specified module", the datatype being [[Data Types#`TCHAR` - CHAR with UNICODE support |LPTSTR]]. `MAX_PATH` is a macro defined by `<windows.h>` and is the max length of a buffer needed to store a filename in Win32/ 


