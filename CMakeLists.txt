cmake_minimum_required(VERSION 3.19)
IF(MSVC)
    SET(CMAKE_GENERATOR_PLATFORM win32)
ENDIF()

PROJECT(Projectfinal)

SET(CMAKE_CXX_STANDARD 17)

SET(CMAKE_DISABLE_IN_SOURCE_BUILD ON)
SET(CMAKE_SUPPRESS_REGENERATION ON)
SET(CMAKE_USE_RELATIVE_PATHS ON)


OPTION(GLFW_BUILD_DOCS OFF)
OPTION(GLFW_BUILD_EXAMPLES OFF)
OPTION(GLFW_BUILD_TESTS OFF)
OPTION(GLFW_INSTALL OFF)
ADD_SUBDIRECTORY(External/glfw)

option(ASSIMP_BUILD_ASSIMP_TOOLS OFF)
option(ASSIMP_BUILD_SAMPLES OFF)
option(ASSIMP_BUILD_TESTS OFF)
add_subdirectory(External/assimp)

#config glfw
INCLUDE_DIRECTORIES(External/glfw/include/)
LIST(APPEND LIBS glfw ${GLFW_LIBRARIES})

#config glad
SET(GLAD_SRC External/GL/glad/src/glad.c)
IF(NOT WIN32)
    LIST(APPEND LIBS dl)
ENDIF()
INCLUDE_DIRECTORIES(External/GL/glad/include)

#config assimp
INCLUDE_DIRECTORIES(External/assimp/include/)
LIST(APPEND LIBS assimp ${ASSIMP_LIBRARIES})
message(">>>>> APPEND: ASSIMP ${LIBS}")

#config glm
INCLUDE_DIRECTORIES(External/GL/glm/)

#config include imgui
INCLUDE_DIRECTORIES(External/Imgui/include)
SET(IMGUI_SRC External/Imgui/src)

#config lodepng
SET(LODEPNG_SRC External/lodepng/lodepng.cpp)
INCLUDE_DIRECTORIES(External/lodepng/)

#config include files
INCLUDE_DIRECTORIES(include/)
INCLUDE_DIRECTORIES(External/)
INCLUDE_DIRECTORIES(External/GL/)

#config stb_img
SET(IMG_SRC External/stb_image/stb_image.h)
INCLUDE_DIRECTORIES(External/stb_image/)

#config FileDialog
SET(DIALOG_SRC External/FileDialog/ImGuiFileDialog.cpp)
INCLUDE_DIRECTORIES(External/FileDialog/)
IF(MSVC)
INCLUDE_DIRECTORIES(External/FileDialog/dirent)
ENDIF()

#config sources files
FILE(GLOB EXTERNAL_SOURCES ${GLAD_SRC}
${LODEPNG_SRC}
${IMG_SRC}
${DIALOG_SRC})
FILE(GLOB PROJECT_IMGUI ${IMGUI_SRC}/*.cpp)
FILE(GLOB PROJECT_HEADERS include/*.h)
FILE(GLOB PROJECT_SOURCES src/*.cpp)
FILE(GLOB PROJECT_SHADERS res/shader/*.glsl)

SOURCE_GROUP("Headers" FILES ${PROJECT_HEADERS})
SOURCE_GROUP("Sources" FILES ${PROJECT_SOURCES})
SOURCE_GROUP("Imgui" FILES ${PROJECT_IMGUI})
SOURCE_GROUP("Externals" FILES ${EXTERNAL_SOURCES})
SOURCE_GROUP("Shaders" FILES ${PROJECT_SHADERS})

ADD_EXECUTABLE(${PROJECT_NAME} ${PROJECT_SOURCES} ${PROJECT_IMGUI} ${PROJECT_HEADERS} ${EXTERNAL_SOURCES} ${PROJECT_SHADERS})

TARGET_LINK_LIBRARIES(${PROJECT_NAME} ${LIBS})

