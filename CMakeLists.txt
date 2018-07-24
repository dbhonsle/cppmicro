cmake_minimum_required (VERSION 2.6)
project (Restclient)

if(POLICY CMP0012)
  cmake_policy(SET CMP0012 NEW)
endif()

set(OPENSSL_ROOT_DIR "/usr/local/opt/openssl")

find_package(OpenSSL REQUIRED)
find_package(Boost REQUIRED COMPONENTS system thread log program_options chrono)
find_package(cpprestsdk REQUIRED)

file(GLOB microsvc_SRC
    "*.hpp"
    "*.cpp"
	"foundation/include/*.hpp"
    "foundation/*.cpp"
)
if(MSVC)
	add_compile_options(/W4)
else()
	add_compile_options(-Wall -Wextra -Wpedantic -DBOOST_LOG_DYN_LINK)
endif()
add_executable(Restsvc ${microsvc_SRC})
target_include_directories(Restsvc PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/foundation/include)
target_link_libraries(Restsvc cpprestsdk::cpprest ${Boost_LIBRARIES})
target_compile_features(Restsvc PRIVATE cxx_lambda_init_captures)
install (TARGETS Restsvc DESTINATION bin)
