# 自分メモ
## imgui
imgui+win32+openglを使用した方法について記載する。
まずは、demoを動作させる.

## 環境
* Gitからダウンロード
* 下記のファイルを自分の環境に合わせて保存。
    * imstb_truetype.h
    * imconfig.h
    * imgui.cpp
    * imgui.h
    * imgui_internal.h
    * imgui_widgets.cpp
    * imstb_rectpack.h
    * imstb_textedit.h
    * imgui_impl_win32.h
    * imgui_impl_opengl2.cpp
    * imgui_impl_opengl2.h
    * imgui_impl_win32.cpp
    * imgui_draw.cpp
    * imgui_stdlib.h
    * imgui_stdlib.cpp

* プロジェクトを作成
    * コンソールアプリで作成する。
* 文字セット
    *　ユニコード
## 作成
* ウィンドウ
```
void WndClass(WNDCLASSEX &_Wc , HWND &_Hwnd , int _nX, int _nY,int _nW , int _nH)
{
	_Wc.cbSize = sizeof(WNDCLASSEX);
	_Wc.style = CS_CLASSDC;
	_Wc.lpfnWndProc = WndProc;
	_Wc.cbClsExtra = 0;
	_Wc.cbWndExtra = 0;
	_Wc.hInstance = GetModuleHandle(NULL);
	_Wc.hIcon = NULL;
	_Wc.hCursor = NULL;
	_Wc.hbrBackground = NULL;
	_Wc.lpszMenuName = NULL;
	_Wc.lpszClassName = _T("Test");
	_Wc.hIconSm = NULL;

	RegisterClassEx(&_Wc);

	_Hwnd = CreateWindow(_T("Test"), _T("Test"), WS_OVERLAPPEDWINDOW, _nX, _nY , _nW, _nH, NULL, NULL, _Wc.hInstance, NULL);
}
```
* コールバック
```
LRESULT WINAPI WndProc(HWND hWnd, UINT msg, WPARAM wParam, LPARAM lParam)
{
    if (ImGui_ImplWin32_WndProcHandler(hWnd, msg, wParam, lParam))
        return true;

    switch (msg)
    {
    case WM_SIZE:
        return 0;
    case WM_SYSCOMMAND:
        break;
    case WM_DESTROY:
        PostQuitMessage(0);
        return 0;
    }
    return DefWindowProc(hWnd, msg, wParam, lParam);
}
```
* ピクセルフォーマット
```
void CreateGlContext(HWND &_Hwnd,HDC &_HDC,HGLRC &_Hglrc)
{
	PIXELFORMATDESCRIPTOR pfd =
	{
		sizeof(PIXELFORMATDESCRIPTOR),
		1,
		PFD_DRAW_TO_WINDOW | PFD_SUPPORT_OPENGL | PFD_DOUBLEBUFFER,
		PFD_TYPE_RGBA,
		32,
		0, 0, 0, 0, 0, 0,
		0,
		0,
		0,
		0, 0, 0, 0,
		24,
		8, 
		0, 
		PFD_MAIN_PLANE,
		0,
		0, 0, 0
	};

	_HDC = GetDC(_Hwnd);   

    int pixelFormal = ChoosePixelFormat(_HDC, &pfd); 
    SetPixelFormat(_HDC,pixelFormal, &pfd);
    _Hglrc = wglCreateContext(_HDC);
    wglMakeCurrent(_HDC, _Hglrc);       
}
```
