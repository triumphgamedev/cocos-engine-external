
set(CC_EXTERNAL_LIBS)
set(CC_EXTERNAL_INCLUDES ${CMAKE_CURRENT_LIST_DIR}/sources)
set(CC_EXTERNAL_PRIVATE_INCLUDES)
set(CC_EXTERNAL_PRIVATE_DEFINITIONS)

list(APPEND CC_EXTERNAL_PRIVATE_DEFINITIONS
    TBB_USE_EXCEPTIONS=0 # no rtti for now
)
MESSAGE(STATUS "platform path: ${platform_spec_path}")
include(${CMAKE_CURRENT_LIST_DIR}/cmake/CocosExternalConfig.cmake)
include(${CMAKE_CURRENT_LIST_DIR}/sources/CMakeLists.txt)
if(WINDOWS OR NX_WINDOWS)
    include(${CMAKE_CURRENT_LIST_DIR}/win64/CMakeLists.txt)
elseif(ANDROID)
    include(${CMAKE_CURRENT_LIST_DIR}/android/CMakeLists.txt)
elseif(APPLE)
    if(MACOSX)
        include(${CMAKE_CURRENT_LIST_DIR}/mac/CMakeLists.txt)
    elseif(IOS)
        # TRIUMPH PATCH: upstream expects you to hand-edit which of these two
        # lines is commented out to switch between device and Apple-Silicon
        # simulator prebuilts. We build BOTH slices to package an XCFramework,
        # so make it a flag: -DCC_IOS_SIMULATOR=ON.
        if(CC_IOS_SIMULATOR)
            include(${CMAKE_CURRENT_LIST_DIR}/ios-m1-simulator/CMakeLists.txt)
        else()
            include(${CMAKE_CURRENT_LIST_DIR}/ios/CMakeLists.txt)
        endif()
    endif()
elseif(OPENHARMONY)
    include(${CMAKE_CURRENT_LIST_DIR}/openharmony/CMakeLists.txt)
elseif(OHOS)
    include(${CMAKE_CURRENT_LIST_DIR}/ohos/CMakeLists.txt)
elseif(EMSCRIPTEN)
    include(${CMAKE_CURRENT_LIST_DIR}/emscripten/CMakeLists.txt)
elseif(LINUX)
    include(${CMAKE_CURRENT_LIST_DIR}/linux/CMakeLists.txt)
elseif(NX)
endif()
