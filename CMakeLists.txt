cmake_minimum_required(VERSION 3.6)
project(TFTraj)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -D_GLIBCXX_USE_CXX11_ABI=0")

if(NOT EXISTS ${Tensorflow_INCLUDE_DIR})
    message(STATUS "Trying to find tensorflow")
    execute_process(
            COMMAND python -c "import tensorflow as tf; print(tf.sysconfig.get_include())"
            RESULT_VARIABLE Tensorflow_RESULT
            OUTPUT_VARIABLE Tensorflow_INCLUDE_DIR
            ERROR_VARIABLE Tensorflow_ERROR
    )

    if(${Tensorflow_RESULT})
        message(SEND_ERROR ${Tensorflow_ERROR})
    endif()

    set(Tensorflow_INCLUDE_DIR ${Tensorflow_INCLUDE_DIR} CACHE PATH "Tensorflow include dir")
endif()

execute_process(
    COMMAND python -c "import tensorflow as tf; print(tf.sysconfig.get_lib(), end='')"
    OUTPUT_VARIABLE TF_LINK_DIR
)

set(Tensorflow_LINK_DIR ${TF_LINK_DIR})

add_subdirectory(tftraj/rmsd)

