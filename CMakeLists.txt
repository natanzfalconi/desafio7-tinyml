cmake_minimum_required(VERSION 3.13)
include(pico_sdk_import.cmake)
project(desafio7_tinyml C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 14)

pico_sdk_init()

add_executable(main main.cpp)

# Avisa onde estao as pastas da IA
target_include_directories(main PRIVATE
    .
    edge-impulse-sdk
    edge-impulse-sdk/classifier
    edge-impulse-sdk/porting
    edge-impulse-sdk/dsp
    edge-impulse-sdk/dsp/kissfft
    edge-impulse-sdk/dsp/dct
    edge-impulse-sdk/third_party/ruy
    edge-impulse-sdk/third_party/flatbuffers/include
    edge-impulse-sdk/third_party/gemmlowp
    model-parameters
    tflite-model
)

# Captura todos os arquivos de IA matematicos automaticamente
file(GLOB_RECURSE EI_SOURCES
    edge-impulse-sdk/dsp/kissfft/*.cpp
    edge-impulse-sdk/dsp/kissfft/*.c
    edge-impulse-sdk/dsp/dct/*.cpp
    edge-impulse-sdk/classifier/*.cpp
    edge-impulse-sdk/porting/*.cpp
    tflite-model/*.cpp
    tflite-model/*.c
)

target_sources(main PRIVATE ${EI_SOURCES})
target_link_libraries(main pico_stdlib hardware_i2c hardware_gpio)
pico_add_extra_outputs(main)
