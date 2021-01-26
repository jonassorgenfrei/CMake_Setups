# add the library that runs
add_library(MathFunctions MathFunctions.cxx)
set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions
                           INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
                           )

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# SqrtLibrary that will be build & installed conditionally
# explicitly require that SqrtLibrary is built statically
if(USE_MYMATH)

  target_compile_definitions(MathFunctions PRIVATE "USE_MYMATH")

  # first we add the executable that generates the table
  add_executable(MakeTable MakeTable.cxx)

  # add the command to generate the source code
  add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/Table.h
    COMMAND MakeTable ${CMAKE_CURRENT_BINARY_DIR}/Table.h
    DEPENDS MakeTable
    ) 

  # if STATIC SHOULD BE BUILD
  # library that just does sqrt
  #add_library(SqrtLibrary STATIC
  #            mysqrt.cxx
  #            ${CMAKE_CURRENT_BINARY_DIR}/Table.h
  #            )
  
  # if SHARED SHOULD BE BUILD
  add_library(SqrtLibrary SHARED
              mysqrt.cxx
              ${CMAKE_CURRENT_BINARY_DIR}/Table.h
              )
              
  # state that we depend on our binary dir to find Table.h
  target_include_directories(SqrtLibrary PRIVATE
                             ${CMAKE_CURRENT_BINARY_DIR}
                             )

  # state that SqrtLibrary need Position Idependent Code when 
  # the default is shared libraries
  set_target_properties(SqrtLibrary PROPERTIES
                        POSITION_INDEPENDENT_CODE ${BUILD_SHARED_LIBS}
                        )
  # disable when build as shared library
  target_link_libraries(MathFunctions PUBLIC SqrtLibrary)
endif()

# define the symbol stating we are using the declspec(dllexport) when
# building on windows
target_compile_definitions(MathFunctions PRIVATE "EXPORTING_MYMATH")

# install rules
set(installable_libs MathFunctions)
if(TARGET SqrtLibrary)
  list(APPEND installable_libs SqrtLibrary)
endif()

# install rules
# if STATIC SHOULD BE BUILD
install(TARGETS ${installable_libs} DESTINATION bin COMPONENT lib)
# if SHARED SHOULD BE BUILD
#install(TARGETS ${installable_libs} DESTINATION lib COMPONENT lib)
install(FILES MathFunctions.h DESTINATION include COMPONENT include)