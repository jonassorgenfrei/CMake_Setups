add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})
# NOTE: INTERFACE means things that consumers require but the producer doesn't
install(TARGETS MathFunctions DESTINATION "${PROJECT_BINARY_DIR}/lib")
install(FILES MathFunctions.h DESTINATION "${PROJECT_BINARY_DIR}/inlcude")
