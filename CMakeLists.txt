
cmake_minimum_required(VERSION 2.8)
set(OpenCV_DIR ~/tools/opencv/share/OpenCV)
project( ShotDetect )

add_executable(ShotDetect shotdetect.cc)
add_library(FFMPEGVideo ffmpegvideosource.cpp)
find_package( OpenCV 3.1 REQUIRED )
target_link_libraries(ShotDetect FFMPEGVideo)
target_link_libraries(ShotDetect ${OpenCV_LIBS} )

target_link_libraries(ShotDetect avcodec avformat swscale avutil)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fopenmp")
