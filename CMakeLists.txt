cmake_minimum_required(VERSION 3.21)

include(${CMAKE_CURRENT_LIST_DIR}/tools/rp6502.cmake)

find_package(Python3 REQUIRED COMPONENTS Interpreter)

add_custom_command(
    OUTPUT background.bin background_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        bitmap 4 ${CMAKE_CURRENT_SOURCE_DIR}/img/background.png
        background.bin background_pal.bin
    DEPENDS img/png2bin.py img/background.png
    VERBATIM
)

add_custom_command(
    OUTPUT splash.bin splash_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        bitmap 4 ${CMAKE_CURRENT_SOURCE_DIR}/img/splash.png
        splash.bin splash_pal.bin
    DEPENDS img/png2bin.py img/splash.png
    VERBATIM
)

add_custom_command(
    OUTPUT crosshair.bin crosshair_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        sprite 1 32x32 ${CMAKE_CURRENT_SOURCE_DIR}/img/crosshair.png
        crosshair.bin crosshair_pal.bin
    DEPENDS img/png2bin.py img/crosshair.png
    VERBATIM
)

add_custom_command(
    OUTPUT target.bin target_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        sprite 4 16x16 ${CMAKE_CURRENT_SOURCE_DIR}/img/target.png
        target.bin target_pal.bin
    DEPENDS img/png2bin.py img/target.png
    VERBATIM
)

add_custom_command(
    OUTPUT target_boom.bin target_boom_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        sprite 1 16x16 ${CMAKE_CURRENT_SOURCE_DIR}/img/target_boom.png
        target_boom.bin target_boom_pal.bin
    DEPENDS img/png2bin.py img/target_boom.png
    VERBATIM
)

add_custom_command(
    OUTPUT game_over1.bin game_over1_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        sprite 4 56x64 ${CMAKE_CURRENT_SOURCE_DIR}/img/game_over1.png
        game_over1.bin game_over1_pal.bin
    DEPENDS img/png2bin.py img/game_over1.png
    VERBATIM
)

add_custom_command(
    OUTPUT game_over2.bin game_over2_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        sprite 4 56x64 ${CMAKE_CURRENT_SOURCE_DIR}/img/game_over2.png
        game_over2.bin game_over2_pal.bin
    DEPENDS img/png2bin.py img/game_over2.png
    VERBATIM
)

add_custom_command(
    OUTPUT shell.bin shell_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        sprite 4 8x16 ${CMAKE_CURRENT_SOURCE_DIR}/img/shell.png
        shell.bin shell_pal.bin
    DEPENDS img/png2bin.py img/shell.png
    VERBATIM
)

add_custom_command(
    OUTPUT hostage.bin hostage_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        sprite 4 16x32 ${CMAKE_CURRENT_SOURCE_DIR}/img/hostage.png
        hostage.bin hostage_pal.bin
    DEPENDS img/png2bin.py img/hostage.png
    VERBATIM
)

add_custom_command(
    OUTPUT bad_guy.bin bad_guy_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        sprite 4 32x32 ${CMAKE_CURRENT_SOURCE_DIR}/img/bad_guy.png
        bad_guy.bin bad_guy_pal.bin
    DEPENDS img/png2bin.py img/bad_guy.png
    VERBATIM
)

add_custom_command(
    OUTPUT digits.bin digits_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        sprite 1 8x8 ${CMAKE_CURRENT_SOURCE_DIR}/img/digits.png
        digits.bin digits_pal.bin
    DEPENDS img/png2bin.py img/digits.png
    VERBATIM
)

add_custom_command(
    OUTPUT diff_easy.bin diff_easy_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        sprite 2 48x12 ${CMAKE_CURRENT_SOURCE_DIR}/img/diff_easy.png
        diff_easy.bin diff_easy_pal.bin
    DEPENDS img/png2bin.py img/diff_easy.png
    VERBATIM
)

add_custom_command(
    OUTPUT diff_medium.bin diff_medium_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        sprite 2 48x12 ${CMAKE_CURRENT_SOURCE_DIR}/img/diff_medium.png
        diff_medium.bin diff_medium_pal.bin
    DEPENDS img/png2bin.py img/diff_medium.png
    VERBATIM
)

add_custom_command(
    OUTPUT diff_hard.bin diff_hard_pal.bin
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/img/png2bin.py
        sprite 2 48x12 ${CMAKE_CURRENT_SOURCE_DIR}/img/diff_hard.png
        diff_hard.bin diff_hard_pal.bin
    DEPENDS img/png2bin.py img/diff_hard.png
    VERBATIM
)

if(RP6502_BASIC)
    project(MY-RP6502-PROJECT BASIC)
    add_executable(target_practice)
    rp6502_asset(target_practice help src/help.txt)
    rp6502_asset(target_practice target_practice.bas src/main.bas)
    rp6502_basic(target_practice target_practice.bas)
    rp6502_web(target_practice)
