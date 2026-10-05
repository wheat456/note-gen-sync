# 项目目录

- app
	- main.c
	- src
	- inc
	- CMakeLists.txt
    ```cmake
            ​  
        set(APP_SOURCES  
            main.c  
            src/n32g45x_it.c  
        )  
        ​  
        add_executable(${PROJECT_NAME} ${APP_SOURCES})   
        target_include_directories(${PROJECT_NAME} PRIVATE  
        inc  
        )  
        target_link_libraries(${PROJECT_NAME}   
        bsp  
        )  
    ```
    ​ 
- bsp
    - CMSIS
    - driver
    - CMakeLists.txt​  
```
    file(GLOB DRIVER_SOURCES  
    "n32g45x_std_periph_driver/src/*.c"  
    ​  
    )  
    set(BSP_SOURCES  
    ${DRIVER_SOURCES}  
    "CMSIS/device/system_n32g45x.c"  
    "CMSIS/device/startup/startup_n32g45x_gcc.s"  
    )  
    ​  
    ​  
    add_library(bsp STATIC ${BSP_SOURCES})  
    ​  
    target_include_directories(bsp PUBLIC  
    CMSIS/core  
    CMSIS/device  
    n32g45x_std_periph_driver/inc  
    )  
    ​  
    target_compile_definitions(bsp PUBLIC  
        USE_STDPERIPH_DRIVER  
        N32G45X  
    )  
```


- cmake
	- xxx.cmake
- middlewares
- cmakelists.txt
```cmake
cmake_minimum_required(VERSION 3.22)  
project(n32g45x01 C ASM) # 这里的n32g45x01是项目名  
​  
​  
set(CMAKE_C_STANDARD 11)  
set(CMAKE_C_STANDARD_REQUIRED ON)  
​  
add_subdirectory(bsp) # 这里是文件夹名称  
add_subdirectory(app)
```

- CMakePresets.json