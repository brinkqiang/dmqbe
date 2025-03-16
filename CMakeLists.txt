cmake_minimum_required(VERSION 3.10)
project(qbe C)

set(CMAKE_C_STANDARD 99)

if (unix)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -Wall -Wextra -Wpedantic")
endif()

# 公共源文件
set(COMMON_SRC
    main.c
    util.c
    parse.c
    abi.c
    cfg.c
    mem.c
    ssa.c
    alias.c
    load.c
    copy.c
    fold.c
    gvn.c
    gcm.c
    simpl.c
    live.c
    spill.c
    rega.c
    emit.c
)

# 架构特定源文件
set(AMD64_SRC
    amd64/targ.c
    amd64/sysv.c
    amd64/isel.c
    amd64/emit.c
)

set(ARM64_SRC
    arm64/targ.c
    arm64/abi.c
    arm64/isel.c
    arm64/emit.c
)

set(RV64_SRC
    rv64/targ.c
    rv64/abi.c
    rv64/isel.c
    rv64/emit.c
)

# 所有源文件
set(SRC_ALL ${COMMON_SRC} ${AMD64_SRC} ${ARM64_SRC} ${RV64_SRC})

# 包含目录
include_directories(.)
include_directories(amd64)
include_directories(arm64)
include_directories(rv64)

# 生成可执行文件
add_executable(qbe ${SRC_ALL})

# 处理config.h
if(APPLE)
    if(CMAKE_SYSTEM_PROCESSOR MATCHES "arm64")
        set(DEFTGT "T_arm64_apple")
    else()
        set(DEFTGT "T_amd64_apple")
    endif()
else()
    if(CMAKE_SYSTEM_PROCESSOR MATCHES "aarch64|arm64")
        set(DEFTGT "T_arm64")
    elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "riscv64")
        set(DEFTGT "T_rv64")
    else()
        set(DEFTGT "T_amd64_sysv")
    endif()
endif()

configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/config.h.in
    ${CMAKE_CURRENT_BINARY_DIR}/config.h
)

# 安装目标
install(TARGETS qbe DESTINATION bin)
