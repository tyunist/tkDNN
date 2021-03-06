cmake_minimum_required(VERSION 3.5)

project (tkDNN)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -fPIC -Wno-deprecated-declarations -Wno-unused-variable")
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include/tkDNN)

# project specific flags
if(DEBUG)
    add_definitions(-DDEBUG)
endif()


#-------------------------------------------------------------------------------
# CUDA
#-------------------------------------------------------------------------------
find_package(CUDA 9.0 REQUIRED)
SET(CUDA_SEPARABLE_COMPILATION ON)
#set(CUDA_NVCC_FLAGS "${CUDA_NVCC_FLAGS} -arch=sm_30 --compiler-options '-fPIC'")
set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS} --maxrregcount=32)

find_package(CUDNN REQUIRED)

# compile
file(GLOB tkdnn_CUSRC "src/kernels/*.cu" "src/sorting.cu")
cuda_include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include ${CUDA_INCLUDE_DIRS} ${CUDNN_INCLUDE_DIRS})
cuda_add_library(kernels SHARED ${tkdnn_CUSRC})


#-------------------------------------------------------------------------------
# External Libraries
#-------------------------------------------------------------------------------
find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})

find_package(OpenCV REQUIRED)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DOPENCV")

find_package(yaml-cpp REQUIRED)

#-------------------------------------------------------------------------------
# Build Libraries
#-------------------------------------------------------------------------------
file(GLOB tkdnn_SRC "src/*.cpp")
set(tkdnn_LIBS kernels ${CUDA_LIBRARIES} ${CUDA_CUBLAS_LIBRARIES} ${CUDNN_LIBRARIES} ${OpenCV_LIBS} yaml-cpp)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include ${CUDA_INCLUDE_DIRS} ${OPENCV_INCLUDE_DIRS} ${NVINFER_INCLUDES})
add_library(tkDNN SHARED ${tkdnn_SRC})
target_link_libraries(tkDNN ${tkdnn_LIBS} nvinfer_plugin)

#static 
#add_library(tkDNN_static STATIC ${tkdnn_SRC})
#target_link_libraries(tkDNN_static ${tkdnn_LIBS})

add_executable(test_simple tests/simple/test_simple.cpp)
target_link_libraries(test_simple tkDNN)

add_executable(test_mnist tests/mnist/test_mnist.cpp)
target_link_libraries(test_mnist tkDNN)

add_executable(test_mnistRT tests/mnist/test_mnistRT.cpp)
target_link_libraries(test_mnistRT tkDNN)

## YOLO NETS
add_executable(test_yolo tests/yolo/yolo.cpp)
target_link_libraries(test_yolo tkDNN)

add_executable(test_yolo_voc tests/yolo_voc/yolo_voc.cpp)
target_link_libraries(test_yolo_voc tkDNN)

add_executable(test_yolo_tiny tests/yolo_tiny/yolo_tiny.cpp)
target_link_libraries(test_yolo_tiny tkDNN)

add_executable(test_yolo_relu tests/yolo_relu/yolo_relu.cpp)
target_link_libraries(test_yolo_relu tkDNN)


add_executable(test_yolo_224 tests/yolo_224/yolo_224.cpp)
target_link_libraries(test_yolo_224 tkDNN)

add_executable(test_yolo_berkeley tests/yolo_berkeley/yolo_berkeley.cpp)
target_link_libraries(test_yolo_berkeley tkDNN)

add_executable(test_yolo3_coco4 tests/yolo3_coco4/yolo3_coco4.cpp)
target_link_libraries(test_yolo3_coco4 tkDNN)

add_executable(test_yolo3 tests/yolo3/yolo3.cpp)
target_link_libraries(test_yolo3 tkDNN)

add_executable(test_yolo3_512 tests/yolo3_512/yolo3_512.cpp)
target_link_libraries(test_yolo3_512 tkDNN)

add_executable(test_yolo3_512tp tests/yolo3_512tp/yolo3_512tp.cpp)
target_link_libraries(test_yolo3_512tp tkDNN)

add_executable(test_yolo3_tiny tests/yolo3_tiny/yolo3_tiny.cpp)
target_link_libraries(test_yolo3_tiny tkDNN)

