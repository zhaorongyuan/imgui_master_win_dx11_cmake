cmake_minimum_required(VERSION 3.12)

# 项目名称
project(MyDx11Project CXX)

# 使用 C++14 标准
set(CMAKE_CXX_STANDARD 14)

# 定义 ImGui 的路径
set(IMGUI_DIR "imgui")

# 1. 核心源文件 (5个)
set(IMGUI_CORE_SOURCES
    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_demo.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
)

# 2. 后端源文件 (2个)
set(IMGUI_BACKEND_SOURCES
    ${IMGUI_DIR}/backends/imgui_impl_win32.cpp
    ${IMGUI_DIR}/backends/imgui_impl_dx11.cpp
)

# 3. 生成可执行文件
# 注意：加上 WIN32 参数，表示这是一个窗口程序，不会弹出黑色控制台
add_executable(MyApp main.cpp ${IMGUI_CORE_SOURCES} ${IMGUI_BACKEND_SOURCES})

# 4. 设置头文件路径
# 告诉编译器去哪里找 imgui.h 和 imgui_impl_win32.h
target_include_directories(MyApp PRIVATE 
    ${IMGUI_DIR} 
    ${IMGUI_DIR}/backends
)

# 5. 链接系统库 (关键步骤)
# 对应你日志里的 d3d11.lib d3dcompiler.lib
# dwmapi.lib 是用于支持视口特性(Viewports)的窗口管理
target_link_libraries(MyApp PRIVATE
    d3d11.lib
    d3dcompiler.lib
    dwmapi.lib
)

# 6. 开启 Unicode 支持
# 对应你日志里的 /D UNICODE /D _UNICODE
target_compile_definitions(MyApp PRIVATE UNICODE _UNICODE)