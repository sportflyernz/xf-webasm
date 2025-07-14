cmake_minimum_required(VERSION 3.16)
project(XFormsWasm CXX)


# Check if Emscripten toolchain is being used
if(CMAKE_TOOLCHAIN_FILE MATCHES "Emscripten.cmake")
    message(STATUS "Building for WebAssembly with Emscripten")
    set(CMAKE_CXX_STANDARD 17) # Or your preferred C++ standard
    set(CMAKE_CXX_STANDARD_REQUIRED ON)

    # Add Emscripten-specific flags
    # -s EXPORT_ES6=1 for ES6 module output (modern JS integration)
    # -s MODULARIZE=1 for a cleaner module structure
    # -s WASM=1 ensures WASM output
    # -s MALLOC=emmalloc or dlmalloc for memory management
    # -s EXPORTED_RUNTIME_METHODS for specific runtime functions
    # -s ALLOW_MEMORY_GROWTH=1 if your memory needs to expand
    set(EMSCRIPTEN_FLAGS
        "-s" "EXPORT_ES6=1"
        "-s" "MODULARIZE=1"
        "-s" "WASM=1"
        "-s" "MALLOC=emmalloc" # or dlmalloc
        "-s" "ALLOW_MEMORY_GROWTH=1"
        "-s" "ASYNCIFY" # If you need synchronous C++ functions to wait on async JS
    )
    # Add exported functions/symbols
    # e.g., -s EXPORTED_FUNCTIONS=_myCppFunction -s EXPORT_NAME=MyXFormsModule
    # Or use EMSCRIPTEN_KEEPALIVE in C++ code

    add_executable(xforms_wasm ${XFORMS_SOURCE_FILES}) # Define your source files

    # Link with Emscripten libraries (if needed)
    # target_link_libraries(xforms_wasm PRIVATE ${EMSCRIPTEN_LIBRARIES})

    # Add custom command to copy generated WASM/JS and HTML to web directory
    add_custom_command(
        TARGET xforms_wasm POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy "$<TARGET_FILE:xforms_wasm>" "${CMAKE_CURRENT_SOURCE_DIR}/web/xforms_wasm.wasm"
        COMMAND ${CMAKE_COMMAND} -E copy "$<TARGET_FILE_DIR:xforms_wasm>/xforms_wasm.js" "${CMAKE_CURRENT_SOURCE_DIR}/web/xforms_wasm.js"
        COMMENT "Copying WebAssembly output to web directory"
    )

else()
    message(STATUS "Building for Native")
    # Native build settings (e.g., for testing on desktop)
    add_executable(xforms_native ${XFORMS_SOURCE_FILES})
    # Add platform-specific libraries/settings here
endif()

# Define source files for both targets (or use target_sources)
# For simplicity, assuming all sources are in the main project directory
file(GLOB XFORMS_SOURCE_FILES "src/*.cpp" "src/*.h")