else()
    project(MY-RP6502-PROJECT C CXX ASM)
    add_executable(target_practice)
    rp6502_map(target_practice src/xram.h "XRAM_.*")
    rp6502_asset(target_practice help src/help.txt)
    rp6502_asset(target_practice background.bin ${CMAKE_CURRENT_BINARY_DIR}/background.bin)
    rp6502_asset(target_practice splash.bin ${CMAKE_CURRENT_BINARY_DIR}/splash.bin)
    rp6502_asset(target_practice XRAM(XRAM_BACKGROUND_PALETTE) ${CMAKE_CURRENT_BINARY_DIR}/background_pal.bin)
    rp6502_asset(target_practice XRAM(XRAM_SPLASH_PALETTE) ${CMAKE_CURRENT_BINARY_DIR}/splash_pal.bin)
    rp6502_asset(target_practice XRAM(XRAM_DIFF_PALETTE) ${CMAKE_CURRENT_BINARY_DIR}/diff_easy_pal.bin)
    rp6502_asset(target_practice XRAM(XRAM_DIFF_EASY) ${CMAKE_CURRENT_BINARY_DIR}/diff_easy.bin)
    rp6502_asset(target_practice XRAM(XRAM_DIFF_MEDIUM) ${CMAKE_CURRENT_BINARY_DIR}/diff_medium.bin)
    rp6502_asset(target_practice XRAM(XRAM_DIFF_HARD) ${CMAKE_CURRENT_BINARY_DIR}/diff_hard.bin)

    rp6502_asset(target_practice XRAM(XRAM_CROSSHAIR_PALETTE) ${CMAKE_CURRENT_BINARY_DIR}/crosshair_pal.bin)
    rp6502_asset(target_practice XRAM(XRAM_CROSSHAIR) ${CMAKE_CURRENT_BINARY_DIR}/crosshair.bin)
    rp6502_asset(target_practice XRAM(XRAM_TARGET_PALETTE) ${CMAKE_CURRENT_BINARY_DIR}/target_pal.bin)
    rp6502_asset(target_practice XRAM(XRAM_TARGET) ${CMAKE_CURRENT_BINARY_DIR}/target.bin)
    rp6502_asset(target_practice XRAM(XRAM_TARGET_BOOM_PALETTE) ${CMAKE_CURRENT_BINARY_DIR}/target_boom_pal.bin)
    rp6502_asset(target_practice XRAM(XRAM_TARGET_BOOM) ${CMAKE_CURRENT_BINARY_DIR}/target_boom.bin)
    rp6502_asset(target_practice XRAM(XRAM_GAME_OVER1_PALETTE) ${CMAKE_CURRENT_BINARY_DIR}/game_over1_pal.bin)
    rp6502_asset(target_practice XRAM(XRAM_GAME_OVER1) ${CMAKE_CURRENT_BINARY_DIR}/game_over1.bin)
    rp6502_asset(target_practice XRAM(XRAM_GAME_OVER2) ${CMAKE_CURRENT_BINARY_DIR}/game_over2.bin)
    rp6502_asset(target_practice XRAM(XRAM_SHELL_PALETTE) ${CMAKE_CURRENT_BINARY_DIR}/shell_pal.bin)
    rp6502_asset(target_practice XRAM(XRAM_SHELL) ${CMAKE_CURRENT_BINARY_DIR}/shell.bin)
    rp6502_asset(target_practice XRAM(XRAM_HOSTAGE_PALETTE) ${CMAKE_CURRENT_BINARY_DIR}/hostage_pal.bin)
    rp6502_asset(target_practice XRAM(XRAM_HOSTAGE) ${CMAKE_CURRENT_BINARY_DIR}/hostage.bin)
    rp6502_asset(target_practice XRAM(XRAM_BAD_GUY_PALETTE) ${CMAKE_CURRENT_BINARY_DIR}/bad_guy_pal.bin)
    rp6502_asset(target_practice XRAM(XRAM_BAD_GUY) ${CMAKE_CURRENT_BINARY_DIR}/bad_guy.bin)
    rp6502_asset(target_practice XRAM(XRAM_DIGITS_PALETTE) ${CMAKE_CURRENT_BINARY_DIR}/digits_pal.bin)
    rp6502_asset(target_practice XRAM(XRAM_DIGITS) ${CMAKE_CURRENT_BINARY_DIR}/digits.bin)
    rp6502_executable(target_practice DATA default RESET default)
    rp6502_web(target_practice)
    target_sources(target_practice PRIVATE
        src/main.c
        src/sound.c
        src/splash.c
    )
endif()
