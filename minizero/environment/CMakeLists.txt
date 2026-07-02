file(GLOB_RECURSE SRCS *.cpp)
# Atari needs ALE+OpenCV; board-game builds on a host without them exclude the Atari sources.
# Engine/MCTS/network/zero + all board games stay unmodified real MiniZero.
if(NOT GAME_TYPE STREQUAL "ATARI")
    list(FILTER SRCS EXCLUDE REGEX "/atari/")
endif()

add_library(environment ${SRCS})
target_include_directories(
    environment PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    base
    amazons
    atari
    breakthrough
    clobber
    conhex
    connect6
    dotsandboxes
    go
    gomoku
    havannah
    hex
    killallgo
    linesofaction
    nogo
    othello
    rubiks
    santorini
    surakarta
    tictactoe
    stochastic
    stochastic/puzzle2048
    stochastic/tetrisblockpuzzle
)
target_link_libraries(
    environment
    config
    utils
)
if(GAME_TYPE STREQUAL "ATARI")
    target_link_libraries(environment ale::ale-lib ${OpenCV_LIBS})
endif()
