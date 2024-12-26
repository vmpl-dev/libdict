cmake_minimum_required(VERSION 3.10)

project(libdict VERSION 1.0.0 LANGUAGES C)

# 设置C标准
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

# 设置安装前缀（如果没有指定，则使用 /usr/local）
if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set(CMAKE_INSTALL_PREFIX "/usr/local" CACHE PATH "Installation prefix" FORCE)
endif()

# 包含 GNUInstallDirs
include(GNUInstallDirs)

# 添加编译选项
add_compile_options(-Wall -Wextra)

# 定义头文件列表
file(GLOB DICT_HEADERS
    "${PROJECT_SOURCE_DIR}/include/*.h"
    "${PROJECT_SOURCE_DIR}/include/dict/*.h"
)

# 源文件
file(GLOB SOURCES 
    "src/*.c"
)

# 头文件目录
include_directories(
    ${PROJECT_SOURCE_DIR}/include
)

# 创建共享库
add_library(dict SHARED ${SOURCES})

# 设置版本号
set_target_properties(dict PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION 1
)

# 安装规则
install(TARGETS dict
    COMPONENT runtime
    LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}
)

install(FILES ${DICT_HEADERS}
    COMPONENT devel
    DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_INCLUDEDIR}/dict
)

# 设置组件
set(CPACK_DEB_COMPONENT_INSTALL ON)
set(CPACK_COMPONENTS_GROUPING IGNORE)

# 设置包名称
set(CPACK_PACKAGE_NAME "libdict")
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION}")

# 添加包维护者信息
set(CPACK_PACKAGE_CONTACT "DUNE Team <dune@example.com>")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "DUNE Team <dune@example.com>")

# 设置组件包名称
set(CPACK_DEBIAN_RUNTIME_PACKAGE_NAME "libdict")
set(CPACK_DEBIAN_DEVEL_PACKAGE_NAME "libdict-dev")

# 设置文件名格式
set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}-${PROJECT_VERSION}-Linux")

# 设置包依赖
set(CPACK_DEBIAN_RUNTIME_PACKAGE_DEPENDS "libc6 (>= 2.3.1-6)")
set(CPACK_DEBIAN_DEVEL_PACKAGE_DEPENDS "libdict (= ${PROJECT_VERSION})")

# 设置包描述
set(CPACK_COMPONENT_RUNTIME_DESCRIPTION "Dictionary runtime library")
set(CPACK_COMPONENT_DEVEL_DESCRIPTION "Dictionary development files")

# 明确指定组件
set(CPACK_COMPONENTS_ALL runtime devel)

# 设置组件安装规则
install(TARGETS dict
    COMPONENT runtime
    LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}
)

install(FILES ${DICT_HEADERS}
    COMPONENT devel
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/dict
)

# 包含 CPack
include(CPack) 