add_executable(test_yolo3_tiny512 tests/yolo3_tiny512/yolo3_tiny512.cpp)
target_link_libraries(test_yolo3_tiny512 tkDNN)

add_executable(test_yolo3_tinyNM512 tests/yolo3_tinyNM512/yolo3_tinyNM512.cpp)
target_link_libraries(test_yolo3_tinyNM512 tkDNN)

add_executable(test_yolo3_tiny512tp tests/yolo3_tiny512tp/yolo3_tiny512tp.cpp)
target_link_libraries(test_yolo3_tiny512tp tkDNN)

add_executable(test_yolo3_berkeley tests/yolo3_berkeley/yolo3_berkeley.cpp)
target_link_libraries(test_yolo3_berkeley tkDNN)

add_executable(test_yolo3_flir tests/yolo3_flir/yolo3_flir.cpp)
target_link_libraries(test_yolo3_flir tkDNN)

add_executable(test_yolo4 tests/yolo4/yolo4.cpp)
target_link_libraries(test_yolo4 tkDNN)

add_executable(test_mobilenetv2ssd tests/mobilenetv2ssd/mobilenetv2ssd.cpp)
target_link_libraries(test_mobilenetv2ssd tkDNN)

add_executable(test_bdd-mobilenetv2ssd tests/bdd-mobilenetv2ssd/bdd-mobilenetv2ssd.cpp)
target_link_libraries(test_bdd-mobilenetv2ssd tkDNN)

add_executable(test_mobilenetv2ssd512 tests/mobilenetv2ssd512/mobilenetv2ssd512.cpp)
target_link_libraries(test_mobilenetv2ssd512 tkDNN)

add_executable(test_resnet101 tests/resnet101/resnet101.cpp)
target_link_libraries(test_resnet101 tkDNN)

add_executable(test_csresnext50-panet-spp tests/csresnext50-panet-spp/csresnext50-panet-spp.cpp)
target_link_libraries(test_csresnext50-panet-spp tkDNN)

add_executable(test_bdd-csresnext50-panet-spp tests/bdd-csresnext50-panet-spp/bdd-csresnext50-panet-spp.cpp)
target_link_libraries(test_bdd-csresnext50-panet-spp tkDNN)

add_executable(test_resnet101_cnet tests/resnet101_cnet/resnet101_cnet.cpp)
target_link_libraries(test_resnet101_cnet tkDNN)

add_executable(test_dla34 tests/dla34/dla34.cpp)
target_link_libraries(test_dla34 tkDNN)

add_executable(test_dla34_cnet tests/dla34_cnet/dla34_cnet.cpp)
target_link_libraries(test_dla34_cnet tkDNN)

add_executable(test_imuodom tests/imuodom/imuodom.cpp)
target_link_libraries(test_imuodom tkDNN)
################################################################################


add_executable(test_rtinference tests/test_rtinference/rtinference.cpp)
target_link_libraries(test_rtinference tkDNN)

add_executable(map_demo demo/demo/map.cpp)
target_link_libraries(map_demo tkDNN)

add_executable(demo demo/demo/demo.cpp)
target_link_libraries(demo tkDNN)

#-------------------------------------------------------------------------------
# Install
#-------------------------------------------------------------------------------
#if (CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
#    set (CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" 
#         CACHE PATH "default install path" FORCE)
#endif()
message("install dir:" ${CMAKE_INSTALL_PREFIX})
install(DIRECTORY include/ DESTINATION include/)
install(TARGETS tkDNN kernels DESTINATION lib)
install(DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/cmake/" # source directory
        DESTINATION "share/tkDNN/cmake/" # target directory
)


#-------------------------------------------------------------------------------
# Prepare for test (not needed anymore)
#-------------------------------------------------------------------------------
#set(TEST_DATA true CACHE BOOL "If true download deps")
#if( ${TEST_DATA} )
#	message("Launching pre-build dependency installer script...")
#
#    execute_process (COMMAND bash -c "bash build_models.sh download"
#        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/tests)
#
#	set(TEST_DATA false CACHE BOOL "If true download deps" FORCE)
#	message("Finished dowloading test weights")
#endif()

