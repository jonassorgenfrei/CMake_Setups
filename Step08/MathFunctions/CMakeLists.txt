# include a precomputed table for log and exp functions
add_executable(MakeTable MakeTable.cxx)

# specify how to produce table.h
add_custom_command(
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/Table.h
  COMMAND MakeTable ${CMAKE_CURRENT_BINARY_DIR}/Table.h
  DEPENDS MakeTable
  )

# mysqrt.cxx depends on the generated file Table.h
add_library(MathFunctions mysqrt.cxx ${CMAKE_CURRENT_BINARY_DIR}/Table.h)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
# state that we depend on Tutorial_BINARY_DIR but consumers don't, as the
# TutorialConfig.h include is an implementation detail
# state that we depend on our binary dir to find Table.h
target_include_directories(MathFunctions
			   INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
			   PRIVATE ${CMAKE_CURRENT_BINARY_DIR}
			   )

install(TARGETS MathFunctions DESTINATION lib COMPONENT libraries)
install(FILES MathFunctions.h DESTINATION include COMPONENT headers)
