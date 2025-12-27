cmake_minimum_required(VERSION 3.10)
project(cuda-neural-network LANGUAGES CUDA CXX)

set(CMAKE_CUDA_STANDARD 14)
set(CMAKE_CUDA_STANDARD_REQUIRED ON)

# Set CUDA architectures (RTX 4070 = sm_86/sm_89)
set(CMAKE_CUDA_ARCHITECTURES 86)

# Source files (excluding main.cu for test target)
set(SOURCES
    cuda-neural-network/src/neural_network.cu
    cuda-neural-network/src/coordinates_dataset.cu
    cuda-neural-network/src/layers/linear_layer.cu
    cuda-neural-network/src/layers/relu_activation.cu
    cuda-neural-network/src/layers/sigmoid_activation.cu
    cuda-neural-network/src/nn_utils/bce_cost.cu
    cuda-neural-network/src/nn_utils/matrix.cu
    cuda-neural-network/src/nn_utils/shape.cu
)

# Main source files for main executable
set(MAIN_SOURCES
    cuda-neural-network/src/main.cu
    ${SOURCES}
)

# Main executable
add_executable(nn_main ${MAIN_SOURCES})

target_include_directories(nn_main PRIVATE
    cuda-neural-network/src/
)

# Test executable
add_executable(nn_test
    ${SOURCES}
    cuda-neural-network-test/test/all_tests.cu
    cuda-neural-network-test/test/test_utils.cu
    cuda-neural-network-test/googletest-lib/gtest/gtest-all.cc
)

target_include_directories(nn_test PRIVATE
    cuda-neural-network/src/
    cuda-neural-network-test/test/
    cuda-neural-network-test/googletest-lib/
)
