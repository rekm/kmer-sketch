cmake_minimum_required(VERSION 3.25...3.30)
project(
    kmer_sketch
    VERSION 0.1.0
    DESCRIPTION "Fork of KoslickiLab kmer-sketch with library support"
    LANGUAGES CXX
)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(kmer_sketch_lib INTERFACE)

target_include_directories(kmer_sketch_lib
    INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

add_library(kmer_sketch::kmer_sketch ALIAS kmer_sketch_lib)


add_executable(kmer_sketch_sketch src/sketch_main.cpp)
target_link_libraries(kmer_sketch_sketch PRIVATE kmer_sketch_lib)
set_target_properties(kmer_sketch_sketch PROPERTIES OUTPUT_NAME kmer_sketch)

add_executable(kmer_sketch_filter src/filter_main.cpp)
target_link_libraries(kmer_sketch_filter PRIVATE kmer_sketch_lib)

add_executable(kmer_sketch_pwsimilarity src/pwsim_main.cpp)
target_link_libraries(kmer_sketch_pwsimilarity PRIVATE kmer_sketch_lib)

add_executable(kmer_sketch_expt_growth src/expt_growth.cpp)
target_link_libraries(kmer_sketch_expt_growth PRIVATE kmer_sketch_lib)


install(
    TARGETS
        kmer_sketch_sketch
        kmer_sketch_filter
        kmer_sketch_pwsimilarity
        kmer_sketch_expt_growth
    RUNTIME DESTINATION bin
)

install(
    DIRECTORY include/
    DESTINATION include
)