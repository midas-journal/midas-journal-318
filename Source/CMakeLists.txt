cmake_minimum_required(VERSION 2.4)

PROJECT( ScalarToFractalImageFilter )

#The following lines are required to use Dart
ENABLE_TESTING()
INCLUDE(Dart)

# Set up ITK
IF(USE_ITK)
  FIND_PACKAGE(ITK)
  IF(ITK_FOUND)
    INCLUDE(${ITK_USE_FILE})
  ELSE(ITK_FOUND)
    MESSAGE(FATAL_ERROR
      "Cannot build without ITK.  Please set ITK_DIR.")
  ENDIF(ITK_FOUND)
ENDIF(USE_ITK)

INCLUDE (${CMAKE_ROOT}/Modules/FindITK.cmake) 
IF (USE_ITK_FILE) 
  INCLUDE(${USE_ITK_FILE}) 
ENDIF(USE_ITK_FILE) 

#SET(IJ_INCLUDE_DIRS
#)

#INCLUDE_DIRECTORIES(${IJ_INCLUDE_DIRS})

LINK_DIRECTORIES(${ITK_LIBRARY_PATH})

IF(BUILD_TESTING)
# non-templated class -- this should be stored in a library and linked in...
ADD_EXECUTABLE(itkScalarToFractalImageFilterTest itkScalarToFractalImageFilterTest.cxx)
TARGET_LINK_LIBRARIES(itkScalarToFractalImageFilterTest ITKIO ITKNumerics ITKStatistics)

SET(CurrentExe "ImageCompare")
ADD_EXECUTABLE(${CurrentExe} ImageCompare.cxx) 
TARGET_LINK_LIBRARIES(${CurrentExe}  ITKIO ITKNumerics ITKStatistics)

ENDIF(BUILD_TESTING)

###
#  Perform testing
###

FOREACH( FILE 00 01 02 )
  ADD_TEST( TEST_0_${FILE} itkScalarToFractalImageFilterTest 2
          ${CMAKE_SOURCE_DIR}/textures/1.0.${FILE}.png 
          ${CMAKE_BINARY_DIR}/sf_1.0.${FILE}.nii.gz 1 )
  ADD_TEST( COMPARE_0_${FILE} ImageCompare
          ${CMAKE_BINARY_DIR}/sf_1.0.${FILE}.nii.gz 
          ${CMAKE_SOURCE_DIR}/Output/sf_1.0.${FILE}.nii.gz )
ENDFOREACH(FILE)

FOREACH( FILE 01 02 03 04 05 06 07 08 09 10 11 12 13 )
  ADD_TEST( TEST_1_${FILE} itkScalarToFractalImageFilterTest 2
          ${CMAKE_SOURCE_DIR}/textures/1.1.${FILE}.png 
          ${CMAKE_BINARY_DIR}/sf_1.1.${FILE}.nii.gz 2 )
  ADD_TEST( COMPARE_1_${FILE} ImageCompare
          ${CMAKE_BINARY_DIR}/sf_1.1.${FILE}.nii.gz 
          ${CMAKE_SOURCE_DIR}/Output/sf_1.1.${FILE}.nii.gz )
ENDFOREACH(FILE)

ADD_TEST( TEST_Lenna itkScalarToFractalImageFilterTest 2
        ${CMAKE_SOURCE_DIR}/textures/Lenna.png 
        ${CMAKE_BINARY_DIR}/sf_Lenna.nii.gz 3 )
