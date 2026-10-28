cmake_minimum_required(VERSION 3.16)

project(PDB VERSION 0.9 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# ── Locate hog2 ───────────────────────────────────────────────────────────────
# Search ~/.local first (default hog2 install prefix), then standard paths.
list(PREPEND CMAKE_PREFIX_PATH "$ENV{HOME}/.local")
find_package(hog2 REQUIRED)

# ── PDB executable (headless; built from Driver.cpp against hog2-stub) ───────
add_executable(PDB Driver.cpp Driver.h)
target_link_libraries(PDB PRIVATE hog2::hog2-stub)
if(UNIX AND NOT APPLE)
    target_link_libraries(PDB PRIVATE pthread)
endif()

set_target_properties(PDB PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
)
