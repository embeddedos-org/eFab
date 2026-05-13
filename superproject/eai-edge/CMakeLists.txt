# eFab eai-edge superproject
#
# Pulls eNI + eIPC + eAI via CMake FetchContent at the version pins
# declared in ../../manifests/eai-edge.yml. The pins are exposed as
# cache variables so a CI matrix or downstream consumer can override
# them without editing this file (e.g. -DEFAB_PIN_EAI=master).
#
# This file is intentionally tiny — eFab is a manifest, not a product.
# All real build logic lives in the child repos' own CMakeLists.txt.

cmake_minimum_required(VERSION 3.20)

project(eFab_eai_edge
        LANGUAGES C CXX
        VERSION   0.1.0
        DESCRIPTION "EmbeddedOS — eai-edge profile (ENI + EIPC + eAI superproject)"
        HOMEPAGE_URL "https://embeddedos-org.github.io/stacks/eai-edge.html")

# ---------------------------------------------------------------------------
# Version pins (defaults match manifests/eai-edge.yml)
# ---------------------------------------------------------------------------

set(EFAB_PIN_ENI  "v0.1.0" CACHE STRING "eNI tag/branch to fetch")
set(EFAB_PIN_EIPC "v0.1.0" CACHE STRING "eIPC tag/branch to fetch")
set(EFAB_PIN_EAI  "v0.1.0" CACHE STRING "eAI tag/branch to fetch")

option(EFAB_BUILD_TESTS "Build the eai-edge integration smoke test driver" ON)

# ---------------------------------------------------------------------------
# Fetch the canonical product repos
# ---------------------------------------------------------------------------

include(FetchContent)

set(FETCHCONTENT_QUIET FALSE)

FetchContent_Declare(
  eNI
  GIT_REPOSITORY https://github.com/embeddedos-org/eNI.git
  GIT_TAG        ${EFAB_PIN_ENI}
  GIT_SHALLOW    TRUE
)

FetchContent_Declare(
  eIPC
  GIT_REPOSITORY https://github.com/embeddedos-org/eIPC.git
  GIT_TAG        ${EFAB_PIN_EIPC}
  GIT_SHALLOW    TRUE
)

FetchContent_Declare(
  eAI
  GIT_REPOSITORY https://github.com/embeddedos-org/eAI.git
  GIT_TAG        ${EFAB_PIN_EAI}
  GIT_SHALLOW    TRUE
)

# Realise all three at once. Each child repo's CMakeLists.txt continues to
# own its own targets, options, and tests.
FetchContent_MakeAvailable(eNI eIPC eAI)

# ---------------------------------------------------------------------------
# Tiny convenience target so consumers can drive the whole stack with one
# `cmake --build build --target eai_edge` invocation.
# ---------------------------------------------------------------------------

add_custom_target(eai_edge
  COMMENT "eFab :: built eNI ${EFAB_PIN_ENI} + eIPC ${EFAB_PIN_EIPC} + eAI ${EFAB_PIN_EAI}"
)

if(TARGET eni)
  add_dependencies(eai_edge eni)
endif()
if(TARGET eipc)
  add_dependencies(eai_edge eipc)
endif()
if(TARGET eai)
  add_dependencies(eai_edge eai)
endif()

# ---------------------------------------------------------------------------
# Smoke test wiring (driven by Python — see tests/eai-edge/smoke_test.py).
# ---------------------------------------------------------------------------

if(EFAB_BUILD_TESTS)
  enable_testing()
  find_package(Python3 COMPONENTS Interpreter)
  if(Python3_Interpreter_FOUND)
    add_test(
      NAME    eai_edge.smoke
      COMMAND ${Python3_EXECUTABLE}
              ${CMAKE_CURRENT_SOURCE_DIR}/../../tests/eai-edge/smoke_test.py
              --build-dir ${CMAKE_BINARY_DIR}
    )
    set_tests_properties(eai_edge.smoke PROPERTIES TIMEOUT 60)
  else()
    message(WARNING
      "Python3 not found — skipping eai_edge.smoke. Install python3 or pass "
      "-DEFAB_BUILD_TESTS=OFF to silence this warning.")
  endif()
endif()

message(STATUS "eFab :: eai-edge configured with eNI=${EFAB_PIN_ENI} eIPC=${EFAB_PIN_EIPC} eAI=${EFAB_PIN_EAI}")
